---
layout: post
title: AutoUnlock
date: 2018-04-17
tag: iOSre
site: https://zhangkn.github.io
---

### 前言


### 正文

### 自动解锁

>* unlockUIFromSource

```
//最好使用activate 开头的方法
%hook SBLockScreenViewController
-(void)activate{

   %orig;

   [[%c(SBLockScreenManager) sharedInstance] unlockUIFromSource:0 withOptions:nil];


}
%end
```

### 分析
>* unlockUIFromSource: withOptions:

```
A01-06:~ root# cycript -p SpringBoard

cy# var lockScreenManager = [SBLockScreenManager sharedInstance]
#"<SBLockScreenManager: 0x1700f400>"
cy#  [lockScreenManager unlockUIFromSource: 0 withOptions: nil]

```
>* SBLockScreenManager

```
- (void)unlockUIFromSource:(int)source withOptions:(id)options;
- (void)lockUIFromSource:(int)source withOptions:(id)options;
- (_Bool)attemptUnlockWithPasscode:(id)arg1; // 密码解锁

cy# [lockScreenManager  attemptUnlockWithPasscode: @"8888"]

```

### cy

>* SBLockScreenNotificationListController 

```

cy# [[[UIWindow keyWindow] rootViewController] _printHierarchy].toString()
`<SBMainScreenAlertWindowViewController 0x1bbb5c30>, state: appeared, view: <UIView 0x1ba6c3f0>
   | <SBLockScreenViewController 0x183e5000>, state: appeared, view: <SBLockScreenView 0x1ba3f5e0>
   |    | <SBLockScreenDateViewController 0x1ba32240>, state: appeared, view: <SBFLockScreenDateView 0x1ba319a0>
   |    | <SBLockScreenStatusTextViewController 0x1bbf3360>, state: disappeared, view: <_UILegibilityLabel 0x1bbf3710>
   |    | <SBLockScreenNotificationListController 0x1ba227e0>, state: appeared, view: <SBLockScreenNotificationListView 0x1ba1f700>
   |    | <MPUSystemMediaControlsViewController 0x1ba1e610>, state: disappearing, view: <UIView 0x1ba31de0> not in the window`



cy# [[[UIWindow keyWindow] rootViewController] _printHierarchy].toString()
`<SBMainScreenAlertWindowViewController 0x1bbb5c30>, state: appeared, view: <UIView 0x1ba6c3f0>
   | <SBLockScreenViewController 0x183e5000>, state: appeared, view: <SBLockScreenView 0x1ba3f5e0>
   |    | <SBLockScreenDateViewController 0x1ba32240>, state: appeared, view: <SBFLockScreenDateView 0x1ba319a0>
   |    | <SBLockScreenStatusTextViewController 0x1bbf3360>, state: disappeared, view: <_UILegibilityLabel 0x1bbf3710>
   |    | <SBLockScreenNotificationListController 0x1ba227e0>, state: appeared, view: <SBLockScreenNotificationListView 0x1ba1f700>
   |    | <MPUSystemMediaControlsViewController 0x1ba1e610>, state: appearing, view: <UIView 0x1ba31de0> not in the window
   |    | <SBLockScreenBatteryChargingViewController 0x17f574a0>, state: appeared, view: <SBLockScreenBatteryChargingView 0x17f57720>`


```



### see also
