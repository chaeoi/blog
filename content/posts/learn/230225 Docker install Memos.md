---
title: "Memos之Docker部署"
date: 2023-02-25
lastmod: 2023-02-25
author: ["沧海"]
tags: ["Memos"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
🦄 开源且永久免费  
🚀 支持在几秒钟内使用 `Docker` 自托管  
📜 纯文本优先，并支持一些`Markdown`语法  
👥 将备忘录设置为私人或公开给其他人  
‍💻 完全的`API`支持  
📑 将备忘录设置为私人或公开给其他人  
📆 交互式日历视图  
💾 轻松的数据迁移和备份  
```c
docker run -d \
    --name memos \
    --restart always \
    --network host \
    -e MEMOS_PORT=5009 \
    -v /opt/memos:/var/opt/memos \
    neosmemo/memos:stable
```