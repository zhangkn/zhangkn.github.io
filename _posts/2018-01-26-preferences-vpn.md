---
layout: post
title: preferences-vpn
date: 2018-01-26
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

### 正文



###  参考
- [逆向Preferences中关于VPN部分的问题](http://bbs.iosre.com/t/preferences-vpn/5000)

```
1、-- Preferences打开VPN连接的实现方式：
 在Preferences中通过调用VPNPreferences.bundle里面的VPNBundleController的setVPNActive:forSpecifier:方法实现的;
2、-- tweak 实现打开已配置过的VPN功能：
 并且可以用Cycript注入Preferences，生成一个VPNBundleController对象直接调用_setVPNActive:打开已配置过的VPN 
3、---VPNBundleController实例的创建：
 通过VPNBundleController的initWithParentListController:方法可以拿到一个VPNBundleController实例去调用_setVPNActive:方法。
4、创建要在Preferences, 切换可以在SB：
 缺乏头文件引入，可以使用  VPNBundleController = [[objc_getClass("VPNBundleController") alloc] initWithParentListController:nil]; 实现
5、--SBVPNConnectionChangedNotification 监听"VPNConnectionStatusChanged"消息
```

- [/preferences-vpn](http://bbs.iosre.com/t/preferences-vpn/5000/3)
- [SetVpn.m](https://github.com/I-SpongeBob/iosre/blob/1b4d46d9523fd9562e46d97653f6f622b39afb9f/iosre/VPN/setvpna/setvpntest/SetVpn.m)
- [VPNPreferences-Structs.h](https://github.com/CPDigitalDarkroom/iOS9-SpringBoard-Headers/blob/a11be523d5644a178614585ff57f9638300c2cc0/System/Library/PreferenceBundles/VPNPreferences.bundle/VPNPreferences-Structs.h)
- [iOS-10.1.1-Headers](https://github.com/zhangkn/iOS-10.1.1-Headers)
- [ManualVPN](https://github.com/Naville/ManualVPN/blob/8570c5fefae1fdbe9f6ef0c485bd69dee0124b36/Tweak.xm)
- [smyvpn](https://github.com/yangzhenglun/hookPro/blob/4eba639a395935dde190419459e54f011fa00e42/smyvpn/Tweak.xm)
- [https://github.com/yangzhenglun/hookPro/blob/4eba639a395935dde190419459e54f011fa00e42/smyvpn/Tweak.xm](http://bbs.iosre.com/t/app-deb/4869)