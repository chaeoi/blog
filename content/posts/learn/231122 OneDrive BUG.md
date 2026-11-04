---
title: "解决OneDrive Business和Personal同时自启"
date: 2023-11-22
lastmod: 2023-11-22
author: ["沧海"]
tags: ["OneDrive"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
该问题在2023年下半年OneDrive更新后出现，当登陆了Business账户但没有登录个人账户时，每次开机都会在任务栏右下角托盘出现**两个OneDrive图标**，一个是蓝色的OneDrive for Business，一个是灰色的OneDrive for Personal。即使你关闭开机自启手动打开OneDrive，也会同时启动两个界面。虽然不影响使用，但强迫症很难受，解决方法如下：

- 进入注册表编辑器打开`HKEY_CURRENT_USER\SOFTWARE\Microsoft\OneDrive`

- 在左侧OneDrive文件夹右键新建**DWORD**并将其命名为`DisablePersonalSync`，数值改为十六进制1


附上一个简单的一键脚本，用于快速修改注册表。运行后请手动重启 OneDrive：

```cmd
@echo off
chcp 65001 >nul
title 解决 OneDrive 双图标与自启问题
echo 正在写入注册表，禁用 OneDrive 个人版同步...

reg add "HKCU\Software\Microsoft\OneDrive" /v DisablePersonalSync /t REG_DWORD /d 1 /f
if errorlevel 1 (
    echo 注册表写入失败。
    pause
    exit /b 1
)

echo.
echo 注册表写入完成，请手动重启 OneDrive。
pause
```
