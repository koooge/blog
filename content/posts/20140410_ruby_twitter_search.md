---
title: "rubyでtwitterAPIを使わずにエゴサーチする"
date: 2018-03-13T14:20:28.000Z
draft: false
---

こにちは。
Ruby継続しております。

題名の通りRubyでエゴサーチするわけなのですが、
トークンとかめんどくせえな、と思いまして
HTMLをGetしてオレオレパースするというコードを書きました。Ruby1.9.2です。
searchWordに探したい文言を入れ実行すると最近の20件が表示される、
という仕様です。日本語対応です。

twitter_ego_search.rb

||

coding : utf-8
require 'net/http'
require 'uri'

searchWord = 'hello'

FullNameRegexp = %r|(.?)|
UserNameRegexp = %r|@(.?)|
TweetRegexp = %r|

(.*?)

|

def removeHtmlTag(str)
str = str.gsub(%r|</?strong>|, '')
str = str.gsub(%r|<a href.?(.?)|, '@\1')
str = str.gsub(%r|<a href="http[^>]?>(.?)|, '\1')
str = str.gsub(%r|]?>|, '').gsub(%r||, '')
str = str.gsub(%r|<img class="twitter-emoji" src="(.?)"[^>]*?>|, '')

str = str.gsub(%r|<img .?src="(.?)"[^>]*?>|, '\1')
str = str.gsub(%r|pic.twitter.com/|, 'https://pic.twitter.com/')
return str
end

def removeRefChar(str)
str = str.gsub(%r|
|, ' ')
str = str.gsub(%r| |, ' ')
return str
end

Net::HTTP.version_1_2
url = URI.parse(URI.encode('https://twitter.com/search?q=' + searchWord +
'&src=typd'))
Net::HTTP.get(url).each_line {|h|
if h =~ FullNameRegexp
print($1)
end
if h =~ UserNameRegexp
print('(@' + $1 + ') ')
end
if h =~ TweetRegexp
_tweet = $1
_tweet = removeHtmlTag(_tweet)
_tweet = removeRefChar(_tweet)
print(_tweet + "\n")
end
}
||<

もうちょっと手直しする予定・・ですが、
GitHubにあげたことないのでやってみたいと思ってます。

こんなクソコードあげても大丈夫なんかな～
