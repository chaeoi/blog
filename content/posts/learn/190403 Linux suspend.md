---
title: "Linux设置笔记本盒盖不关机"
date: 2019-04-03
lastmod: 2019-04-03
author: ["沧海"]
tags: ["Linux"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

打开配置文件：

```bash
vi /etc/systemd/logind.conf
```

常见会看到这几项：

```ini
#HandlePowerKey=poweroff
#HandleSleepKey=suspend
#HandleHibernateKey=hibernate
#HandleLidSwitch=suspend
```
> 分别解释一下键值
- `HandlePowerKey`
   按下电源键后的行为
- `HandleSleepKey`
   按下睡眠键后的行为
- `HandleHibernateKey`
   按下休眠键后的行为
- `HandleLidSwitch`
   合上笔记本盖后的行为

> 常见可选值

- `ignore`
   忽略，不做任何事
- `poweroff`
   关机
- `reboot`
   重启
- `suspend`
   挂起
- `hibernate`
   休眠
- `lock`
   锁屏


注意取消注释，改完以后执行：

```bash
systemctl restart systemd-logind
```

