---
sidebar_position: 2
title: ASDFの使用
tags:
    - プログラミング言語
---

# ASDFの使用

asdfのインストールは前回の記事参照

asdfはプラグインを追加することで様々な言語をバージョン管理することが可能である

## プラグインのインストール

### プラグインのリストの取得

以下のコマンドにてasdfで管理出来る言語やフレームワーク、ツールが確認できる

pythonやrustの他、Flutterやyoutube-dlなんかも管理が可能である。

```bash
asdf plugin list all
```

### プラグインのインストール

```bash
asdf plugin add [プラグイン名]
```

コピペ用によく使う言語のpluginインストールコマンド

```bash
asdf plugin add nodejs
```

```bash
asdf plugin add python
```

```bash
asdf plugin add dotnet
```

## 言語の指定バージョンのインストールとグローバルでの有効化

### インストールできる言語バージョンの表示

```bash
asdf list all [プラグイン名]
```

実行するとインストール可能なバージョンが一覧で出る

### 指定言語の指定バージョンのインストール

```bash
asdf install [プラグイン名] [バージョン名]
```

インストールするバージョンに困ったら、最新のLTS版をインストールするべきである。

コピペ用にnodejs,Python,dotnetのLTS版インストールコマンドを示す。

```bash
asdf install nodejs 20.11.1
```

```bash
asdf install python 3.11.8
```

```bash
asdf install dotnet 8.0.203
```

:::note
Pythonのインストールがコケたら

PythonのインストールではPython自体をビルドするためのライブラリをインストールする必要がある

```bash
sudo apt update
sudo apt install build-essential
sudo apt install libreadline-dev libncursesw5-dev libssl-dev libsqlite3-dev libgdbm-dev libbz2-dev liblzma-dev zlib1g-dev uuid-dev libffi-dev libdb-dev
```

:::

### グローバルでの有効化

asdfはフォルダごとに使うバージョンを指定できるが、全フォルダでデフォルトで使用するバージョンを指定することができる

基本的にLTS（長期サポート版）のバージョンにて使用することをお勧めする

```bash
asdf global [プラグイン名] [バージョン名]
```

## 特定フォルダでの有効化

以下のコマンドで特定フォルダ以下にてバージョンを指定することができる
```bash
asdf local [プラグイン名] [バージョン名]
```

`asdf local`コマンドを実行すると実行したディレクトリに`.tool-versions`ファイルが生成される

`.tool-versions`ファイルがあるフォルダにて`asdf install`をすることで指定されているバージョンがインストールされる

これにより複数人開発にて、複数の言語を使ったプロジェクトを進めている際に言語やツールのバージョン差異による不具合に悩まされなくなる