---
title: "快速上手搭建饥荒云服"
date: 2022-11-16
lastmod: 2022-11-16
author: ["沧海"]
tags: ["饥荒"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
#### 生成令牌及信任证书
> Wegame下载安装饥荒专用服务器
- 点击注册许可证书，提示证书生成在饥荒联机版游戏目录的`bin64`中，你需要手动把证书放到饥荒联机版专用服务器目录下的`bin`中。
> 打开饥荒联机版，进入主界面点击左下角**账户**进入Klei账户设置：
- 点击<u>游戏</u>，<u>《饥荒：联机版》的游戏服务器</u>，输入任意英文集群名，点击添加新服务器，复制生成的**令牌**，新建txt文本，将生成的令牌粘贴进去，并将文件命名为`cluster_token.txt`。
- <u>用户信息</u>界面复制**Klei用户ID**，新建txt文本，将用户ID粘贴进去，并命名为`adminlist.txt`，设置自己为管理员。

<center class="half">
    <img src="https://cdn.canghai.org/images/22111601.jpg" alt="22111601" width="415" style="display: unset;"/>
    <img src="https://cdn.canghai.org/images/22111602.jpg" alt="22111602" width="415" style="display: unset;"/>
</center>

#### 生成世界配置
- 进入游戏，新建一个世界，注意一下这是你创建的第几个世界，设置好世界参数勾选好所需模组，即可生成世界，到选人界面退出。
#### 整理文件
- 游戏主界面点击数据，进入游戏配置文件夹，复制`Cluster_1`文件夹，新建的世界是你的第几个就复制数字几，将文件夹粘贴到文档目录中的`Klei\DoNotStarveTogetherRail`中。

<div style="text-align: center;">
<img src="https://cdn.canghai.org/images/22111603.jpg" alt="22111603" width="650" style="display: unset;"/>
</div>

- 将创建的`cluster_token.txt`以及`adminlist.txt`放入`Cluster_1`文件夹中。
- 将游戏目录下`mod`文件夹中的内容复制到饥荒专用服务器目录中的`mod`文件夹中。
#### 开启专用服务器
- Wegame启动饥荒专用服务器，服务器配置列表选择你刚刚复制过去的`Cluster_1`，勾选洞穴，输入证书密码，点击启动，稍等片刻，看到输出**Sim paused**,即搭建完成。

<div style="text-align: center;">
<img src="https://cdn.canghai.org/images/22111604.jpg" alt="22111604" width="800" style="display: unset;"/>
</div>