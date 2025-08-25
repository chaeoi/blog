---
title: "利用ADB给PIC-AL00加速"
date: 2020-09-13
lastmod: 2020-09-13
author: ["沧海"]
tags: ["ADB"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
- 下载**ADB驱动**，打开目录
- 将以下代码保存成`bat`在目录中执行，即可卸载冗余应用，卸载内容根据需要**自行修改**
```c
@echo off 
echo Uninstalling, please wait...
adb shell pm uninstall --user 0 com.huawei.fastapp
adb shell pm uninstall --user 0 com.ibimuyu.lockscreen
adb shell pm uninstall --user 0 com.meizu.flymelab
adb shell pm uninstall --user 0 com.meizu.creator.launcher
adb shell pm uninstall --user 0 com.meizu.picker
adb shell pm uninstall --user 0 com.meizu.account.pay
adb shell pm uninstall --user 0 com.meizu.cloud
adb shell pm uninstall --user 0 com.meizu.net.search
adb shell pm uninstall --user 0 com.youku.phone.player.meizu
adb shell pm uninstall --user 0 com.flyme.roamingpay
adb shell pm uninstall --user 0 com.meizu.flyme.directservice
adb shell pm uninstall --user 0 com.android.browser
adb shell pm uninstall --user 0 com.huawei.wallet
adb shell pm uninstall --user 0 com.huawei.hifolder
adb shell pm uninstall --user 0 com.huawei.intelligent
adb shell pm uninstall --user 0 com.huawei.himovie
adb shell pm uninstall --user 0 com.huawei.phoneservice
adb shell pm uninstall --user 0 com.huawei.hwdetectrepair
adb shell pm uninstall --user 0 com.android.mediacenter
adb shell pm uninstall --user 0 com.huawei.android.findmyphone
adb shell pm uninstall --user 0 com.huawei.contactscamcard
adb shell pm uninstall --user 0 com.android.wallpaper.livepicker
adb shell pm uninstall --user 0 com.huawei.nlp
adb shell pm uninstall --user 0 com.huawei.tips
adb shell pm uninstall --user 0 com.huawei.decision
adb shell pm uninstall --user 0 com.huawei.rcsserviceapplication
adb shell pm uninstall --user 0 com.huawei.android.FloatTasks
adb shell pm uninstall --user 0 com.huawei.vassistant
adb shell pm uninstall --user 0 com.android.dreams.phototable
adb shell pm uninstall --user 0 com.huawei.hiview
adb shell pm uninstall --user 0 com.android.keychain
adb shell pm uninstall --user 0 com.huawei.trustagent
adb shell pm uninstall --user 0 com.huawei.android.projectmenu
adb shell pm uninstall --user 0 com.huawei.hilink.framework
adb shell pm uninstall --user 0 com.huawei.trustspace
adb shell pm uninstall --user 0 com.huawei.watch.sync
adb shell pm uninstall --user 0 com.huawei.parentcontrol
adb shell pm uninstall --user 0 com.android.keyguard
adb shell pm uninstall --user 0 com.android.printspooler
adb shell pm uninstall --user 0 com.huawei.android.mirrorshare
adb shell pm uninstall --user 0 com.huawei.android.instantshare
adb shell pm uninstall --user 0 com.android.bips
adb shell pm uninstall --user 0 com.iflytek.speechsuite
adb shell pm uninstall --user 0 com.android.stk
adb shell pm uninstall --user 0 com.huawei.securitymgr
adb shell pm uninstall --user 0 com.huawei.android.internal.app
adb shell pm uninstall --user 0 com.android.htmlviewer
adb shell pm uninstall --user 0 com.android.exchange
adb shell pm uninstall --user 0 com.huawei.motionservice
adb shell pm uninstall --user 0 com.android.dreams.basic
adb shell pm uninstall --user 0 com.huawei.android.airsharing
adb shell pm uninstall --user 0 com.android.carrierconfig
adb shell pm uninstall --user 0 com.huawei.hiaction
adb shell pm uninstall --user 0 com.android.emergency
adb shell pm uninstall --user 0 com.huawei.screenrecorder
adb shell pm uninstall --user 0 com.android.apps.tag
adb shell pm uninstall --user 0 com.huawei.recsys
adb shell pm uninstall --user 0 com.android.inputdevices
adb shell pm uninstall --user 0 com.android.managedprovisioning
adb shell pm uninstall --user 0 com.huawei.KoBackup
adb shell pm uninstall --user 0 com.amap.android.ams
adb shell pm uninstall --user 0 com.huawei.android.karaoke
adb shell pm uninstall --user 0 com.huawei.msdp
adb shell pm uninstall --user 0 com.huawei.android.hwupgradeguide
adb shell pm uninstall --user 0 com.huawei.gardenlivingwallpaper
adb shell pm uninstall --user 0 com.huawei.oceanlivingwallpaper
adb shell pm uninstall --user 0 com.huawei.novalivingwallpaper
adb shell pm uninstall --user 0 com.google.android.marvin.talkback
adb shell pm uninstall --user 0 com.huawei.hitouch
adb shell pm uninstall --user 0 com.huawei.scanner
adb shell pm uninstall --user 0 com.huawei.contentsensor
adb shell pm uninstall --user 0 com.huawei.android.hwpay
adb shell pm uninstall --user 0 com.huawei.videoeditor
adb shell pm uninstall --user 0 com.UCMobile
adb shell pm uninstall --user 0 com.taobao.taobao
adb shell pm uninstall --user 0 com.huawei.educenter
adb shell pm uninstall --user 0 com.huawei.welinknow
adb shell pm uninstall --user 0 com.huawei.search
adb shell pm uninstall --user 0 com.huawei.hwvplayer.youku
adb shell pm uninstall --user 0 com.huawei.bone
adb shell pm uninstall --user 0 com.huawei.smarthome
adb shell pm uninstall --user 0 com.vmall.client
adb shell pm uninstall --user 0 com.huawei.android.tips
adb shell pm uninstall --user 0 com.huawei.lives
adb shell pm uninstall --user 0 com.android.hwmirror
adb shell pm uninstall --user 0 com.hicloud.android.clone
adb shell pm uninstall --user 0 com.android.email
adb shell pm uninstall --user 0 com.example.android.notepad
adb shell pm uninstall --user 0 com.huawei.hiskytone
adb shell pm uninstall --user 0 com.huawei.vdrive
adb shell pm uninstall --user 0 com.huawei.fans
adb shell pm uninstall --user 0 cn.wps.moffice_eng
adb shell pm uninstall --user 0 com.sina.weibo
adb shell pm uninstall --user 0 com.achievo.vipshop
adb shell pm uninstall --user 0 com.ss.android.article.news
adb shell pm uninstall --user 0 com.tencent.news
adb shell pm uninstall --user 0 com.Qunar
adb shell pm uninstall --user 0 com.sankuai.meituan
adb shell pm uninstall --user 0 com.jingdong.app.mall
adb shell pm uninstall --user 0 com.huawei.hwireader
adb shell pm uninstall --user 0 com.huawei.gamebox
adb shell pm uninstall --user 0 com.baidu.searchbox
adb shell pm uninstall --user 0 com.autonavi.minimap
adb shell pm uninstall --user 0 com.huawei.gameassistant
echo Uninstall complete!
echo. & pause
```
