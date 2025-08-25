---
title: "Win10电源选项失效，两分钟自动睡眠"
date: 2020-06-27
lastmod: 2021-06-27
author: ["沧海"]
tags: ["Win10"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
Win10设置睡眠时间无法成功，仍然两分钟自动睡眠，需要开启高级电源设置解决。
- **Win+R**输入`regedit`进入注册表编辑器，进入如下目录，将**Attributes**值由1改成2
```cmd
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\PowerSettings\238C9FA8-0AAD-41ED-83F4-97BE242C8F20\7bc4a2f9-d8fc-4469-b07b-33eb785aaca0
```
- 打开高级电源设置，将**无人参与系统睡眠超时**改为你想要的时间即可