---
title: "vagrant upでsshの認証こコケる"
date: 2018-03-13T14:24:18.000Z
draft: false
---

こんばんは。

掲題のとおりvagrant upしたとき

||
default: Warning: Connection timeout. Retrying...
default: Warning: Remote connection disconnect. Retrying...
default: Warning: Authentication failure. Retrying...
default: Warning: Authentication failure. Retrying...
default: Warning: Authentication failure. Retrying...
default: Warning: Authentication failure. Retrying...
default: Warning: Authentication failure. Retrying...
default: Warning: Remote connection disconnect. Retrying...
default: Warning: Authentication failure. Retrying...
default: Warning: Remote connection disconnect. Retrying...
default: Warning: Authentication failure. Retrying...
default: Warning: Remote connection disconnect. Retrying...
default: Warning: Authentication failure. Retrying...
||<
sshでコケはじめたPOI

前々回のブログ((
[http://koooge.hatenablog.com/entry/2015/05/19/000951:embed:cite]
))で書いたやつが全て元凶なのですが、、

前回のブログ((
[http://koooge.hatenablog.com/entry/2015/05/19/235224:embed:cite]
))で書いたとおりvagrant up時に既存のVM無視して新規にvagrant作られたせいだと思われます。

これによって.vagrantが上書きされたのですね。。

.vagrantの中身は下記のようになってます。

||
.vagrant
└─machines
└─default
└─virtualbox
action_set_name
id
index_uuid
private_key
||<

前回はidだけ修正しましたが、この他のファイルも上書きされたままとなって不味いようです。
sshだと恐らくprivate_keyが怪しいですね。
他のファイルは・・・なんだか分かりません。

軽くググって見た感じ、
private_keyは初回のvagrant up時にのみ鍵ペアを作成するそうです。
あれ？上書きしちゃった場合オワタなのでは・・？
