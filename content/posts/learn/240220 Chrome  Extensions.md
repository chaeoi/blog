---
title: "流氓行为之自动安装Chrome插件"
date: 2024-02-20
lastmod: 2024-02-20
author: ["沧海​"]
tags: ["Chrome"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

之前安装某雷之后，**自动安装Chrome插件**，甚至重装Chrome都会自动安装，简直毒瘤。最近又发现了几款类似软件，记录一下他们是如何实现自动安装插件的。

- 通过注册表安装，如Adobe、EndNote，具体目录如下：
```cmd
HKLM\SOFTWARE\Google\Chrome\Extensions 
```
```cmd
HKLM\SOFTWARE\WOW6432Node\Google\Chrome\Extensions
```
```cmd
HKU\S-X-X-XX-XXXXXXXXXX-XXXXXXXXXX-XXXXXXXXX-XXXX\Software\Google\Chrome\Extensions
```

- 通过默认目录安装，如某雷软件在**ChromeExtensionCache**内直接保存`crx文件`，具体目录如下：
```cmd
C:\Users\%UserName%\AppData\Local\ChromeExtensionCache
```