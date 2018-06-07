---
title: "twitterにRubyでHello World! Windowsで!(1/3)"
date: 2018-03-13T14:19:12.000Z
draft: false
---

自分用のメモも兼ねているので、ところどころ詳細になってたりアバウトだったりします。ご容赦を。

題名の通りtwitterにRubyでHello World!しました。

このための作業としては3つのStep。

①WindowsにRubyの環境構築

②twitterのAPI key取得とアプリ認証

③hello_world.rbを書く

題名を(1/3)としてますが、今回は①WindowsにRuby環境を構築。

Rubyは1.8までと1.9以降でやや異なる言語らしいです。

このためRubyの版数を管理できるpikをインストールします。((https://www.ruby-lang.org/ja/installation/
))Linux のrbenvのようなツールです。



インストールは以下を参考

Rubyアソシエーション: 複数のRuby環境の構築

まず"C:\pik"にインストールして、パスを通す。(以下pik 0.2.8の情報です)

私の環境では元々Rubyが入っていなかったので、

"C:\Users<ユーザー名>.pik\config.yml"を編集しダミーを記述する必要がありました。

pik を使って Windows に ruby をインストール - miauの避難所


--------------------------------------------------------------------------------

"000: ruby 0.0.0 (dummy ruby for pik)":
:path: !ruby/object:Pathname
path: C:/pik/dummy
--- {}
その後cmdでpik listが通ることを確認。

pik list -rするとruby2.0.0までしか対応してないようだったので、

pik install ruby 1.8.7 と2.0.0しました。

その後ダミー削除pik uninstall 000

C:\Users<ユーザー名>.pik>pik list

187: ruby 1.8.7 (2012-10-12 patchlevel 371) [i386-mingw32]

200: ruby 2.0.0p195 (2013-05-14) [i386-mingw32]

よしよし。

以降ruby2.0.0を使っていくので、pik use 200します。#pik 200でもOK

そして、DevKit(32bit版)もインストールしておきます。((pikでinstallしたRubyが32bit版のためhttp://rubyinstaller.org/downloads))

これが無いとgem updateとかでエラーメッセージがでました。

ちなみに私はDevKit64bit版をインストールしていてハマりました。。

下記が大変わかりやすいです。

DevKitのインストール - Railsインストール

ruby dk.rb init、ruby dk.rb installが無事通って完了です。

最後に

gem update --system

gem update

しておきました。

長くなりました。続きは次回。
