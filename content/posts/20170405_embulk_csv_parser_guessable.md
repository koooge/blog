---
title: "embulk-parser-csv_guessable作った"
date: 2018-03-13T14:33:17.000Z
draft: false
---

embulk-parsre-csv_guessableというembulkのプラグインを作りました。

[https://github.com/koooge/embulk-parser-csv_guessable:embed:cite]

## これは何？
embulk-standardsのcsvパーサを継承して機能拡張したものです。
オリジナルのcsvパーサでは、csvのスキーマをconfig.ymlにきっちり書く必要があります。
このためスキーマが変更された場合には動的に対応できず、embulkがコケてしまいます。

以前はこの問題に対応するために、embulk runの前にconfig.ymlを生成するオレオレguessスクリプトをかましていました。
が、

これやってしまうとなんでembulk使ってるんだっけ？と思うんですよねえ

— こうげ (@koooge) 2017年4月11日
[https://twitter.com/koooge/status/851758746556551168]という思いから、"Share & reuse the
plugin"の精神でpluginを作ってみることにしました。

## やりたいこと
下のような1行目がヘッダ行のcsvをよく見ると思います。（なんか名前あるんでしょうか？）

[http://www.soumu.go.jp/toukei_toukatsu/index/seido/9-5.htm:title]

```
ken-code,sityouson-code,tiiki-code,ken-name,sityouson-name1,sityouson-name2,sityouson-name3,yomigana
1,0,1000,北海道,,,,ほっかいどう
1,100,1100,北海道,札幌市,,,さっぽろし
1,101,1101,北海道,札幌市,,中央区,ちゅうおうく
```


このヘッダ行を使って、いい感じにパースしたいというのがこのpluginの発想です。

## スキーマ変更に柔軟に対応する
オリジナルのcsvパーサだとオレオレスクリプトを噛ましていましたが、embulk-parser-csv_guessableはヘッダ行をカラム名として利用します。

```
  parser:
    type: csv_guessable
    schema_file: path/to/file_target.csv  # ヘッダのあるファイルを指定する
```


これによって、カラムの挿入や削除といったスキーマ変更にも対応できるだけでなく、
副次的な効果としてダラダラとcolumnsを定義せずとも簡単に書けます。

## カラムの型も指定する
parserでlongなりtimestampなりの型にしておきたいことがあると思います。

型はguessによって（簡単には）どうにもなりそうになかったので、指定したい場合は、元のcsvパーサと同じような書き方ができるようにしました。
なお、string型にする場合は書かなくても良いです。(デフォルトstring)

```
  parser:
    type: csv_guessable
    schema_file: path/to/file_target.csv
    columns:
      - {name: 'ken-code', type: long}
      - {name: 'sityouson-code', type: long}
      - {name: 'tiiki-code', type: long}
```


## 別名にする
特にヘッダが下記のような日本語の場合に、英語名に変更したい事があると思います。

```
県コード,市町村コード,地域コード,県名,市町村名1,市町村名2,市町村名3,読み仮名
1,0,1000,北海道,,,,ほっかいどう
1,100,1100,北海道,札幌市,,,さっぽろし
1,101,1101,北海道,札幌市,,中央区,ちゅうおうく
```



古き良きcsvといった感じ。。
下記のような設定で、value_nameをnameに置き換えることができます。

```
  parser:
    type: csv_guessable
    schema_file: path/to/file_target.csv
    columns:
      - {value_name: '県コード', name: 'ken-code', type: long}
      - {value_name: '市町村コード', 'sityouson-code', type: long}
      - {value_name: '地域コード', name: 'tiiki-code', type: long}
      - {value_name: '県名', name: 'ken-name', type: string}
      - {value_name: '市町村名1', name: 'sityouson-name1', type: string}
      - {value_name: '市町村名2', name: 'sityouson-name2', type: string}
      - {value_name: '市町村名3', name: 'sityouson-name3', type: string}
      - {value_name: '読み仮名', name: 'yomigana', type: string}
```


parserという割にguessしたりfilterっぽい事もしてるきもしますが、こういうcsvはよくあるので自分的には便利なんですよね。
これらのサンプルをgithubに置いてますのでご覧ください。

## 今後
イケてない部分がまだありますし、なんかもっと上手い方法がある気がしていますので、もう少しメンテ予定。
お気づきの点をリプライなりPRなりコメントいただけたら幸いです :bow: