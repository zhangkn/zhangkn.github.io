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


### see also