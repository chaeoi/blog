---
title: "Microsoft Edge“由你的组织管理”如何解决"
date: 2023-12-30
lastmod: 2023-12-30
author: ["沧海"]
tags: ["Edge", "Win10"]
description: "Edge 显示“由你的组织管理”时，通常是本机存在浏览器策略项。本文记录最直接的排查和清理方式。"
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---


Edge 出现“由你的组织管理”并不一定真的是公司设备，很多时候只是本机被写入了浏览器策略项。

#### 先确认是不是策略导致

在地址栏输入：

```text
edge://policy
```

如果页面里确实出现了已配置策略，那基本就能确定问题来源。

#### 清理注册表中的策略项

打开注册表编辑器，在地址栏依次检查下面两个位置：

```text
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft
HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft
```

如果左侧存在和 `Edge` 相关的策略项，删除对应文件夹即可。

#### 让策略重新加载

删除后重新打开：

```text
edge://policy
```

然后点击“重新加载策略”确认是否恢复正常。

#### 如果还是不行

可以继续排查下面几类软件：

- 杀毒软件
- 广告软件或流氓软件
- 最近新装的系统工具
- 会注入浏览器策略的企业软件或同步工具

把可疑软件卸载后，再重复上面的清理步骤。

#### 备注

补充一个一键脚本

```bat
@echo off
chcp 65001 >nul

net session >nul 2>&1
if %errorlevel% neq 0 (
    powershell -Command "Start-Process -FilePath '%~f0' -Verb runAs"
    exit /b
)

reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge" >nul 2>&1
if %errorlevel% equ 0 (
    reg delete "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge" /f >nul 2>&1
    if %errorlevel% equ 0 (
        echo 已删除: HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge
    ) else (
        echo 删除失败: HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge
    )
) else (
    echo 路径不存在: HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge
)

reg query "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Edge" >nul 2>&1
if %errorlevel% equ 0 (
    reg delete "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Edge" /f >nul 2>&1
    if %errorlevel% equ 0 (
        echo 已删除: HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Edge
    ) else (
        echo 删除失败: HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Edge
    )
) else (
    echo 路径不存在: HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Edge
)

echo 已清除所有Edge策略。

timeout /t 5 /nobreak >nul
exit
```
