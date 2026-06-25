---
title: "SOLIDWORKS开启RealView"
date: 2026-05-09
lastmod: 2026-05-09
author: ["沧海"]
tags: ["SOLIDWORKS"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

如果 SOLIDWORKS 在新显卡上出现显示异常、RealView 不可用等问题，可以尝试给当前显卡添加 OpenGL 显卡白名单。这里记录的是`NVIDIA GeForce RTX 4080 SUPER/PCIe/SSE2`对应的注册表。

新建一个`solidworks.reg`文件，写入以下内容：

```reg
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\SolidWorks\AllowList\Gl2Shaders\NV40\NVIDIA GeForce RTX 4080 SUPER/PCIe/SSE2]
"Workarounds"=dword:32408
```

双击导入，或者在 CMD 中执行：

```cmd
reg import solidworks.reg
```

导入后重启 SOLIDWORKS 生效。

需要注意，注册表路径里的显卡名称要和本机识别到的 OpenGL 显卡名称一致。如果不是`NVIDIA GeForce RTX 4080 SUPER/PCIe/SSE2`，需要把路径中的显卡名称改成自己的显卡名称。
