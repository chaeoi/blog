---
title: "安装与配置Codex、Claude Code"
date: 2025-07-16
lastmod: 2026-07-27
author: ["沧海"]
tags: ["Codex", "Claude Code"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

安装 Codex 和 Claude Code 前，需要先安装 Node.js 和 npm。

> 安装Codex

```bash
npm install -g @openai/codex
```

使用镜像仓库安装

```bash
npm install -g @openai/codex --registry=https://registry.npmmirror.com
```

启动时跳过审批和沙箱：

```bash
codex --yolo
```

> 安装Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

使用镜像仓库安装

```bash
npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com
```

启动时跳过权限确认：

```bash
claude --dangerously-skip-permissions
```

以上参数仅对本次启动生效。它们会跳过审批或权限确认，其中 Codex 还会禁用沙箱；请仅在隔离且可信的环境中使用。
