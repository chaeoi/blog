---
title: "修复SOLIDWORKS安装时的VBA错误"
date: 2021-05-27
lastmod: 2021-05-27
author: ["沧海"]
tags: ["SOLIDWORKS"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
- 安装**SOLIDWORKS 2021 SP2**后，卸载安装之前版本，会卡在安装**VBA 7.1**，并提示<u>A newer version of this application is already installed</u>
- 管理员身份打开CMD，运行如下命令即可解决
```cmd
msiexec.exe /X {90120064-0070-0000-0000-4000000FF1CE} /q
msiexec.exe /X {90F60409-7000-11D3-8CFE-0150048383C9} /q
msiexec.exe /X {90F60404-7000-11D3-8CFE-0150048383C9} /q
msiexec.exe /X {90F60407-7000-11D3-8CFE-0150048383C9} /q
msiexec.exe /X {90F6040C-7000-11D3-8CFE-0150048383C9} /q
msiexec.exe /X {90F60410-7000-11D3-8CFE-0150048383C9} /q
msiexec.exe /X {90F60411-7000-11D3-8CFE-0150048383C9} /q
msiexec.exe /X {90F60412-7000-11D3-8CFE-0150048383C9} /q
msiexec.exe /X {90F60416-7000-11D3-8CFE-0150048383C9} /q
msiexec.exe /X {90F60804-7000-11D3-8CFE-0150048383C9} /q
msiexec.exe /X {90F60C0A-7000-11D3-8CFE-0150048383C9} /q
msiexec.exe /X {07C0BFE1-3291-409C-B96A-797340719C8F} /q
msiexec.exe /X {826216B1-0F04-409B-A33E-C6A004AA1097} /q
msiexec.exe /X {B07D817E-C885-4F06-9AB3-E7C6B9D4BBC8} /q
msiexec.exe /X {3577C582-28BF-47CF-A134-0F4E2C3A6148} /q
msiexec.exe /X {B7526322-7FED-4F4F-8F17-E3ACE9E0D725} /q
msiexec.exe /X {B8702FFD-CD7E-4A33-A863-DCD32CF2558F} /q
msiexec.exe /X {47B42A58-D3A9-4A09-B10C-DFBF38665B92} /q
msiexec.exe /X {33A11EB1-D1F7-4874-9A8C-280084753911} /q
msiexec.exe /X {2A6AD565-3D10-4326-9C07-9D469946A0A2} /q
msiexec.exe /X {E4E1AAF7-A590-4DD7-85CE-C5B542677D2F} /q
msiexec.exe /X {7DBDBAC5-B994-467B-9D5F-EA427F2A5212} /q
```