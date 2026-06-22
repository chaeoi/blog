---
title: "iOS上代码块字体大小不一的解决办法"
date: 2022-03-13
lastmod: 2022-03-13
author: ["沧海"]
tags: ["CSS", "iOS"]
description: "记录一次iPhone上代码块字号异常放大的问题，以及我最后采用的CSS修复办法。"
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

在手机上看自己的博客时，发现`代码块`字号不对。同一篇文章里，正文看起来正常，但代码块有时会突然变大，而且不是所有代码块一起变，是部分代码块明显比别的大一圈，在`iPhone`上看起来非常突兀。主要表现为：

**正文字号正常、代码块字号不统一、部分代码块会被放大、Safari和Chrome都会出现**

解决方法简单粗暴，**直接把`code`的文字缩放固定住**

```css
code {
    text-size-adjust: 100%;
    -ms-text-size-adjust: 100%;
    -moz-text-size-adjust: 100%;
    -webkit-text-size-adjust: 100%;
}
```
