---
title: "ISUCON9参加した"
date: 2019-09-13T00:16:47+09:00
categories: ["node", "nginx", "event"]
draft: false
aliases: ["/post/20190913_isucon9q/"]
---

[Rebuild.fm 246](http://rebuild.fm/246/)でghostとhugoの話をしていて、"ghostやhugoへの移行記事だけ書いて更新止まる奴"と聴いてギクリとしました。もろに私です。

タイトルの通り[ISUCON](http://isucon.net/)に会社の同僚(gms-hi-oda)とインフラエンジニア2人で参加してきました。ISUCONにはこれが初参加で、チーム名は"ISUCONなにもわからない"、言語はNode.jsをチョイス。最終スコアは6730イスコインで予選落ちしました。予選通過ラインは10000イスコイン強だったらしい。我々としては思ったより健闘してたという評価です。来年にむけてメモ。

### やったこと
- 10:10 競技開始
- 10:44 参考実装切り替え、各種logの仕込み。初期スコア1100イスコイン
- 11:10 SSH鍵の設定、GitHuB repoの設定完了
- 12:33 キャンペーンを4にする。3110イスコイン
- 14:14 静的ファイルをnginxから配信するようにした 4310イスコイン
- 16:27 [itemsテーブルにindex張った](https://github.com/koooge/isucon9q/commit/1a84b81e2e6bea76a38946421512098917152bb0)　5410イスコイン
- 17:01 VMを3台とも使う構成にした
- 17:15 http2にした
- 17:29 logを切って再起動試験した。6730イスコイン (疲れたので撤収)

##### 最終的にマシンの役割はこうなった
1. 終端nginx(静的ファイル配信) + webapp
2. webapp
3. MySQL

### 問題について
isucariというまんまメルカリのサービスの改修でした。isucariはロックの多いOLTPのワークロードで、業務で扱ってるものとは違っていて慣れてませんでした。これにより、ソースコードをパっと見てだけでは何がまずいのかわからなかったです。まず勝負の土俵にあがれなさを感じました。

### 競技時間について
競技時間は10:00-18:00と8時間もあるので、十分に長いと思っていました。過去のISCUON参加blogやtwitterをみると、豪華なお弁当を用意し、優雅に挑まれてる方もいましたし。しかし、いざやってみると、お昼はコンビニ弁当を食べながら作業し、それでも時間が足りないといった感じでした。時間配分的には、ISUCON自体に慣れてないために何をやったらいいかを模索する時間が多かったです。圧倒的な時間不足により、最後残り時間で取り組めるちょうどよい課題がなく、逆に時間が余ったので、結果論ですがお昼に時間とって問題を読み込めばよかったです。

### Node.jsについて
ISUCONではGoが強く人気なのは少し調べて分かっていましたが、会社の同僚と出場したこともあり、会社で使ってるメイン言語であるNode.jsを自然に選択しました。Node.jsの参考実装では、nodeは最新の12で、npmはfastifyにmysql2といった速度重視の最新のライブラリが使われていました。このためミドルウェアに関しては、競技中にアップグレードする必要がなく、すんなり問題に取り組めました。
それでも、ISUCON9の[予選突破チームの使用言語](http://isucon.net/archives/53789935.html)をみると、Goが強く、また参考実装がある言語の中でNode.jsだけ予選突破チームがいなかったです。とはいえ、今回はたまたま強い人が他の言語を使っていただけの話だと思います。多分おそらく。

### 競技マシンについて
マシンはAlibaba cloudというあまり利用事例を聴かないチャレンジングな環境でした。競技開始直後、VMを2つ作った段階でセキュリティロックがかかり、1時間程度VMが2つでした(焦ったけど我々は特に困らなかった）。AWSでいうEC2の概念とほぼ同じで、VM作った後はUbutu18の中で作業をするということで特に不都合はありませんでした。しいていうとVMの削除のことを"release"と書かれていたり用語でちょっと悩みました。
マシンは2コアのメモリ4GB。マルチコアだとNode.jsは不利そうだなあと思いましたが、そもそも我々はwebappがボトルネックに至らなかったです。。終始MySQLがボトルネックでした。

### 学びが多かった
今回は二人ともアプリケーションの改善を全くできなかったことが主な敗因だと学びました。ISUCONはインフラの設定はシュッと終えてアプリで差をつけるものだったとようやく分かりました。

- アプリ面
  - アプリケーションや仕様を読み解く力をつける
  - Node.jsでGOのpprofのような計測をする術を確立する
  - ISUCONであるあるな改善ポイントをしっかりゲットする
- インフラ面
  - 初期設定をシュッとするためにプロビジョナー作っておく
    - 特にdeploy周りに無用に時間を食った
  - nginx, MySQL, systemd、(redis)を学習しておく
  - ログや計測したデータをSlackなどにうまいこと共有する術を確立しておく
  - 外部API呼び出し箇所を計測する術を確立する

作業repositoryを公開しました。見返しても生々しい雑さで、deploy-all.shなどは、うまく動作せず時間の都合で諦めたなどなど。
https://github.com/koooge/isucon9q
