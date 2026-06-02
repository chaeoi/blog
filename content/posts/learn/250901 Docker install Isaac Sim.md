---
title: "使用Docker安装Isaac Sim"
date: 2025-09-01
lastmod: 2025-09-01
author: ["沧海"]
tags: ["Docker", "Isaac Sim", "NVIDIA"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

`Isaac Sim`可以通过 NVIDIA 官方容器镜像运行。这里记录一下在 Docker 中安装 NVIDIA Container Toolkit，并以 Headless 模式启动`Isaac Sim 5.0.0`。

操作前需要先确认宿主机已经安装 NVIDIA 驱动，并且`nvidia-smi`可以正常看到显卡。

#### 配置 NVIDIA Container Toolkit 仓库

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | \
sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && \
curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list && \
sudo apt-get update
```

#### 安装 NVIDIA Container Toolkit

```bash
sudo apt-get install -y nvidia-container-toolkit
```

重启 Docker：

```bash
sudo systemctl restart docker
```

#### 配置 Docker 使用 NVIDIA Runtime

```bash
sudo nvidia-ctk runtime configure --runtime=docker
```

再次重启 Docker：

```bash
sudo systemctl restart docker
```

可以先测试一下 Docker 是否能调用显卡：

```bash
docker run --rm --gpus all nvidia/cuda:12.4.1-base-ubuntu22.04 nvidia-smi
```

#### 启动 Isaac Sim

```bash
docker run -d \
    --name isaac-sim \
    --restart always \
    --network=host \
    --gpus all \
    --runtime=nvidia \
    -e "ACCEPT_EULA=Y" \
    -e "PRIVACY_CONSENT=Y" \
    -v /opt/isaac-sim/cache/kit:/isaac-sim/kit/cache:rw \
    -v /opt/isaac-sim/cache/ov:/root/.cache/ov:rw \
    -v /opt/isaac-sim/cache/pip:/root/.cache/pip:rw \
    -v /opt/isaac-sim/cache/glcache:/root/.cache/nvidia/GLCache:rw \
    -v /opt/isaac-sim/cache/computecache:/root/.nv/ComputeCache:rw \
    -v /opt/isaac-sim/logs:/root/.nvidia-omniverse/logs:rw \
    -v /opt/isaac-sim/data:/root/.local/share/ov/data:rw \
    -v /opt/isaac-sim/documents:/root/Documents:rw \
    nvcr.io/nvidia/isaac-sim:5.0.0 ./runheadless.sh -v
```

其中：

- `--gpus all`表示把所有 GPU 暴露给容器
- `--runtime=nvidia`表示使用 NVIDIA 容器运行时
- `ACCEPT_EULA=Y`表示接受 Isaac Sim EULA
- `PRIVACY_CONSENT=Y`表示接受隐私数据收集选项
- `/opt/isaac-sim/cache`用于持久化各类缓存
- `/opt/isaac-sim/logs`用于保存日志
- `/opt/isaac-sim/data`和`/opt/isaac-sim/documents`用于保存运行数据和文档

查看容器状态：

```bash
docker ps | grep isaac-sim
```

查看日志：

```bash
docker logs -f isaac-sim
```

如果需要重新创建容器：

```bash
docker stop isaac-sim
docker rm isaac-sim
```
