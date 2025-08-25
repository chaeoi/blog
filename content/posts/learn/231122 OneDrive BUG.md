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