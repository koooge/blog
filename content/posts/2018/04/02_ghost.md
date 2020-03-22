---
title: "Ghostに移行しました"
date: 2018-03-29T02:00:00.000Z
categories: ["infra"]
draft: false
aliases: ["/post/20180402_ghost/"]
---

はてなブログをやめ、Ghost [https://ghost.org/]に移行した。理由は前回書いた通りです。いろいろな知見を得たので綴っておきたいと思います。

## ghost
ghostは、node.js製のcmsでGitHubでソースが公開されているOSSです。DBは選択でき、SQLiteもサポートされているようですが、基本はmysqlを使うと良さそうです。デザインは、デフォルト以外では

Marketplace [https://marketplace.ghost.org/]から購入するか、OSSを探すか、doc
[https://themes.ghost.org/v1/docs]に従って自作するかを選べます。テンプレートエンジンはHandlebars.js
[http://handlebarsjs.com/]を使用しており、自由度高くかつリッチにデザインが作れそうです。ちなみに、本blogは現在のところ
zutrinken/bleak [https://github.com/zutrinken/bleak]
を使用させてもらっています。ただ今後勉強がてら自作するかもしれません。

## https
ghostは、前段にnginxを立てることを想定されています。ghostを構築するghost-cli
[https://github.com/TryGhost/Ghost-CLI]を実行すると、let's
encryptを用いた証明書の設定を行い、nginxのconfの設定まで自動で行ってくれます。はてブは自分でサーバーを用意しないで良い代わりにこのあたりの自由度が低かったのですが、ghost-cliによって簡単にhttps化をすることができました。

## インフラについて
前述の通り自前でサーバーを用意する用がありました。といってもオンプレミスではなく、クラウド(IaaS)にインフラを構築しました。AWS,
heroku等々色々検討しましたが、GCP(GCE)のf1-micro(1vCPU,
0.6GBRAM)をusリージョンに立て、ストレージを磁器ディスクで30GBまでにすると、（本ブログのようなトラフィックが少ないものは）無料で運用できることがわかりました。
これについて、qiitaのこの記事 [https://qiita.com/ndxbn/items/7ef0a96e409a5b5837bd]
が大変参考になりました。

## Terraform化
Terraformを使って上記インフラの構築をコード化しました。GitHubで公開しています。
完全自動化ではなく、実際にghostをinstallする寸前まで構築するものとなっています。koooge/ghost-terraform
[https://github.com/koooge/ghost-terraform]。
mysqlのid/pwはハードコードになっていますが、外部公開しないのでSSHされないようにすれば問題ないと思います。"default-allow-ssh"はプロビジョニングのために設定されています。プロビジョニング後はこの設定を削除してsshポートを閉じるとよいと思います。

GCPの1年間の無料期間ということもあり、本当に完全無料で運用できているのか不明ですが、当初の目標は完璧に満たせていますので、このまま続けようと思います。
