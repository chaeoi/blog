---
title: "利用ADB给STF-AL00加速"
date: 2020-09-12
lastmod: 2020-09-12
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
adb shell pm uninstall --user 0 com.huawei.android.hwouc
adb shell pm uninstall --user 0 com.tmall.wireless
adb shell pm uninstall --user 0 com.taobao.taobao
adb shell pm uninstall --user 0 com.huawei.hnreader
adb shell pm uninstall --user 0 com.baidu.searchbox
adb shell pm uninstall --user 0 com.android.browser
adb shell pm uninstall --user 0 com.Qunar
adb shell pm uninstall --user 0 com.ss.android.article.news
adb shell pm uninstall --user 0 cn.wps.moffice_eng
adb shell pm uninstall --user 0 com.baidu.BaiduMap
adb shell pm uninstall --user 0 com.tencent.news
adb shell pm uninstall --user 0 com.huawei.gamebox
adb shell pm uninstall --user 0 com.huawei.health
adb shell pm uninstall --user 0 com.sina.weibo
adb shell pm uninstall --user 0 com.jingdong.app.mall
adb shell pm uninstall --user 0 com.huawei.skytone
adb shell pm uninstall --user 0 com.huawei.android.remotecontroller
adb shell pm uninstall --user 0 com.eazer.app.huawei
adb shell pm uninstall --user 0 co.spe3d.paipai_huawei
adb shell pm uninstall --user 0 com.huawei.gameassistant
adb shell pm uninstall --user 0 com.huawei.hwvplayer.youku
adb shell pm uninstall --user 0 com.huawei.bone
adb shell pm uninstall --user 0 com.huawei.smarthome
adb shell pm uninstall --user 0 com.huawei.contactscamcard
adb shell pm uninstall --user 0 com.huawei.watch.sync
adb shell pm uninstall --user 0 com.huawei.android.instantshare
adb shell pm uninstall --user 0 com.iflytek.speechsuite
adb shell pm uninstall --user 0 com.android.htmlviewer
adb shell pm uninstall --user 0 com.huawei.securitymgr
adb shell pm uninstall --user 0 com.android.printspooler
adb shell pm uninstall --user 0 com.huawei.nlp
adb shell pm uninstall --user 0 com.huawei.vassistant
adb shell pm uninstall --user 0 com.huawei.hitouch
adb shell pm uninstall --user 0 com.huawei.android.FloatTasks
adb shell pm uninstall --user 0 com.huawei.hifolder
adb shell pm uninstall --user 0 com.google.android.marvin.talkback
adb shell pm uninstall --user 0 com.huawei.tips
adb shell pm uninstall --user 0 com.huawei.android.projectmenu
adb shell pm uninstall --user 0 com.android.wallpaper.livepicker
adb shell pm uninstall --user 0 com.huawei.contentsensor
adb shell pm uninstall --user 0 com.huawei.trustspace
adb shell pm uninstall --user 0 com.android.keyguard
adb shell pm uninstall --user 0 com.huawei.android.findmyphone
adb shell pm uninstall --user 0 com.huawei.hilink.framework
adb shell pm uninstall --user 0 com.huawei.parentcontrol
adb shell pm uninstall --user 0 com.huawei.videoeditor
adb shell pm uninstall --user 0 com.android.bips
adb shell pm uninstall --user 0 com.android.dreams.phototable
adb shell pm uninstall --user 0 com.huawei.android.mirrorshare
adb shell pm uninstall --user 0 com.huawei.rcsserviceapplication
adb shell pm uninstall --user 0 com.huawei.android.tips
adb shell pm uninstall --user 0 com.android.dreams.basic
adb shell pm uninstall --user 0 com.huawei.android.airsharing
adb shell pm uninstall --user 0 com.android.hwmirror
adb shell pm uninstall --user 0 com.vmall.client
adb shell pm uninstall --user 0 com.stupeflix.replay
adb shell pm uninstall --user 0 com.huawei.lives
adb shell pm uninstall --user 0 com.hicloud.android.clone
adb shell pm uninstall --user 0 com.huawei.intelligent
adb shell pm uninstall --user 0 com.android.apps.tag
adb shell pm uninstall --user 0 com.huawei.hiaction
adb shell pm uninstall --user 0 com.example.android.notepad
adb shell pm uninstall --user 0 com.android.exchange
adb shell pm uninstall --user 0 com.android.emergency
adb shell pm uninstall --user 0 com.huawei.android.totemweatherwidget
adb shell pm uninstall --user 0 com.huawei.scanner
adb shell pm uninstall --user 0 com.huawei.motionservice
adb shell pm uninstall --user 0 com.android.mediacenter
adb shell pm uninstall --user 0 com.google.android.backuptransport
adb shell pm uninstall --user 0 com.android.email
adb shell pm uninstall --user 0 com.huawei.screenrecorder
adb shell pm uninstall --user 0 com.android.inputdevices
adb shell pm uninstall --user 0 com.huawei.himovie
adb shell pm uninstall --user 0 com.huawei.phoneservice
adb shell pm uninstall --user 0 com.huawei.wallet
adb shell pm uninstall --user 0 com.huawei.KoBackup
adb shell pm uninstall --user 0 com.huawei.hiskytone
adb shell pm uninstall --user 0 com.huawei.fans
adb shell pm uninstall --user 0 com.huawei.vdrive
adb shell pm uninstall --user 0 com.huawei.msdp
adb shell pm uninstall --user 0 com.huawei.recsys
adb shell pm uninstall --user 0 com.huawei.bd
adb shell pm uninstall --user 0 com.huawei.android.karaoke
adb shell pm uninstall --user 0 com.android.stk
adb shell pm uninstall --user 0 com.huawei.hwdetectrepair
adb shell pm uninstall --user 0 com.huawei.android.hwpay
adb shell pm uninstall --user 0 com.huawei.decision
adb shell pm uninstall --user 0 com.android.carrierconfig
adb shell pm uninstall --user 0 com.android.managedprovisioning
adb shell pm uninstall --user 0 com.huawei.hidisk
adb shell pm uninstall --user 0 com.android.documentsui
adb shell pm uninstall --user 0 com.huawei.secime
adb shell pm uninstall --user 0 com.baidu.input_huawei
adb shell pm uninstall --user 0 com.huawei.hiviewtunnel
adb shell pm uninstall --user 0 com.tencent.mtt
adb shell pm uninstall --user 0 com.hihonor.vmall
adb shell pm uninstall --user 0 com.honor.club
adb shell pm uninstall --user 0 com.huawei.welinknow
adb shell pm uninstall --user 0 com.huawei.ohos.smarthome
adb shell pm uninstall --user 0 com.huawei.search
adb shell pm uninstall --user 0 com.huawei.fastapp
adb shell pm uninstall --user 0 com.huawei.hiai
adb shell pm uninstall --user 0 com.huawei.printservice
adb shell pm uninstall --user 0 com.huawei.mediacontroller
adb shell pm uninstall --user 0 com.huawei.hiview
adb shell pm uninstall --user 0 com.huawei.mycenter
adb shell pm uninstall --user 0 com.huawei.ohos.health
adb shell pm uninstall --user 0 cn.honor.qinxuan
adb shell pm uninstall --user 0 com.huawei.pengine
adb shell pm uninstall --user 0 com.huawei.ohos.famanager
adb shell pm uninstall --user 0 com.android.internal.display.cutout.emulation.wide
adb shell pm uninstall --user 0 com.android.internal.display.cutout.emulation.narrow
adb shell pm uninstall --user 0 com.android.internal.display.cutout.emulation.tall
adb shell pm uninstall --user 0 com.huawei.meetime
adb shell pm uninstall --user 0 com.huawei.ohos.suggestion.hm.overlay
adb shell pm uninstall --user 0 com.huawei.hicloud
adb shell pm uninstall --user 0 com.huawei.searchservice
adb shell pm uninstall --user 0 com.huawei.educenter
adb shell pm uninstall --user 0 com.huawei.hwvoipservice
adb shell pm uninstall --user 0 com.huawei.ohos.search
adb shell pm uninstall --user 0 com.huawei.suggestion
adb shell pm uninstall --user 0 com.huawei.audioaccessorymanager
adb shell pm uninstall --user 0 com.huawei.ohos.suggestion.overlay
adb shell pm uninstall --user 0 com.huawei.ohos.suggestion
adb shell pm uninstall --user 0 com.huawei.pcassistant
adb shell pm uninstall --user 0 com.huawei.controlcenter
adb shell pm uninstall --user 0 com.unionpay.tsmservice
adb shell pm uninstall --user 0 com.huawei.ohos.inputmethod
adb shell pm uninstall --user 0 com.huawei.featurelayer.sharedfeature.map
adb shell pm uninstall --user 0 com..hihonor.vmall
adb shell pm uninstall --user 0 com.huawei.hicard
adb shell pm uninstall --user 0 com.huawei.android.hwupgradeguide
adb shell pm uninstall --user 0 com.android.calendar
adb shell pm uninstall --user 0 com.android.providers.calendar
adb shell pm uninstall --user 0 com.huawei.android.totemweather
adb shell pm uninstall --user 0 com.android.keychain
adb shell pm uninstall --user 0 com.huawei.trustagent
adb shell pm uninstall --user 0 com.google.android.printservice.recommendation
adb shell pm uninstall --user 0 com.huawei.hwstartupguide
adb shell pm uninstall --user 0 com.google.android.syncadapters.contacts
adb shell pm uninstall --user 0 com.android.wallpaperbackup
echo Uninstall complete!
echo. & pause
```
- 卸载谷歌套件
```c
@echo off 
echo Uninstalling, please wait...
adb shell pm uninstall --user 0 com.android.vending
adb shell pm uninstall --user 0 com.google.android.partnersetup
adb shell pm uninstall --user 0 com.google.android.gsf
adb shell pm uninstall --user 0 com.google.android.overlay.gmsconfig
adb shell pm uninstall --user 0 com.google.android.gms.policy_sidecar_aps
adb shell pm uninstall --user 0 com.google.android.onetimeinitializer
adb shell pm uninstall --user 0 com.google.android.gms
echo Uninstall complete!
echo. & pause
```
