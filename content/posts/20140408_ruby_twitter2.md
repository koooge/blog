---
title: "twitterにRubyでHello World! Windowsで!(2/3)"
date: 2018-03-13T14:19:36.000Z
draft: false
---

こにちは。
さてStep2は、②twitterのAPI key取得とアプリ認証　です。((現時点ではTwitter API1.1です))

[https://dev.twitter.com:title]
上記リンクから、twitterIDでSign inして[Create New App]します。
下記が参考になります。
[
http://pocketstudio.jp/log3/2012/02/12/how_to_get_twitter_apikey_and_token/:title
]

尚、今回はHello World!とつぶやくため、Access TypeはRead and Writeとしました。
API key、API secret、Access token、Access token secretの4つの文字列を取得できました。

最後に、Twitterにログインし、[設定]→[アプリ連携]から該当のアプリを許可します。

以上です。
