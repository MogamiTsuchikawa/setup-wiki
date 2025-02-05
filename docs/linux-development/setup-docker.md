---
sidebar_position: 3
title: UbuntuなどでDockerをインストールする
tags:
    - Docker
---

# UbuntuなどでDockerをインストールする

WSL環境の人はDockerDesktopをインストールすることをお勧めする。

Ubuntu DesktopやUbuntu Serverなどの環境を想定している。

## 前提ソフトウェアの用意

以下のコマンドにてインストール
```bash
sudo apt update
sudo apt install ca-certificates curl gnupg lsb-release
```

## Docker公式のGPG鍵の追加

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

:::note
RaspberryPiのRasbianなどのDebianの場合は以下

```bash
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

:::

## Dockerのインストール

Stable版を以下で指定する

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

:::note
RaspberryPiのRasbianなどのDebianの場合は以下

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

:::

以下でDockerをインストールする
```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```


## Dockerの動作確認

```bash
sudo docker run hello-world
```

以下のメッセージが出ればインストールは完了している

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec31eb5944: Pull complete
Digest: sha256:53641cd209a4fecfc68e21a99871ce8c6920b2e7502df0a20671c6fccc73a7c6
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## dockerコマンドをsudo無しで実行する

sudo無しで実行できるように、dockerグループに現在のユーザを追加する

```bash
sudo gpasswd -a $USER docker
```

追加完了後、一度exitして再ログインすることで反映される

## 参考資料

- [Docker Engine インストール（Ubuntu 向け）](https://matsuand.github.io/docs.docker.jp.onthefly/engine/install/ubuntu/)
- [Dockerコマンドをsudoなしで実行する方法](https://qiita.com/DQNEO/items/da5df074c48b012152ee)