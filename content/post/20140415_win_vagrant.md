---
title: "windowsでvagrant設定って割とめんどいですよね"
date: 2018-03-13T14:22:03.000Z
draft: false
---

①まずrubyをいれるのにひと手間
②Gemでひと手間
③Bundlerでひと手間
さらに認証プロキシ環境下だと色々と障害がありました・・。
そこを突破してようやくvagrantが使えるわけです。

で、オマケに
④winはsshがない
なので、これまでwinのPowerShellでvagrant upしてTeratermからssh接続してました。

cygwinは入れたくないな～mingwとかなあ～ウーン・・

”それ、Git-Bashにあるよ！”
マジ？ということで早速ＤＬ。

Git-Bashのbinにパス通して
vagrant sshできようになりましたー！

ついでににsaharaもいれた。
saharaって何か知らなかったのですが、VBOXのsnap shotをお手軽に取れる感じ、
超便利ですね。

会社のPCでvagrant使ってる環境そのうちメモる。。
