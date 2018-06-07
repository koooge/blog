---
title: "expressのエラーハンドラーは引数の数が４つじゃないとダメ"
date: 2018-03-13T14:33:30.000Z
draft: false
---

あけましておめでとうございます。本年もよろしくお願いいたします。(1/31)

今年は一か月に一回のポストが目標です。(1/31)

最近は仕事でnodeを書いており、サーバーはexpressを使っているのですが、documentをちゃんと読まないで痛い目見たという話を書きます。

## nodeの関数
まずnode(js)の特徴としてcallbackがあると思います。

```
function hoge(callback) {
  callback('ho', 'ge');
}

hoge((a, b) => {
  console.log(a); // ho
  console.log(b); // ge
});
```

このとき、nodeとしては、callbackに何個引数を渡してもOKです。

```
function hoge(callback) {
  callback('ho', 'ge', 'fuga');
}

hoge((a, b) => {
   console.log(a); // ho
   console.log(b); // ge
});


function hoge(callback) {
  callback('ho');
}

hoge((a, b) => {
   console.log(a); // ho
   console.log(b); // undefined
});
```

つまるところnodeの処理系には、Javaのようなオーバーロード(引数の違いで別の関数とみなす）の仕組みはないです。
また、当然ですが関数に渡された引数は未使用でもよいです。

```
function hoge(a, b) {
  console.log(a); // ho
  // bはhoge内で未使用
}

hoge('ho');
```

## eslint
nodeの静的解析ツールは、eslintを使用しています。このeslintには、未使用の変数を指摘するno-unused-var
[https://eslint.org/docs/rules/no-unused-vars]があります。
そしてこのルールはeslintの推奨設定です。
前述の例は下記のように指摘されます。`1:18 error 'b' is defined but never used no-unused-vars`

便利ですね。hogeはbは使用していないので省略して書けます。

```
function hoge(a) {
  console.log(a);
}
```

## express
さてepressのerror handlerの話。
express-generator [https://www.npmjs.com/package/express-generator]
を使ってひな形を作成した場合下記のエラーハンドラーが生成されます

```
// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});
```

これをeslintにかけると、使ってない変数が指摘されます。`36:33 error 'next' is defined but never used no-unused-vars`

よしじゃあnext消しますか、と、消しても一見普通に動きます。
が、あるときまったく関係ない部分でエラーがぼろぼろ出るようになりました。。

原因が全然わからず、エラー混入をコミットレベルまで絞った挙句原因分からず、困り果ててAPIドキュメント
[http://expressjs.com/en/4x/api.html]をみると、思いっきり書いてました。

```
Error-handling middleware always takes four arguments. You must provide four arguments to identify it as an error-handling middleware function. Even if you don’t need to use the next object, you must specify it to maintain the signature. Otherwise, the next object will be interpreted as regular middleware and will fail to handle errors.
```

訳：エラーハンドラーのコールバックに渡す引数の数は、絶対４つにしとけ

ええ。。こんなの知らないお。。。

実装はこのあたりみたいです。
https://github.com/expressjs/express/blob/master/lib/router/layer.js#L62-L68

個人的にはnodeで引数の数を指定するのはイマイチだと思います。が、ドキュメントは隅々まで読みましょう。。
eslintの設定で、no-used-varsのargsをnoneに設定すれば一応指摘は消せます。

あなかしこ