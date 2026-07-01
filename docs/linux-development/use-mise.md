---
sidebar_position: 4
title: miseの使用
tags:
    - プログラミング言語
---

# miseの使用

miseのインストールは前回の記事参照

miseは開発ツールのバージョン管理だけでなく、プロジェクトごとの環境変数やタスクも管理できる。

## インストールできるツールの確認

以下のコマンドでmiseからインストールできるツールを確認できる。

```bash
mise registry
```

Node.jsやPythonのような言語だけでなく、GitHub Releases, npm, pipx, aquaなどのバックエンドからCLIツールを入れることもできる。

特定のツールだけ確認したい場合は、名前を指定する。

```bash
mise registry node
```

## インストールできるバージョンの確認

指定したツールでインストールできるバージョンを確認する。

```bash
mise ls-remote [ツール名]
```

例としてNode.jsのバージョンを確認する場合は以下。

```bash
mise ls-remote node
```

メジャーバージョンを絞りたい場合は以下のように指定できる。

```bash
mise ls-remote node@20
```

## グローバルで使うツールのインストール

全フォルダでデフォルトとして使うツールは`--global`を付けて指定する。

```bash
mise use --global [ツール名]@[バージョン名]
```

コピペ用によく使うツールのインストールコマンドを示す。

```bash
mise use --global node@lts
```

```bash
mise use --global python@3
```

```bash
mise use --global go@latest
```

`--global`で指定した内容は、基本的に`~/.config/mise/config.toml`に保存される。

## 特定フォルダで使うツールの指定

プロジェクトごとに使うバージョンを変えたい場合は、そのプロジェクトのフォルダで`mise use`を実行する。

```bash
mise use node@lts
```

```bash
mise use python@3
```

このコマンドを実行すると、カレントディレクトリに`mise.toml`が作成または更新される。

例として、以下のような内容になる。

```toml
[tools]
node = "lts"
python = "3"
```

`mise.toml`を直接編集した場合は、以下のコマンドで指定されているツールをインストールする。

```bash
mise install
```

これにより、プロジェクトの参加者が同じツールバージョンを使いやすくなる。

## asdfの.tool-versionsとの互換性

miseはasdfで使っていた`.tool-versions`も読むことができる。

そのため、既存のasdfプロジェクトにmiseを導入する場合でも、最初から`mise.toml`に移行しなくても動かせることが多い。

ただし、miseの環境変数やタスクランナー機能を使う場合は`mise.toml`を使う。

## mise trust

他人が作ったリポジトリをcloneした場合、`mise.toml`の内容を確認してから信頼する。

```bash
cat mise.toml
```

問題なければ以下を実行する。

```bash
mise trust
```

`mise trust`は、現在のディレクトリまたは親ディレクトリにある`mise.toml`を信頼済みにするコマンドである。

信頼済みかどうかを確認したい場合は以下。

```bash
mise trust --show
```

信頼を取り消したい場合は以下。

```bash
mise trust --untrust
```

:::caution

`mise.toml`には環境変数、hooks、タスクなど、実行時に影響する設定を書ける。

知らないリポジトリでは、内容を確認せずに`mise trust`しないこと。

:::

## mise run

miseでは`mise.toml`にタスクを定義して、`mise run`で実行できる。

例えば以下のように書く。

```toml
[tasks.doctor]
description = "開発環境の確認"
run = "node --version && python --version"

[tasks.dev]
description = "開発サーバーを起動"
run = "npm run dev"
```

定義したタスクは以下のように実行する。

```bash
mise run doctor
```

タスク一覧を確認したい場合は以下。

```bash
mise tasks
```

実際には実行せず、どのタスクが動くかだけ確認したい場合は`-n`を付ける。

```bash
mise run -n doctor
```

`mise run`でタスクを実行すると、`mise.toml`で指定したツールや環境変数が読み込まれた状態でコマンドが動く。

プロジェクトのセットアップ手順をREADMEに長く書く代わりに、`mise run setup`や`mise run doctor`のようなタスクを用意しておくと便利である。

## 参考資料

- [Getting Started](https://mise.jdx.dev/getting-started.html)
- [mise trust](https://mise.jdx.dev/cli/trust.html)
- [mise run](https://mise.jdx.dev/cli/run.html)
- [Tasks](https://mise.jdx.dev/tasks/)
