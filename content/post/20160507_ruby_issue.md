---
title: "Rubyの#13053を読んでみる(1)"
date: 2018-03-13T14:30:25.000Z
categories: ["ruby"]
draft: false
---

前回 [http://www.koooge.com/entry/2016/12/10/061440]のあと、しばらくしてsvn update
し直してからmakeしてみたところ無事rubyのビルドが成功しました。やはり単に最新のrubyを使うだけでいろんなバグが踏めそうです。（普通にrubyを使うにはそれが目的でないとは無いと思いますが）

Issue読んでみる
さて、引き続いて今回はissueを読んでみます。
まずは、既にクローズされたチケットから読んでみて、雰囲気を味わうことを目的にします。

#13053
これを見ていきます。
[https://bugs.ruby-lang.org/issues/13053:title]

選んだ理由はなんとなくです。朝通勤中に電車でメールを見てたら[ruby-core:78739] で流れてきたので選んでみました。
タイトルから見るにArray#select!((http://ref.xaio.jp/ruby/classes/array/select_bang
))の問題みたいです。

descriptionを読む
まずはチケットのdescriptionを確認

Since Ruby 2.3 ([Feature #10714]), the following code cause the Array to be nagative number size.

ary = [1,2,3,4,5]
ary.select! { |i| ary.clear if i==5; false }
p ary #=> []
p ary.size #=> -5


コードで説明しているissueは分かりやすくて助かります。ary.select! { |i| ary.clear if i==5; false }
は、配列をイテレートして、5だったら配列を空に、次行でブロックの戻り値をfalseとしています。
なんでこんなことすんの(´・ω・｀)って思いますが、
aryは空になっているにもかかわらず、ary.sizeが-5になのは明らかに変で、0になるべきです。

因みにこれをruby-2.2.6で実行すると

irb(main):001:0> ary = [1,2,3,4,5]
=> [1, 2, 3, 4, 5]
irb(main):002:0> ary.select! { |i| ary.clear if i==5; false }
=> []
irb(main):003:0> p ary
[]
=> []
irb(main):004:0> p ary.size
0
=> 0


ちゃんとary.sizeが0になっています。

修正内容をみる
revision 57121で修正されていますので、diff((
https://bugs.ruby-lang.org/projects/ruby-trunk/repository/revisions/57121
))を確認します。
rubyでは、svnとredmineの連携を使用されているので見やすく良いです。
diffから見るに、trunk直下のarray.cとそのユニットテストが更新されことが確認できます。

testには、先ほどdescriptionでみた問題コードが記述されています。
これ、確かにテストにはなると思いますが、なんでこのテストコードでいいんだろう。。

testの内容も気になりますが、とりあえず置いといて、array.cの修正をみます。
redmineのdiffが見づらいのでgithubで確認しました。
[
https://github.com/ruby/ruby/commit/b5da45d6d7435d42591bc5c65c9c015984f2e4b2:title
]

修正前コード

static VALUE
select_bang_ensure(VALUE a)
{
    volatile struct select_bang_arg *arg = (void *)a;
    VALUE ary = arg->ary;
    long len = RARRAY_LEN(ary);
    long i1 = arg->len[0], i2 = arg->len[1];

    if (i2 < i1) {
	if (i1 < len) {
	    RARRAY_PTR_USE(ary, ptr, {
		    MEMMOVE(ptr + i2, ptr + i1, VALUE, len - i1);
		});
	}
	ARY_SET_LEN(ary, len - i1 + i2);
    }
    return ary;
}


ふむ。まずselect_bang_ensure(VALUE
a)っていう関数がなんぞやって感じですが、関数名から推察するにselect!の処理ルーチンのようです。

引数のVALUEは、値へのポインタです。（このあたりはRubyのしくみ
[https://www.amazon.co.jp/gp/product/4274050653/ref=as_li_qf_sp_asin_il?ie=UTF8&camp=247&creative=1211&creativeASIN=4274050653&linkCode=as2&tag=koooge-22]
に書いてました)
select_bang_arg構造体のaryメンバに配列情報が入っているみたいで、。
RARRAY_LEN(ary)は、マクロ処理で配列の長さを取得できるようです。

long i1 = arg->len[0], i2 = arg->len[1];


のlen[0]とlen[1]がパッと見わからなかったのでselect_bang_arg構造体を見ます。

struct select_bang_arg {
    VALUE ary;
    long len[2];
};


なるほど。何も説明が無い！
勘となりますが、len[0]が配列のheadへのポインタで、len[1]が配列のtailへのポインタ(´・ω・｀)違う?

これは遡ってlenが何かを確認する必要がありそうです。
select_bang_ensure()は、rb_ary_select_bang()からrb_ensure()に関数ポインタを渡しているみたいなので、次回rb_ary_select_bang()から追ってみたいと思います。


--------------------------------------------------------------------------------

ここまで気になったこと
array
Arrayなんてとっくに枯れて安定しているものと思っていましたが、
これみたいに機能追加でデグレがあるんですね。
こういうところは貢献していきやすいのではないかと思いました。

array.cのtest
テストこれで過不足ないのか感。やっぱDone better than perfect.なんでしょうか。

修正の確認環境
redmineにあるコードをコピペしたら、改行コードの関係か１行が２行にずれる。
githubで修正を確認しましたが、他に楽に差分確認する方法あるんでしょうか？
Rubyコミッター方の作業環境(使用ツールとか使ってるwebサービスとか）に興味ある。

インデントがタブとスペース
array.c見ただけですが、タブとスペースがごっちゃまぜ(´・ω・｀)(´・ω・｀)(´・ω・｀)
統一してほしいです！
宗教戦争になりそうですが・・・

redmine
会社でもredmine使っているのに、家でもredmineでissueみることになるなこれ(^ω^；三；^ω^)
redmineがもうちょっと進化してくんない限りgithubがいいですなあ・・

スポンサードリンク
[asin:4274050653:detail]

続く Rubyの#13053を読んでみる(2) [http://www.koooge.com/entry/2016/12/26/005958]
