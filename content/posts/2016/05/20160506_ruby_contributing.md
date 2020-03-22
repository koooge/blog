---
title: "rubyに貢献してみる（ことを目指す)"
date: 2018-03-13T14:29:38.000Z
categories: ["ruby"]
draft: false
---

タイトルの通りワナビー脱却を宣言してみます。
rubyの実装になんとなく興味があるのでrubyに貢献することを目指します。


--------------------------------------------------------------------------------

ウデマエ
githubの使い方がなんとなく分かって、ruby-buildのコメントをちょっといじれたレベル。
[
https://github.com/rbenv/ruby-build/commit/9bc99717725de2935ebdca2c62cd9572ad65fbb6:embed:cite
]

でもこれで貢献というのはちょっとしょぼすぎる。。


--------------------------------------------------------------------------------

rubyへの貢献にむけて準備した
1. ML
まずML [https://www.ruby-lang.org/ja/community/mailing-lists/]に登録しました。
10種類程度ありますが、一応jrubyは覗いて、rubyとmruby系の物すべてに登録しました。
注意すべきは、とにかくメールがたくさん流れるので、メーラでの振り分け設定は必須です。

2. IRC
次に、IRC [https://www.ruby-lang.org/ja/community/]にも入ってみました。
ただ、こちらはどうやらrubyを使う人が質問を投げかける場のようです。
rubyへの（実装での）貢献を目指す人には無用に見えます。（IRCで投げかけるのもよいとは思われますが）

3. Issue tracker
rubyのIssue trackerは、redmineで運営されているようです。
redmineはrubyで作られたrailsで作られているというのに加え、
rubyはsvnでバージョン管理されていて、svnとの連携もしやすいredmineを選択されているのは妥当ですね。
[https://bugs.ruby-lang.org/:title]

とりあえず登録をし、見てみた感じプロジェクトは4つです。

 * Backport21
 * Backport22
 * CommonRuby
 * Ruby trunk

Ruby trunkだけ見ればよさそうかな？それでも2016/12/10現在でOpenのticketは大量にあって、

 * Bug: 1040
 * Feature: 871
 * Misc: 62

と大量！＠＠；

さすがruby。歴史を感じました。つまりこれらの動向をすべて抑えるというのは100%無理ですね。。
ただ、私の目下の目標としては、これらのticketのどれか１つをcloseまでもっていけばOKということなので、気楽に行きたいと思います。

私がredmineを業務で使っているというのもありますが、
rubyのredmineに特別なpluginも入ってなさそうだし、使用感は問題なさそうに感じました。

4. github
redmineのほかに、やはりイマドキのOSS開発といえばgithubも使われているようです。

[https://github.com/ruby/ruby:title]

Issue機能は(redmineに一本化するために）閉じられているようですが、ソースコードのミラーリングと、
Pull request機能は使われています。githubに慣れている人はコチラといった感じみたいです。

githubへのホスティングは、OSSとしてのrubyの門戸を広げるためだとは思いますが、逆に言うとこっちも見ないといけない気になります。
しかし、数日githubをwatchしてみましたが、そこまで使われていないっぽいです。
このあたりは、オピニオンリーダーmatzがgithub社へのロックインを嫌っているからでしょうか？
なんにせよ必ずしも見る必要がある訳ではなさそうです。


--------------------------------------------------------------------------------

rubyをビルドしてみる
さて、rubyをビルドしてみます。
リポジトリガイド [https://www.ruby-lang.org/ja/documentation/repository-guide/]
を見た感じ、前述してしまいましたが、
svnとgitの選択肢があることがわかります。ここで、svnを選択してみます。
ここにビルド方法も書いてほしいですが、書いてない；。rubyはこのあたりが初心者殺しだと思う。。
ですがgithubのREADME.mdには親切に書いてあったので、その通り勧めました。

build環境は Bash on Ubuntu on Windows  を使っていきます。（多分少数派だと思う。。）
これに必要なパッケージをいれます。svnを使うならsubsversion。あとはautoconfです。
他の依存パッケージはruby-buildを入れるときに入れてたっぽいです。

# apt-get install subversion autoconf


次に、rubyのビルドのためにrubyが必要なので、rbenv [https://github.com/rbenv/rbenv]とruby-build
[https://github.com/rbenv/ruby-build]を使ってruby 2.3.3p222をinstallしました。

ビルド
いよいよビルドですが、githubのREADME通りに勧めてもよいと思いますが、ソースコードの取得には一応svnを使いました。

$ svn co http://svn.ruby-lang.org/repos/ruby/trunk ruby
$ cd ruby
$ svn info
パス: .
Working Copy Root Path: /mnt/d/ruby
URL: http://svn.ruby-lang.org/repos/ruby/trunk
Relative URL: ^/trunk
リポジトリのルート: http://svn.ruby-lang.org/repos/ruby
リポジトリ UUID: b2dd03c8-39d4-4d8f-98ff-823fe69b080e
リビジョン: 57033
ノード種別: ディレクトリ
準備中の処理: 特になし
最終変更者: nobu
最終変更リビジョン: 57033
最終変更日時: 2016-12-09 12:25:20 +0900 (2016年12月09日 (金))
$ autoconf
$ ./configure
$ make


ですが・・・

(snip)

compiling bigdecimal.c
bigdecimal.c:119:1: error: static declaration of ‘rb_rational_den’ follows non-static declaration
 rb_rational_den(VALUE rat)
 ^
In file included from ../.././include/ruby/ruby.h:1997:0,
                 from bigdecimal.h:12,
                 from bigdecimal.c:13:
../.././include/ruby/intern.h:172:7: note: previous declaration of ‘rb_rational_den’ was here
 VALUE rb_rational_den(VALUE rat);
       ^
bigdecimal.c: In function ‘rb_rational_den’:
bigdecimal.c:124:5: error: too few arguments to function ‘rb_funcall’
     return rb_funcall(rat, rb_intern("denominator"));
     ^
In file included from bigdecimal.h:12:0,
                 from bigdecimal.c:13:
../.././include/ruby/ruby.h:1756:7: note: declared here
 VALUE rb_funcall(VALUE, ID, int, ...);
       ^
bigdecimal.c:126:1: warning: control reaches end of non-void function [-Wreturn-type]
 }
 ^
bigdecimal.c: At top level:
cc1: warning: unrecognized command line option "-Wno-self-assign" [enabled by default]
cc1: warning: unrecognized command line option "-Wno-constant-logical-operand" [enabled by default]
cc1: warning: unrecognized command line option "-Wno-parentheses-equality" [enabled by default]
cc1: warning: unrecognized command line option "-Wno-tautological-compare" [enabled by default]
make[2]: *** [bigdecimal.o] エラー 1
make[2]: ディレクトリ `/mnt/d/ruby/ext/bigdecimal' から出ます
make[1]: *** [ext/bigdecimal/all] エラー 2
make[1]: ディレクトリ `/mnt/d/ruby' から出ます
make: *** [build-ext] エラー 2

　　　∧∧
　　（　 ･ω･）
　 ＿|　⊃／(＿＿_
／　└-(＿＿＿_／
￣￣￣￣￣￣￣

　 ＜⌒／ヽ-、_＿_
／＜_/＿＿＿＿／


逆に考えろ、早速貢献のチャンスだ；


--------------------------------------------------------------------------------

まとめ
今後ぼちぼちbugとrubyのソースコードを眺めていきたいと思います。
