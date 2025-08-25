---
title: "Linux切换密码登录"
date: 2022-09-30
lastmod: 2022-09-30
author: ["沧海"]
tags: ["Linux"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
#### 切换Root用户
```c
sudo su
```
#### 设置密码
```c
passwd root
```
- 需要输入**两次密码**，输入的时候**密码不会显示**
#### 编辑**ssh**配置文件
```c
vim /etc/ssh/sshd_config
```
- 把**PasswordAuthentication no**改为**PasswordAuthentication yes**  
- 把**PermitRootLogin no**改为**PermitRootLogin yes**
#### 重启sshd
```c
systemctl restart sshd
```