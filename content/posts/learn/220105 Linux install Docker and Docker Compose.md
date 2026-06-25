---
title: "Linux安装Docker及Docker Compose"
date: 2022-01-05
lastmod: 2022-01-05
author: ["沧海"]
tags: ["Docker"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
推荐使用Docker官方一键安装脚本
```c
curl -fsSL https://get.docker.com | bash -s docker
```
如需自行安装可参考如下步骤：
#### 安装Docker CE
- 更新存储库
```c
apt-get update -y
```
- 安装依赖
```c
apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common -y
```
- 下载并添加 GPG 密钥，**Debian**执行第一条，**Ubuntu**执行第二条
```c
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```c
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
- 将Docker CE存储库添加到APT，**Debian**执行第一条，**Ubuntu**执行第二条
```c
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list
```
```c
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list
```
- 更新存储库
```c
apt-get update -y
```
- 安装Docker CE
```c
apt-get install docker-ce docker-ce-cli containerd.io -y
```
- 验证版本
```c
docker --version
```
#### 安装Docker Compose
最新版地址见[Github](https://github.com/docker/compose/)
```c
apt install docker-compose-plugin -y
```