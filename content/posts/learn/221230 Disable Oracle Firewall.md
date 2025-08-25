---
title: "Oracle关闭防火墙"
date: 2022-12-30
lastmod: 2022-12-30
author: ["沧海"]
tags: ["Oracle"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
#### Ubuntu
- 开放所有端口
```c
iptables -P INPUT ACCEPT && iptables -P FORWARD ACCEPT && iptables -P OUTPUT ACCEPT && iptables -F
```
- 删除防火墙
```c
rm -rf /etc/iptables && reboot
```
#### Centos
- 删除多余附件
```c
systemctl stop oracle-cloud-agent && systemctl disable oracle-cloud-agent && systemctl stop oracle-cloud-agent-updater && systemctl disable oracle-cloud-agent-updater
```
- 停用Firewall
```c
systemctl stop firewalld.service && systemctl disable firewalld.service
```