---
title: "老坛酸菜LTSC部分易用功能恢复指南"
date: 2023-10-09
lastmod: 2023-10-09
author: ["沧海​"]
tags: ["Win10"]
description: "Windows LTSC看图片是真受罪啊，不过可以通过Microsoft Store安装照片应用"
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

老坛酸菜是Windows推出的长期服务版，去掉了很多花里胡哨的功能，并且减少更新频率提升稳定性，不少用户选择使用。Windows 10 LTSC 2021外观界面好看，精简流畅，但缺失了如照片、终端、商店等功能，网上的教程杂乱且繁琐，遇到BUG就重装的我总结了一套易用靠谱的流程，希望对你有所帮助。

> 安装终端

终端Windows Terminal可以大幅提升CMD、PowerShell等工具的使用体验，微软官方在[Github开源](https://github.com/microsoft/terminal)，可直接下载。下面是安装步骤的简单介绍

- 下载**PreinstallKit**包，解压，在文件夹内使用`Add-AppxPackage`安装**Microsoft.UI.Xaml**，Tab键可快速选择文件

- 下载**msixbundle**结尾的Microsoft.WindowsTerminal安装包，用同样方式安装

> 安装Microsoft Store

这里有一个奇淫技巧，可以打开刚刚安装的终端，输入`wsreset -i`，不用管报错，稍等十几秒，接着，你就会发现商店回来了

> 安装照片

- 商店搜索[Microsoft 照片](https://www.microsoft.com/store/productId/9WZDNCRFJBH4?ocid=pdpshare),点击安装，稍等片刻

- 搜索[HFIF 图像扩展](https://www.microsoft.com/store/productId/9PMMSR1CGPWG?ocid=pdpshare),点击安装

- [HEVC视频扩展](https://www.microsoft.com/store/productId/9NMZLZ57R3T7?ocid=pdpshare)售价7元，可通过在[老毛子网站](https://store.rg-adguard.net/)下载[来自设备制造商的 HEVC 视频扩展](https://www.microsoft.com/store/productId/9N4WGH0Z6VHQ?ocid=pdpshare)代替使用