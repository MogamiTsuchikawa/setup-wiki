---
sidebar_position: 6
title: sudoをパスワードなしにする
tags:
    - Linux
    - sudo
---

# sudoをパスワードなしにする

UbuntuなどのLinux環境で、sudoの実行時にパスワードを求められないようにする。
すでにsudoを使える一般ユーザーでログインした状態を想定している。

:::note

設定したユーザーはパスワードなしで管理者権限のコマンドを実行できるようになる。自分だけが使う開発環境など、用途を確認して設定する。

:::

## 設定する

以下で自分のユーザー名を確認する。

```bash
whoami
```

構文をチェックしてくれる`visudo`で設定ファイルを開く。この時点ではパスワードの入力が必要になる場合がある。

```bash
sudo visudo
```

ファイルの末尾に以下を追記する。`your_username`は先ほど確認したユーザー名に置き換える。

```text
your_username ALL=(ALL:ALL) NOPASSWD: ALL
```

保存してエディタを閉じると反映される。構文エラーが出た場合は`e`で編集に戻って修正する。

## 動作確認

認証情報のキャッシュを無効にして、パスワードなしで実行できることを確認する。

```bash
sudo -k
sudo -n whoami
```

`root`と表示されれば設定完了である。`sudo -n`はパスワードが必要な場合に、入力を求めずエラーになる。

## 元に戻す

`sudo visudo`で追加した行を削除して保存する。続けて`sudo -k`を実行すると、次回からパスワードが必要になる（ほかにパスワード不要の設定がない場合）。

## 参考資料

- [sudoersマニュアル](https://www.sudo.ws/docs/man/sudoers.man/)
- [visudoマニュアル](https://www.sudo.ws/docs/man/visudo.man/)
