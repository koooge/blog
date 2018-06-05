---
title: "仮想マシンの起動がめんどい"
date: 2018-03-13T14:23:21.000Z
draft: false
---

こんばんは

開発環境やらgitサーバはvagrant使って構築してるのですが、
PC起動するたびにvagrant up
終了するたびにvagrant halt

を毎回やるのは、さすがに面倒だなとおもったので調べた

[https://technet.microsoft.com/ja-jp/library/cc770300.aspx:title]
やっぱ、あるんですね。

下記が詳しくて参考になったのでメモメモ
[http://piyopiyocs.blog115.fc2.com/blog-entry-890.html:title]

vagrant.bat作って
cd <<.vagrantあるフォルダ>>
vagrant %1
スタート時は引数にup、シャットダウン時はhaltいれる
