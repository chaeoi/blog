---
title: "删除Windows Defender保护记录"
date: 2020-08-25
lastmod: 2020-08-25
author: ["沧海"]
tags: ["Win10"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
电脑下载不安全文件后**Windows Defender**迅速报毒，文件删除后，Windows Defender仍然提示，进入如下目录，**清空目录下所有文件**，即可删除所有保护记录，进而解决问题
```c
C:\ProgramData\Microsoft\Windows Defender\Scans\History\Service\DetectionHistory
```