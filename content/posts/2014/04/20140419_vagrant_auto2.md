---
title: "ローカルグループポリシーエディターだめかも"
date: 2018-03-13T14:23:58.000Z
draft: false
---

こんばんは

[http://koooge.hatenablog.com/entry/2015/05/19/000951:embed:cite]

と書きましたが、なんかうまく動いてないPOI？

[f:id:koooge:20150519234212p:plain]
こんな感じに新しいVM作ってupしちゃってる。。。

とりあえず直し方をググりました
[http://elm-arata.hatenablog.com/entry/2013/09/25/175547:embed:cite]

へー。vagrantって単純に.vagrantとUUIDとを紐づけしてるだけなんですね。

& 'C:\Program Files\Oracle\VirtualBox\VBoxManage.exe' list vms
して
元のUUIDを.vagrant/machines/default/virtualbox/idに書いて元通り一安心。

さて、やりたいことができてなくて困ったのですが
windowsのスタートアップ/シャットダウンは信用してはいけないんですかねえ。。
