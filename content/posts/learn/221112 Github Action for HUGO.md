---
title: "利用Github Action发布HUGO博客"
date: 2022-11-12
lastmod: 2022-11-13
author: ["沧海"]
tags: ["HUGO"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
```py
name: deploy

on:
    push:
    workflow_dispatch:
    
jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout
              uses: actions/checkout@v2
              with:
                  submodules: true
                  fetch-depth: 0

            - name: Setup Hugo
              uses: peaceiris/actions-hugo@v2
              with:
                  hugo-version: "latest"

            - name: Build Web
              run: hugo

            - name: Deploy Web
              uses: peaceiris/actions-gh-pages@v3
              with:
                  PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
                  EXTERNAL_REPOSITORY: username/username.github.io
                  PUBLISH_BRANCH: master
                  PUBLISH_DIR: ./public
                  commit_message: ${{ github.event.head_commit.message }}
```
- `PERSONAL_TOKEN`请在**账户设置/开发者设置**中申请，并部署到项目**密钥**中。
- `username/username.github.io`请根据**实际情况**修改。
- 如需自定义域名可将**CNAME**放入`/static/`下。