---
title: "Debian下安装HUGO"
date: 2022-11-10
lastmod: 2022-11-10
author: ["沧海"]
tags: ["HUGO"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
- 创建文件夹
```c
mkdir /home/hugo && cd /home/hugo
```
- 使用`wget`命令下载HUGO，最新版本见[Github](https://github.com/gohugoio/hugo/releases)
```c
wget -O hugo.tar.gz https://github.com/gohugoio/hugo/releases/download/v0.107.0/hugo_extended_0.107.0_linux-amd64.tar.gz
```
- 解压
```c
tar -zxvf hugo.tar.gz
```
- 移动可执行文件
```c
mv hugo /usr/local/bin
```
- 删除目录
```c
rm -rf /home/hugo
```
- 验证版本
```c
hugo version
```