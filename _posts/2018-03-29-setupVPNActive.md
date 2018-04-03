---
layout: post
title: setupVPNActive
date: 2018-03-29
tag: iOSre
site: https://zhangkn.github.io
---

### 前言


我们常常需要知道本地VPN或者其他类型的VPN连接状态，通过是监听对应的通知进行处理

### 正文


>* SBVPNConnectionChangedNotification

```
如果是VPN服务器超时、断网、鉴权失败或者自定义VPN（对接系统的VPN接口）等情况，  通常是不会触发这个SBVPNConnectionChangedNotification。

<!-- 因此需要监听其他进程的动态---CFUserNotificationCreate -->

因为连接失败系统总要和用户交互，这个就想起了hook CFUserNotification，因为Function CFUserNotificationCreate的功能是：

Creates a CFUserNotification object and displays its notification dialog on screen.

```

>* [CFUserNotificationCreate](https://developer.apple.com/documentation/corefoundation/1534528-cfusernotificationcreate?language=objc)

```
Creates a CFUserNotification object and displays its notification dialog on screen.

<!-- vpn 相关的进程： -->
<!-- 大部分的情况是处理 nesessionmanager、sb  就足够了-->
<string>nesessionmanager</string>
<string>racoon</string>
<string>configd</string>
<string>mDNSResponder</string>
<string>networkd</string>
<string>pppd</string>
```


>* [/System/Library/PreferenceBundles](https://www.theiphonewiki.com/wiki//System/Library/PreferenceBundles)

```

        NSBundle* references = [NSBundle bundleWithPath:@"/System/Library/PreferenceBundles/VPNPreferences.bundle"];

        https://github.com/ichitaso/iOS-iphoneheaders/tree/master/iOS7.1.1/System/Library/PreferenceBundles





```

### StoreServices.framework

>* SSClientAccountStore.h

```
https://github.com/ichitaso/iOS-iphoneheaders/blob/be1e4073114a6572b34b171245a62186a732e561/iOS7.0.3/System/Library/PrivateFrameworks/StoreServices.framework/SSClientAccountStore.h

SSAccountStore


```


### SpringBoard

>* SBApplicationPlaceholderController

```
iOS-iphoneheaders/iOS8.3/System/Library/CoreServices/SpringBoard.app/SBApplicationPlaceholderController.h

iOS-iphoneheaders/iOS8.3/System/Library/CoreServices/SpringBoard.app/SBApplicationLibraryObserver.h

```

### see also

- [AFlexLoader](https://github.com/zhangkn/KNAFlexLoader)


```
/Users/devzkn/code/other/AFlexLoader-master/deploy 

<!-- Depends: mobilesubstrate, preferenceloader (>= 2.2), applist (>= 1.5.11)
 -->

preferenceloader、applist

```

- [bypass_passcode_and_half_slide_unlock](https://github.com/guoc/protecti/blob/f12a0c6509ccb93d25508a1d6378d929a1334453/hooks/bypass_passcode_and_half_slide_unlock.xm)


```
void handleSystemPasscodeChange(CFNotificationCenterRef center,void *observer,CFStringRef name,const void *object,CFDictionaryRef userInfo) {

    global_slfe = nil;

    global_OnceUnlockSuccessfully = NO;

}
```

- [Localizable.strings](https://github.com/TxusBlack/cyand00r-8/blob/4d839d32638c46ac95ad25cda4c9db0d796dc7ed/uncompilied/Library/Application%20Support/CCSettings/Localized.bundle/zh-Hans.lproj/Localizable.strings)

```

"AUTO_LOCK_NEVER"="自动锁定: 永不";
"AUTO_LOCK_MIN"="自动锁定: %d分钟";
"AUTO_LOCK_MINS"="自动锁定: %d分钟";
```