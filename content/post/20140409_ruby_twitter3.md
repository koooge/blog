---
title: "twitterにRubyでHello World! Windowsで!(3/3)"
date: 2018-03-13T14:20:00.000Z
draft: false
---

こにちは。
さて最後、③hello_world.rbを書く　です。

gemでtwitterをinstallします。いやーRubyは大変楽チンですね。

||
gem install twitter
||<
ちなみに現在のバージョンは5.8.0です。

んで下記を参考にhello_world.rbを書きます。
[http://rdoc.info/gems/twitter:title]

||

!/usr/bin/env ruby
require 'twitter'

client = Twitter::REST::Client.new do |config|
config.consumer_key = "YOUR_CONSUMER_KEY" #API key
config.consumer_secret = "YOUR_CONSUMER_SECRET" #API secret
config.access_token = "YOUR_ACCESS_TOKEN" #Access token
config.access_token_secret = "YOUR_ACCESS_SECRET" #Access token secret
end

client.update("ハローRubyなう")
||<
なお、日本語ツイートするためには、rbファイルをUTF-8エンコードで保存する必要がありました。
SJISだとエラーがでました。。

||
ruby hello_world.rb
||<
で、できたかと思います。
twitterにRubyでHello World! Windowsで!は以上となります。

windowsでほぼ初めてコード書きましたが、エディタ悩みますね。
いまのところTeraPadを使ってますが、量書くようになると辛そう。。

以下追記
そういえば途中SSL証明書まわりでエラーがでました。
これは現在のtwitter API1.1はSSL通信を必須としたらしく、該当の証明書を手動で入れないといけないようです。
[https://blog.twitter.com/2013/rest-api-ssl-certificate-updates:title]

symantec(旧verisign)のHPからRoot3のpemを任意の場所にダウンロードします。
[https://www.symantec.com/page.jsp?id=roots:title]
PCA-3G5.pemのあるディレクトリを環境変数SSL_CERT_FILEにパス通して完了です。
