---
title: "隐藏Win10系统文件夹"
date: 2023-03-01
lastmod: 2023-03-01
author: ["沧海"]
tags: ["Windows"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
通过修改注册表来隐藏3D对象、视频、图片、文档、音乐等文件夹。
- 打开注册表编辑器逐级定位到如下目录：
```c
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions
```
- 以**3D对象**文件夹为例，在找到`{31c0dd25-9439-4f12-bf41-7ff4eda38722}`，打开`PropertyBag`，选择`ThisPCPolicy`，修改该项属性为`Hide`，即可完成隐藏。
- 其他文件夹目录如下：
	- **3D对象**`{31c0dd25-9439-4f12-bf41-7ff4eda38722}`
	- **视频**`{35286a68-3c57-41a1-bbb1-0eae73d76c95}`
	- **图片**`{0ddd015d-b06c-45d5-8c4c-f59713854639}`
	- **文档**`{f42ee2d3-909f-4907-8871-4c22fc0bf756}`
	- **下载**`{7d83ee9b-2244-4e70-b1f5-5393042af1e4}`
	- **音乐**`{a0c69a99-21c8-4671-8703-7934162fcf1d}`
	- **桌面**`{b4bfcc3a-db2c-424c-b029-7fe99a87c641}`


附上一个简单的一键脚本，用于隐藏几个不常用的文件夹
```cmd
@echo off
title Hide This PC Folders
echo 正在隐藏 此电脑 中的文件夹...

reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{31c0dd25-9439-4f12-bf41-7ff4eda38722}\PropertyBag" /v ThisPCPolicy /t REG_SZ /d Hide /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{35286a68-3c57-41a1-bbb1-0eae73d76c95}\PropertyBag" /v ThisPCPolicy /t REG_SZ /d Hide /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{0ddd015d-b06c-45d5-8c4c-f59713854639}\PropertyBag" /v ThisPCPolicy /t REG_SZ /d Hide /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{a0c69a99-21c8-4671-8703-7934162fcf1d}\PropertyBag" /v ThisPCPolicy /t REG_SZ /d Hide /f

echo.
echo 已完成，正在重启资源管理器...

taskkill /f /im explorer.exe >nul 2>nul
start explorer.exe

echo.
echo 完成。
pause
```