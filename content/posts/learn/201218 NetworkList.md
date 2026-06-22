---
title: "删除Win10连接过的网络名"
date: 2020-12-18
lastmod: 2020-12-20
author: ["沧海"]
tags: ["Windows"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
路由器刷固件后，SSID不变。笔记本再连接就会从openwrt变成openwrt2。强迫症表示完全受不了！可以通过清理注册表解决。
- **win+R** 输入`regedit`进入注册表编辑器。
- 删除下列目录中所有子项
- 重启电脑。
```c
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles
```
```c
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged
```
终于通过清理注册表后，使其恢复到默认状态。


补充一个一键脚本
```cmd
@echo off
chcp 65001 >nul

net session >nul 2>&1
if %errorlevel% neq 0 (
    powershell -Command "Start-Process -FilePath '%~f0' -Verb runAs"
    exit /b
)

reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles" >nul 2>&1
if %errorlevel% equ 0 (
    for /f "tokens=*" %%A in ('reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles" /s /f "*" /k') do (
        reg delete "%%A" /f >nul 2>&1
        if %errorlevel% equ 0 (
            echo 已删除键: %%A
        ) else (
            echo 删除键失败: %%A
        )
    )
    echo 已删除 Profiles 下的所有子文件夹。
) else (
    echo Profiles 路径不存在。
)

reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged" >nul 2>&1
if %errorlevel% equ 0 (
    for /f "tokens=*" %%A in ('reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged" /s /f "*" /k') do (
        reg delete "%%A" /f >nul 2>&1
        if %errorlevel% equ 0 (
            echo 已删除键: %%A
        ) else (
            echo 删除键失败: %%A
        )
    )
    echo 已删除 Unmanaged 下的所有子文件夹。
) else (
    echo Unmanaged 路径不存在。
)

echo 正在重启所有网络连接...
powershell -Command "Get-NetAdapter | Where-Object { $_.Status -eq 'Up' } | ForEach-Object { Restart-NetAdapter -Name $_.Name -Confirm:$false }"
echo 所有网络连接已重启。

timeout /t 5 /nobreak >nul
exit
```