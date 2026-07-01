---
sidebar_position: 3
title: miseのセットアップ
tags:
    - プログラミング言語
    - Windows
---

# miseのセットアップ

## miseとは？

miseはNode.js, Python, Go, Rustなどの開発ツールのバージョン管理をまとめて行えるツールである。

asdfと同じように複数の言語やCLIツールを管理できるが、miseは環境変数の読み込みやタスクランナーの機能も持っている。

公式サイトは [mise.jdx.dev](https://mise.jdx.dev/)

## Linux/macOSでのインストール

LinuxやmacOSでは以下のコマンドでインストールできる。

```bash
curl https://mise.run | sh
```

標準では `~/.local/bin/mise` にインストールされる。

## Linux/macOSでPathを通す

使っているシェルに合わせて設定する。

UbuntuのデフォルトはBashである。

:::note

vi/vim, emacsなどのCUIなエディタを使ったことがない人は、シンプルなnanoを使ってみましょう。

ファイルを開くには以下のコマンド

```bash
nano [開きたいファイルのパス]
```

矢印キーでカーソル位置を移動できます。

保存時には`Ctrl + O`をしてから`Enter`

nanoを閉じたい場合は`Ctrl + X`

:::

### Bashの場合

`~/.bashrc`の末尾に以下を追記する。

```bash
eval "$(~/.local/bin/mise activate bash)"
```

以下のコマンドで追記してもよい。

```bash
echo 'eval "$(~/.local/bin/mise activate bash)"' >> ~/.bashrc
```

### Zshの場合

`~/.zshrc`の末尾に以下を追記する。

```bash
eval "$(~/.local/bin/mise activate zsh)"
```

以下のコマンドで追記してもよい。

```bash
echo 'eval "$(~/.local/bin/mise activate zsh)"' >> ~/.zshrc
```

設定後はターミナルを開き直す。

## Windowsでのインストール

WindowsではPowerShellを使う。

公式ではScoopでのインストールが推奨されている。

### Scoopのインストール

Scoopが入っていない場合は、PowerShellで以下を実行する。

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

### miseのインストール

Scoopでmiseをインストールする。

```powershell
scoop install mise
```

wingetを使いたい場合は以下でもインストールできる。

```powershell
winget install jdx.mise
```

### PowerShellでPathを通す

PowerShellの起動時にmiseを有効化するように、PowerShell profileに設定を追記する。

```powershell
New-Item -ItemType Directory -Force (Split-Path $PROFILE)
'(&mise activate pwsh) | Out-String | Invoke-Expression' | Add-Content $PROFILE
```

設定後はPowerShellを開き直す。

:::note

Scoopでインストールした場合、miseのshimへのPathは自動で追加される。

PowerShellでディレクトリごとのツール切り替えや環境変数読み込みを使うために、上記の`mise activate pwsh`も設定しておくとよい。

:::

## 動作確認

ターミナルやPowerShellを開き直した後、以下を実行する。

```bash
mise --version
```

続いて、miseの設定状態を確認する。

```bash
mise doctor
```

`mise doctor`で大きなエラーが出なければセットアップ完了である。

## 参考資料

- [Installing Mise](https://mise.jdx.dev/installing-mise.html)
- [Getting Started](https://mise.jdx.dev/getting-started.html)
