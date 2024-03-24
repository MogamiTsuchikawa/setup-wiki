---
sidebar_position: 2
title: デスクトップやドキュメントフォルダをDライブに移動する
tags:
    - Windows
    - PowerShell
---

# デスクトップやドキュメントフォルダをDライブに移動する

デスクトップやドキュメント、ピクチャなどのフォルダは実態はCドライブのユーザーフォルダにあるが、それをメインドライブであるCドライブとは別のドライブ（例としてDドライブ）に移動・設定する方法を記載する

## なぜ移動するのか

デスクトップやドキュメントなどのフォルダをCから別のドライブへ移動するメリットは大きく２つある

- CドライブをOSやアプリ専用、Dドライブをデータ専用にして、Cドライブに容量単価の高い高速なSSD・Dドライブに容量単価の安いHDDや安いSSDとコスパの良い構成を目指せるように
- OSが起動不可・不安定になってOS再インストールした際に、Cドライブしか初期化されないので、データが消えないように

## 方法

### 1. 手動で行う

まず、移行先のフォルダ（例えば、`D:\Desktop` ）を作成して、エクスプローラーからデスクトップを右クリックしてプロパティから場所の移動で先ほど作ったフォルダを指定する

### 2. スクリプトで行う

デスクトップフォルダ含め6つのフォルダで行うのは面倒なので以下のPowerShellスクリプトを使う

PowerShellを開き、コードを貼り付けて実行する

```powershell
# レジストリのパスを更新する関数
function Update-RegistryPath($regName, $newPath) {
    $keyPath = "HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders"
    Set-ItemProperty -Path $keyPath -Name $regName -Value $newPath
    # システムに変更を即時反映させる
    [System.Environment]::SetEnvironmentVariable('USERPROFILE', $env:USERPROFILE, 'User')
}

# 新しいベースフォルダの設定
$destBase = "D:"
# ユーザープロファイルパスの取得
$userProfile = [System.Environment]::GetFolderPath('UserProfile')

# 移動するフォルダとレジストリの値の名前
$foldersToMove = @{
    "Desktop" = "Desktop"
    "Documents" = "Personal"
    "Downloads" = "{374DE290-123F-4565-9164-39C4925E467B}"
    "Music" = "My Music"
    "Pictures" = "My Pictures"
    "Videos" = "My Video"
}

foreach ($folderName in $foldersToMove.Keys) {
    $regName = $foldersToMove[$folderName]
    $newPath = Join-Path -Path $destBase -ChildPath $folderName

    # 新しいフォルダを作成（存在しない場合）
    if (-not (Test-Path -Path $newPath)) {
        New-Item -ItemType Directory -Path $newPath
    }

    # ファイルとフォルダを新しい場所に移動
    Move-Item -Path $folderName\* -Destination $newPath -Force

    # レジストリパスを更新
    Update-RegistryPath -regName $regName -newPath $newPath
}

# システムに変更を反映させるためにExplorer.exeを再起動する
Stop-Process -Name explorer -Force
Start-Process explorer.exe

```