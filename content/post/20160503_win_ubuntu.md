---
title: "Bash on Ubuntu on Windowsが起動しなくなった"
date: 2018-03-13T14:28:32.000Z
catgories: ["windows", "bash", "ubuntu"]
draft: false
---

Bash on Ubuntu on Windowsが起動しなくなった
[f:id:koooge:20161028230721j:plain]

起動しても真っ黒。
この後2~3分したら勝手に窓を閉じる。

コマンドプロンプトから起動を試みる。

C:\Users\koooge>bash
エラー: 0x80080005


エラーが出て起動しない。アカン。

諦めて再install
 1. PCを再起動する
 2. コマンドプロンプトを開く
 3. Linux subsystemのuninstall lxrun /unsintall /full
 4. Linux subsystemのデータを消す rmdir /S %LOCALAPPDATA%\lxssで
 5. Linux subsystemのinstall lxrun /install /y
 6. 完了！yay!

お好みでユーザー作成
上記までだとBash起動時にroot権限となっているので、お好みでユーザーを作成します。
ユーザー名は適宜読み代えてください。

ユーザー作成
Bashで実行

#root@koooge_PC:~# adduser koooge
ユーザー `koooge' を追加しています...
新しいグループ `koooge' (1000) を追加しています...
新しいユーザー `koooge' (1000) をグループ `koooge' に追加しています...
新しい UNIX パスワードを入力してください:
新しい UNIX パスワードを再入力してください:
passwd: password updated successfully
Changing the user information for koooge
Enter the new value, or press ENTER for the default
        Full Name []: koooge
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
以上で正しいですか? [Y/n] y


sudoに追加する
root@koooge_PC:~# gpasswd -a koooge sudo
Adding user koooge to group sudo


コマンドプロンプトでdefault userの切り替え
C:\Users\koooge>lxrun /setdefaultuser koooge
UNIX ユーザーが見つかりました: koooge
既定の UNIX ユーザーが次に設定されました: koooge


以上で完了です。
