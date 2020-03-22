---
title: "2020年版Windowsの開発環境"
date: 2020-03-21T14:52:11+01:00
categories: ["windows", "wsl2", "vscode", "docker"]
draft: false
aliases: ["/post/20200321_2020windevenv/"]
---

　縁あって2019年末にスウェーデンに移住してきました。よっしゃ、これからは北欧移住クソオシャイキリBlogでも書いてやるぜ！ということもなく、昨今のコロナ騒ぎで観光もできず引き籠っています（メチャクチャ寒いし）。

今回はそれとは全く関係なく、2020年のWindowsでのソフトウェア開発環境について書きます。

## これまでのWindows
Q. 世のエンジニアはなぜ大体Mac使ってるんですか

A. Macで環境構築するほうが簡単だからです

私も現在、仕事では開発メンバーと環境を揃えるためにMacを利用しています。が、プライベートではWindowsを使っています。何故？当然 **ゲームするため** です。この理由がある以上Windowsを使い続けると思います。また値段もMacよりも安く、ハードウェアのバリエーションも豊富です。

ソフトウェア開発(社の掟に縛られてない）において、これまでWindowsユーザーは少数派でしたが、徐々にWindowsにシェアが置き換わっているように感じます。

### Windows環境構築つらい話
　Windowsで開発環境を構築するのは大変です（ただし、Visual Studio等のIDEを使って、特定の言語で開発する場合は別）。新卒で入社した会社では **開発環境構築手順書.doc** というドキュメントを片手に開発環境を構築していました。Windowsを利用している開発者にとって、Virtual BoxでLinuxを立ち上げてしてからがスタートラインとなっていました。複雑かつ多量のGUIでの環境構築で、十人十色の環境ができあがり、しばしばこの環境の差異が問題となっていました。（この頃からIaCに傾倒しだしました。）

　Node.jsやRubyなどでも、依存関係のインストール時によくエラーを起こしていました。node-gypがエラーになったり、一部gem(nokogiriとか)のインストールでめちゃくちゃハマったり、環境構築でハマりポイントが多いです。（この頃からWindowsは上級者OSだと思い直すようになりました。）
また、ネットの記事をみても、大体がMac用の記事ばかりです。 まず `brew install` して、とか **brewねえよ！ (ノ`Д´)ノ彡┻━┻** と何度も思いました。

この章、まだまだ書けそうなんですが、要は **Windows色々つらかった** という話です。

### Vagrantが体験を変えた
　[Vagrant](https://www.vagrantup.com/)の登場によって、Virtual Boxの環境構築がものすごく簡単になりました。また、Vagrantfileによって冪等性が担保されるようになりました。
Vagrantは本当に素晴らしいプロダクトで、Windowsの開発環境構築の負荷が劇的に下がりました。

しかし、まだ完璧ではなく、Vagrantもruby依存によりインストールが難しかったり、稀にVMが壊れてしまうなどの難点があり、Macの体験に並んではいませんでした。

### chocolateyコレジャナイ
　Windowsにもbrewのような物、[chocolatey](https://chocolatey.org/)が登場しました。これによってWindowsでもある程度簡単にツールがインストールできるようになりました。

が、これはあくまでbrewのような物。俺たちが本当に欲しいのは、 **blogとかQiitaに書いてある手順をそのままコピペ実行すれば良いという体験** なんだ！chocoなんてほとんど誰も使ってないです。。

### でもWindows環境すごい変わってきた
　様々な要因により、Windowsが使いやすくなってきています。

- Office365やG Suiteの登場: 従来のOfficeが不要になり、クラウド化されることで使いやすくなった。
- Surfaceの登場: Windows公式の完成度の高いイケてるLaptopの登場。これでWindowsユーザーも増えた
- GitHubやnpmの買収: Powershellなどのオレオレ志向から一転、Linuxにすり寄り、OSSに注力するようになった。開発者に優しいWindows
- Google ChromeやFirefoxが動く: Edgeはとりあえず置いておいて、Chromeがサクサク動くからWindowsっていいよね

## 2020年Windowsはもっと使いやすいOSになるぞ
　前置きが長くなってしまいましたが、2020年個人的なWindowsの注目ポイントは、**WSL2** 、**VS code(Visual Studio Code)** 、**Docker Desktop WSL2 backend** です。

### 1) WSL2が来る！
　一番大きな変化はWSL2です。古き良きコマンドプロンプト、強気のPowershellを経て、WSL(Windows Subsystem for Linux)が登場しました。これにより、ソフトウェア開発者は、Linux(やMac)と同じようにソフトウェア開発できるようになりました。

そしてWSL2は、ざっくりWSLが更に高速になりました [（参考）](https://www.atmarkit.co.jp/ait/articles/1906/14/news019.html)。WSL2は、現在はWindows Insider Previewのみ利用可能ですが、来月(2020年4月)に正式リリースされます [参考](https://forest.watch.impress.co.jp/docs/news/1240999.html)。

　2020年4月といえば、[2020/4/23にUbuntuの時期LTS、Ubuntu 20.04(Focal Fossa)がリリース](https://www.omgubuntu.co.uk/2019/10/ubuntu-20-04-release-features)されます。利用するDistributionは、Ubutu18.04ではなく、これを待って20.04を利用するほうが良さそうです。

##### WSL2からWidows側のファイルを触るのは遅い
　WSLから、Windows側のファイルにアクセスすることもできます。Windows側のファイルは、`/mnt/c/`にマウントされています。しかし、Windows側ではファイルの権限がすべて付与された状態の777になるのと、git(statusやステージング)などのファイルアクセスが極端に遅いです。このため、Windows側でアクセスしないようなファイルは、WSL側にファイルを置くほうが良いと思います。

##### WSLの完全なコントロールは得られない
　WSLはPID 1ではなく、systemdにアクセスできません。 `System has not been booted with systemd as init system (PID 1). Can't operate.` と言われてしまいます。WSL2はほぼほぼLinuxそのものに近づいていますが、完全なコントロールは得られないようです。

### 2) VS Codeが使いやすすぎる
　WSL2環境において、WSL2側のファイルは、Windowsとは異なるファイルシステムとなります。このため、本来であればSSHしてからエディタを起動するなどの段階が必要となります。

VS Codeには、このあたりをいい感じにやってくれるPlugin [Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)が用意されています。VS CodeでWSLを起動すると自動でInstallを促される為、セットアップが簡単です。
また、wsl2でwsl2側のファイルを `$ code hoge.md` のように入力すると、Windows側でVS Codeが立ち上がり、普通にWindows側のファイルを編集しているかのように編集できます。普通に動いてすごく楽です。

#### VimからVS Codeへ
　エディタは開発効率に直結するため、可能ならば手に馴染むエディタ１つを使い倒したいものです。私は現在、Vimをメインに利用していますが、これを機にVS Codeに完全移行しようと考えています。理由は以下の４つ。

##### a. JavaScriptを書くのが楽
　仕事は主にJavaScript(TypeScript)を書いているので、これらと親和性の高いVS Codeを利用しない手はありません。

##### b. 設定が楽
　Vimはカスタマイズしてなんぼで、せっせとvimrcを設定していたのですが、もっとライトに使いたいと思っていました。一方VS Codeは、あらかじめおすすめ設定がされており、基本デフォルトで、適宜Pluginを入れて使うという使い方です。Pluginも、ファイルを開けば勝手にInstallを促されるので本当に楽です。

##### c. 言語側がVS Codeをサポートする
　JavaScriptに限らず、Flutter(Dart)などもVS Codeをサポートしています。VS Codeが様々な言語の重要なエコシステムとなっており、今後の発展にも期待できます。これは、VS CodeがOSSであることが大きいと思います。

##### d. SSHしなくなった
　特にインフラにおいて、VimもしくはViの操作を習得することは重要でした。管理するサーバーに接続して設定ファイルを更新する際、viが標準でinstallされているからです。しかし、昨今のサーバレスやSaaS文化において、管理対象の内部を意識しないといけないアーキテクチャが良くないとされています。 **SSHした負け** と言われているほどです。これによりVi(m)の操作方法を習得する理由が薄れてきました。

### 3) Docker Desktop WSL2 backendで高速に
　近年のソフトウェア開発では、VMよりも、Docker(等のコンテナ）が主流となっています。これにより、開発環境の構築がよりライトになりました。
Windows環境においても、これまでもDockerを利用することはできました。が、実質Docker on VMとなっており動作が遅かったり、Windows 10 Pro Editionでのみ利用可能だったりと、Docker for Macに比べ圧倒的にイマイチでした。

そんな折、前述のWSL2を使った、[Docker Desktop WSL2 backend](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)が登場しました。ざっくりDockerが高速になります。そして、Docker Desktop Community 2.2.2.0から、Windows 10 Home Editionでも利用可能になりました。(2020/3/21現在Edge版のみですが、来月にはStableとなるでしょう。)

DockerがWindowsで当たり前に動くようになり、ソフトウェア開発体験が良くなることは確実です。最アンド高。さよなら今までありがとうVirtual BOX。

## まとめ
WindowsでもMacと遜色なくソフトウェア開発が行える時代が来ました。

- WSL2スゴイ！
- VSCodeスゴイ！
- Windows 10 HomeでもDockerできる！
