---
title: "利用ADB给Picasso加速"
date: 2022-03-24
lastmod: 2022-03-24
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
adb shell pm uninstall --user 0 com.miui.notes
adb shell pm uninstall --user 0 com.miui.phrase
adb shell pm uninstall --user 0 com.android.printspooler
adb shell pm uninstall --user 0 com.miui.miwallpaper.earth
adb shell pm uninstall --user 0 com.android.email
adb shell pm uninstall --user 0 com.miui.miservice
adb shell pm uninstall --user 0 com.miui.miwallpaper.mars
adb shell pm uninstall --user 0 com.android.emergency
adb shell pm uninstall --user 0 com.miui.miwallpaper.geometry
adb shell pm uninstall --user 0 com.mi.health
adb shell pm uninstall --user 0 com.android.provision
adb shell pm uninstall --user 0 com.mi.globalbrowser
adb shell pm uninstall --user 0 com.xiaomi.payment
adb shell pm uninstall --user 0 com.android.keychain
adb shell pm uninstall --user 0 com.miui.screenrecorder
adb shell pm uninstall --user 0 com.xiaomi.scanner
adb shell pm uninstall --user 0 com.miui.fm
adb shell pm uninstall --user 0 com.miui.fmservice
adb shell pm uninstall --user 0 com.android.quicksearchbox
adb shell pm uninstall --user 0 com.miui.miwallpaper.miweatherwallpaper
adb shell pm uninstall --user 0 com.milink.service
adb shell pm uninstall --user 0 com.xiaomi.miplay_client
adb shell pm uninstall --user 0 com.miui.miwallpaper.saturn
adb shell pm uninstall --user 0 com.miui.maintenancemode
adb shell pm uninstall --user 0 com.android.cellbroadcastreceiver
adb shell pm uninstall --user 0 com.android.bips
adb shell pm uninstall --user 0 com.android.traceur
adb shell pm uninstall --user 0 com.xiaomi.mi_connect_service
adb shell pm uninstall --user 0 com.mfashiongallery.emag
adb shell pm uninstall --user 0 com.miui.newmidrive
adb shell pm uninstall --user 0 com.miui.miwallpaper.snowmountain
adb shell pm uninstall --user 0 com.google.android.soundpicker
adb shell pm uninstall --user 0 com.miui.misound
adb shell pm uninstall --user 0 com.google.android.feedback
adb shell pm uninstall --user 0 com.miui.bugreport
adb shell pm uninstall --user 0 com.android.providers.userdictionary
adb shell pm uninstall --user 0 com.android.carrierdefaultapp
adb shell pm uninstall --user 0 com.miui.personalassistant
adb shell pm uninstall --user 0 com.google.android.setupwizard
adb shell pm uninstall --user 0 com.google.android.apps.restore
adb shell pm uninstall --user 0 com.google.android.projection.gearhead
adb shell pm uninstall --user 0 com.android.bookmarkprovider
adb shell pm uninstall --user 0 com.android.calllogbackup
adb shell pm uninstall --user 0 com.bsp.catchlog
adb shell pm uninstall --user 0 com.miui.cit
adb shell pm uninstall --user 0 com.miui.qr
adb shell pm uninstall --user 0 com.google.android.inputmethod.latin
adb shell pm uninstall --user 0 com.google.android.googlequicksearchbox
adb shell pm uninstall --user 0 com.google.android.syncadapters.calendar
adb shell pm uninstall --user 0 com.google.android.partnersetup
adb shell pm uninstall --user 0 com.google.android.syncadapters.contacts
adb shell pm uninstall --user 0 com.google.ar.lens
adb shell pm uninstall --user 0 com.android.wallpaper.livepicker
adb shell pm uninstall --user 0 com.miui.videoplayer
adb shell pm uninstall --user 0 com.xiaomi.mirror
adb shell pm uninstall --user 0 com.xiaomi.mtb
adb shell pm uninstall --user 0 com.android.musicfx
adb shell pm uninstall --user 0 com.android.hotwordenrollment.okgoogle
adb shell pm uninstall --user 0 com.android.apps.tag
adb shell pm uninstall --user 0 com.android.stk
adb shell pm uninstall --user 0 com.android.hotwordenrollment.xgoogle
adb shell pm uninstall --user 0 com.qti.xdivert
adb shell pm uninstall --user 0 com.miui.mishare.connectivity
adb shell pm uninstall --user 0 com.miui.huanji
adb shell pm uninstall --user 0 com.google.android.tts
adb shell pm uninstall --user 0 com.UCMobile
adb shell pm uninstall --user 0 com.iflytek.inputmethod.miui
adb shell pm uninstall --user 0 com.dragon.read
adb shell pm uninstall --user 0 com.duokan.phone.remotecontroller
adb shell pm uninstall --user 0 com.baidu.input_mi
adb shell pm uninstall --user 0 tv.danmaku.bili
adb shell pm uninstall --user 0 com.baidu.searchbox
adb shell pm uninstall --user 0 com.xiaomi.smarthome
adb shell pm uninstall --user 0 com.duokan.reader
adb shell pm uninstall --user 0 com.ss.android.article.news
adb shell pm uninstall --user 0 com.mi.liveassistant
adb shell pm uninstall --user 0 com.xiaomi.jr
adb shell pm uninstall --user 0 com.miui.virtualsim
adb shell pm uninstall --user 0 com.miui.thirdappassistant
adb shell pm uninstall --user 0 com.miui.newhome
adb shell pm uninstall --user 0 com.miui.smarttravel
adb shell pm uninstall --user 0 com.taobao.taobao
adb shell pm uninstall --user 0 com.xiaomi.youpin
adb shell pm uninstall --user 0 com.xiaomi.gamecenter
adb shell pm uninstall --user 0 com.xiaomi.shop
adb shell pm uninstall --user 0 com.xiaomi.vipaccount
adb shell pm uninstall --user 0 com.eastmoney.android.berlin
adb shell pm uninstall --user 0 com.zhihu.android
adb shell pm uninstall --user 0 com.sina.weibo
adb shell pm uninstall --user 0 com.xiaomi.mibrain.speech
adb shell pm uninstall --user 0 com.duokan.free
adb shell pm uninstall --user 0 com.xunmeng.pinduoduo
adb shell pm uninstall --user 0 com.miui.userguide
adb shell pm uninstall --user 0 com.ss.android.ugc.aweme
adb shell pm uninstall --user 0 com.eg.android.AlipayGphone
adb shell pm uninstall --user 0 com.xiaomi.drivemode
adb shell pm uninstall --user 0 com.xiaomi.aiasst.vision
adb shell pm uninstall --user 0 com.xiaomi.aiasst.service
adb shell pm uninstall --user 0 com.google.android.marvin.talkback
adb shell pm uninstall --user 0 com.android.theme.color.black
adb shell pm uninstall --user 0 com.android.theme.color.cinnamon
adb shell pm uninstall --user 0 com.android.theme.color.green
adb shell pm uninstall --user 0 com.android.theme.color.ocean
adb shell pm uninstall --user 0 com.android.theme.color.orchid
adb shell pm uninstall --user 0 com.android.theme.color.purple
adb shell pm uninstall --user 0 com.android.theme.color.space
adb shell pm uninstall --user 0 com.android.protips
adb shell pm uninstall --user 0 com.miui.contentextension
adb shell pm uninstall --user 0 com.android.dreams.basic
adb shell pm uninstall --user 0 com.miui.voiceassist
adb shell pm uninstall --user 0 com.xiaomi.ab
adb shell pm uninstall --user 0 com.miui.accessibility
adb shell pm uninstall --user 0 com.miui.hybrid
adb shell pm uninstall --user 0 com.miui.touchassistant
adb shell pm uninstall --user 0 com.miui.hybrid.accessory
adb shell pm uninstall --user 0 com.miui.systemAdSolution
adb shell pm uninstall --user 0 com.xiaomi.gamecenter.sdk.service
adb shell pm uninstall --user 0 com.android.dreams.phototable
adb shell pm uninstall --user 0 com.miui.voicetrigger
adb shell pm uninstall --user 0 com.unionpay.tsmservice.mi
adb shell pm uninstall --user 0 com.android.browser
adb shell pm uninstall --user 0 com.miui.translation.kingsoft
adb shell pm uninstall --user 0 com.miui.translation.youdao
adb shell pm uninstall --user 0 com.xiaomi.joyose
adb shell pm uninstall --user 0 com.xiaomi.mircs
adb shell pm uninstall --user 0 com.miui.securityinputmethod
adb shell pm uninstall --user 0 com.android.managedprovisioning
adb shell pm uninstall --user 0 com.miui.analytics
adb shell pm uninstall --user 0 com.miui.vpnsdkmanager
echo Uninstall complete!
echo. & pause
```
