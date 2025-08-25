---
title: "利用Github Action发布Docker镜像"
date: 2023-07-07
lastmod: 2025-02-13
author: ["沧海"]
tags: ["Docker"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
```yaml
name: Build Docker Images

on:
  schedule:
    - cron: '0 0 * * *'
  workflow_dispatch:

jobs:
  dockerhub:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: user
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          platforms: linux/amd64,linux/arm64,linux/arm/v7,linux/s390x
          push: true
          tags: user/app:latest
```
- `Access Token`可以通过**Docker Hub**的**Account Settings->Personal access tokens->Cenerate new token**创建，然后通过**GitHub**仓库的**Settings->Actions secrets and variables->New repository secret**创建`DOCKERHUB_TOKEN`。
- `tags: user/app:latest`中的`user`和`app`需要修改为实际的**用户名**和**镜像名**。