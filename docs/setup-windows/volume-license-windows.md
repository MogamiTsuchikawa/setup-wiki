---
sidebar_position: 1
title: ボリュームライセンスなWindowsのセットアップ
tags:
    - Windows
    - 芝浦工業大学
---

# ボリュームライセンスなWindowsのセットアップ

私が在籍する芝浦工業大学のWindows11 Educationボリュームライセンスの構築方法をまとめています。

## Windows11 Educationのインストール

[ここ](https://www.microsoft.com/ja-jp/software-download/windows11)
からWindows11のメディア作成ソフトやISOをダウンロードする。

Windows11 のインストーラ起動後はEducationライセンスを選択して、あとは普通にセットアップを進める。

## 初回アカウントセットアップ

ドメインに参加するという選択で、MSアカウントなどは紐付けるにセットアップを進める（後で紐付けれるので、今はやらない方が良い）

アカウント名は日本語名ではなく、アルファベットで空白などを付けない方が良い

:::tip

アカウント名はそのままユーザーフォルダに使用される。日本語や空白文字はアプリによっては不具合の元なので、無難に小文字の名称が良い

:::

## ライセンス認証

:::warning

自宅のマシンなどにインストールした場合は大学へVPNを繋げ、ライセンス認証サーバーに接続できるようにする

:::

`PowerShell`を開き、以下のコマンドでWindows 10/11 Educationのライセンスキーを入力する

```
slmgr -ipk NW6C2-QMPVW-D7KKK-3GKT6-VCFB2
```

:::tip

その他のエディションのライセンスキーは[ここ](https://learn.microsoft.com/ja-jp/windows-server/get-started/kms-client-activation-keys)に記載されている

:::

ライセンス認証サーバーを設定する

```
slmgr -skms kms.sic.shibaura-it.ac.jp
```

ライセンス認証を実行する

```
slmgr -ato
```

ライセンス認証有効期限の確認をする

```
slmgr -dlv
```

## 以上

これでWindows 10/11 Educationのライセンス認証などは完了です。

