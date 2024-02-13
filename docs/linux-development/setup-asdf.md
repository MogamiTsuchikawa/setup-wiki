---
sidebar_position: 1
title: ASDFのセットアップ
tags:
    - プログラミング言語
---

# ASDFのセットアップ

## ASDFとは？

rubyenv, pyenvといった個々の言語バージョン管理ツールとは違い、各言語のバージョン管理を一括で行えるツール。

公式サイトは [asdf-vm.com](https://asdf-vm.com/)

## 依存ソフトのインストール

```
sudo apt update
sudo apt install curl git
```

## asdfのダウンロード

```
git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.14.0
```

コマンドが示すとおり、ユーザーフォルダ内の.asdfフォルダに入る。

## Pathを通す

使っているターミナルソフトにより異なる

UbuntuのデフォルトはBashである

:::note

vi/vim, emacsなどのCUIなエディタを使ったことがない人は、シンプルなnanoを使ってみましょう。

ファイルを開くには以下のコマンド
```
nano [開きたいファイルのパス]
```

矢印キーでカーソル位置を移動できます。

保存時には`Ctrl + O`をしてから`Enter`

nanoを閉じたい場合は`Ctrl + X`

:::

### Bashの場合

`~/.bashrc`の末尾に以下を追記する

```
. "$HOME/.asdf/asdf.sh"
. "$HOME/.asdf/completions/asdf.bash"
```

### Zshの場合

`~/.zshrc`の末尾に以下を追記する

```
. "$HOME/.asdf/asdf.sh"
# append completions to fpath
fpath=(${ASDF_DIR}/completions $fpath)
# initialise completions with ZSH's compinit
autoload -Uz compinit && compinit
```

## 動作確認

`asdf`とターミナルに打って、以下のように使い方説明が出ればOK

```
$ asdf
version: v0.13.1

MANAGE PLUGINS
asdf plugin add <name> [<git-url>]      Add a plugin from the plugin repo OR,
                                        add a Git repo as a plugin by
                                        specifying the name and repo url
asdf plugin list [--urls] [--refs]      List installed plugins. Optionally show
                                        git urls and git-ref
...
```