---
title: "给MIUI做一个减法"
date: 2022-12-20
lastmod: 2022-12-20
author: ["沧海"]
tags: ["MIUI"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
#### 移除AVB校验
AVB2.0启动校验用来校验系统完整性，修改固件前需要去除校验
- 修改`firmware-update`目录下的**vbmeta.img**以及**vbmeta_system.img**
- 修改`vendor\etc`目录下的**fstab.default**以及**fstab.emmc**
#### 精简冗余应用、冗余可执行文件
- 精简应用前应修改**services.jar**，防止卡米
- 将以下代码保存成`bat`执行，删除冗余应用及冗余可执行文件，可根据需要自行修改
```c
@echo off 
echo Cleaning up, please wait...
rd /s /q D:\ROM\product\app\aiasst_service
rd /s /q D:\ROM\product\app\PhotoTable
rd /s /q D:\ROM\product\app\talkback
rd /s /q D:\ROM\product\data-app
rd /s /q D:\ROM\product\overlay\AccentColorBlack
rd /s /q D:\ROM\product\overlay\AccentColorCinnamon
rd /s /q D:\ROM\product\overlay\AccentColorGreen
rd /s /q D:\ROM\product\overlay\AccentColorOcean
rd /s /q D:\ROM\product\overlay\AccentColorOrchid
rd /s /q D:\ROM\product\overlay\AccentColorPurple
rd /s /q D:\ROM\product\overlay\AccentColorSpace
rd /s /q D:\ROM\system\system\app\AiAsstVision
rd /s /q D:\ROM\system\system\app\BasicDreams
rd /s /q D:\ROM\system\system\app\BookmarkProvider
rd /s /q D:\ROM\system\system\app\CarrierDefaultApp
rd /s /q D:\ROM\system\system\app\CatchLog
rd /s /q D:\ROM\system\system\app\Cit
rd /s /q D:\ROM\system\system\app\com.miui.qr
rd /s /q D:\ROM\system\system\app\HybridAccessory
rd /s /q D:\ROM\system\system\app\HybridPlatform
rd /s /q D:\ROM\system\system\app\Joyose
rd /s /q D:\ROM\system\system\app\KeyChain
rd /s /q D:\ROM\system\system\app\KSICibaEngine
rd /s /q D:\ROM\system\system\app\LiveWallpapersPicker
rd /s /q D:\ROM\system\system\app\mab
rd /s /q D:\ROM\system\system\app\mi_connect_service
rd /s /q D:\ROM\system\system\app\MiConnectService171
rd /s /q D:\ROM\system\system\app\MiLinkService2
rd /s /q D:\ROM\system\system\app\MiLink
rd /s /q D:\ROM\system\system\app\MiPlayClient
rd /s /q D:\ROM\system\system\app\MiSound
rd /s /q D:\ROM\system\system\app\MiuiAccessibility
rd /s /q D:\ROM\system\system\app\MIUIAccessibility
rd /s /q D:\ROM\system\system\app\MiuiBugReport
rd /s /q D:\ROM\system\system\app\MiuiFrequentPhrase
rd /s /q D:\ROM\system\system\app\MiuiPrintSpoolerBeta
rd /s /q D:\ROM\system\system\app\MiuiVpnSdkManager
rd /s /q D:\ROM\system\system\app\MIUIVpnSdkManager
rd /s /q D:\ROM\system\system\app\ModemTestBox
rd /s /q D:\ROM\system\system\app\MSA
rd /s /q D:\ROM\system\system\app\PaymentService
rd /s /q D:\ROM\system\system\app\Protips
rd /s /q D:\ROM\system\system\app\SecurityInputMethod
rd /s /q D:\ROM\system\system\app\MIUISecurityInputMethod
rd /s /q D:\ROM\system\system\app\Stk
rd /s /q D:\ROM\system\system\app\TouchAssistant
rd /s /q D:\ROM\system\system\app\MIUITouchAssistant
rd /s /q D:\ROM\system\system\app\Traceur
rd /s /q D:\ROM\system\system\priv-app\Traceur
rd /s /q D:\ROM\system\system\app\Updater
rd /s /q D:\ROM\system\system\app\UPTsmService
rd /s /q D:\ROM\system\system\app\VoiceAssist
rd /s /q D:\ROM\system\system\app\VoiceTrigger
rd /s /q D:\ROM\system\system\app\YouDaoEngine
rd /s /q D:\ROM\system\system\data-app
rd /s /q D:\ROM\system\system\priv-app\Browser
rd /s /q D:\ROM\system\system\priv-app\MiBrowser
rd /s /q D:\ROM\system\system\priv-app\BuiltInPrintService
rd /s /q D:\ROM\system\system\priv-app\CallLogBackup
rd /s /q D:\ROM\system\system\priv-app\CellBroadcastLegacyApp
rd /s /q D:\ROM\system\system\priv-app\ContentExtension
rd /s /q D:\ROM\system\system\priv-app\MIUIContentExtension
rd /s /q D:\ROM\system\system\priv-app\ManagedProvisioning
rd /s /q D:\ROM\system\system\priv-app\MiGameCenterSDKService
rd /s /q D:\ROM\system\system\priv-app\MiRcs
rd /s /q D:\ROM\system\system\priv-app\Mirror
rd /s /q D:\ROM\system\system\priv-app\MIUIMirror
rd /s /q D:\ROM\system\system\priv-app\MIUIMusic
rd /s /q D:\ROM\system\system\priv-app\MiService
rd /s /q D:\ROM\system\system\priv-app\MIService
rd /s /q D:\ROM\system\system\priv-app\MiShare
rd /s /q D:\ROM\system\system\priv-app\MIShare
rd /s /q D:\ROM\system\system\priv-app\MIUIVipService
rd /s /q D:\ROM\system\system\priv-app\MusicFX
rd /s /q D:\ROM\system\system\priv-app\PersonalAssistant
rd /s /q D:\ROM\system\system\priv-app\MIUIPersonalAssistant
rd /s /q D:\ROM\system\system\priv-app\QuickSearchBox
rd /s /q D:\ROM\system\system\priv-app\MIUIQuickSearchBox
rd /s /q D:\ROM\system\system\priv-app\Tag
rd /s /q D:\ROM\system\system\priv-app\TagGoogle
rd /s /q D:\ROM\system\system\priv-app\UserDictionaryProvider
rd /s /q D:\ROM\system_ext\app\FM
rd /s /q D:\ROM\system_ext\app\xdivert
rd /s /q D:\ROM\product\app\xdivert
rd /s /q D:\ROM\system_ext\priv-app\EmergencyInfo
rd /s /q D:\ROM\vendor\app\Joyose
rd /s /q D:\ROM\vendor\data-app
del /f /s /q D:\ROM\system\system\bin\logd
del /f /s /q D:\ROM\system\system\etc\init\logd.rc
del /f /s /q D:\ROM\system\system\bin\mdnsd
del /f /s /q D:\ROM\system\system\etc\init\mdnsd.rc
del /f /s /q D:\ROM\vendor\bin\tcpdump
del /f /s /q D:\ROM\system\system\bin\tcpdump
del /f /s /q D:\ROM\vendor\bin\cnss_diag
echo Clean up done!
echo. & pause
```
#### 其他优化
- 修改**AnalyticsCore**，禁止MIUI自动分析
- 修改**build.prop**，添加以下字段，修复**Mipush**
```c
ro.miui.cust_variant=cn
ro.miui.region=CN
```
#### 根据个人习惯自定义系统
- 修改**charging.ogg**、**disconnect.ogg**、**LowBattery.ogg**、**bootaudio.mp3**去除多余提示音
- 添加主题文件**com.android.systemui**去除状态栏**HD**图标，添加**com.miui.home**修改桌面布局
- 修改图标文件**icons**
- K30系列可通过修改**DevicesOverlay.apk**将药丸挖孔改为猪鼻孔
