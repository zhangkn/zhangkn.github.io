---
layout: post
title: codeshare.frida.re
date: 2017-12-18
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

最近觉得Frida 很有潜力，就发现了[codeshare.frida.re](https://codeshare.frida.re/)
本文演示下如何使用codeshare。

### ios-app-info

>* 1、使用frida-ps 查看app 信息
```
devzkndeMacBook-Pro:zhangkn.github.io devzkn$ frida-ps -Uai
 PID  Name                Identifier                                     
----  ------------------  -----------------------------------------------
4929  Safari              com.apple.mobilesafari                         
4917  微信                  com.tencent.xin                                
4906  邮件                  com.apple.mobilemail                           
```
>* 2、使用-U -p 参数 查看app的信息

```
devzkndeMacBook-Pro:zhangkn.github.io devzkn$ frida --codeshare dki/ios-app-info -U -p 4929
     ____
    / _  |   Frida 10.6.27 - A world-class dynamic instrumentation framework
   | (_| |
    > _  |   Commands:
   /_/ |_|       help      -> Displays the help system
   . . . .       object?   -> Display information about 'object'
   . . . .       exit/quit -> Exit
   . . . .
   . . . .   More info at http://www.frida.re/docs/home/
Attaching...                                                            
Hello! This is the first time you're running this particular snippet, or the snippet's source code has changed.

Project Name: ios-app-info
Author: @dki
Slug: dki/ios-app-info
Fingerprint: 816d326d8a62781447b4fe4570f2b103ed9a1bf27fb43c24895c7295fa6f609a
URL: https://codeshare.frida.re/@dki/ios-app-info
            
Are you sure you'd like to trust this project? [y/N] y
Adding fingerprint 816d326d8a62781447b4fe4570f2b103ed9a1bf27fb43c24895c7295fa6f609a to the trust store! You won't be prompted again unless the code changes.
[iPhone::PID::4929]->  infoLookup("NSAppTransportSecurity")
null
[iPhone::PID::4929]->  infoDictionary()
{
    "BuildMachineOSBuild": "13A603",
    "CFBundleDevelopmentRegion": "English",
    "CFBundleDisplayName": "Safari",
    "CFBundleExecutable": "MobileSafari",
```
### -[objc-method-observer](https://codeshare.frida.re/@mrmacete/objc-method-observer/)

使用这个的好处的，不用频繁的修改代码
>*  使用示例

```
devzkndeMacBook-Pro:redPackageRebort devzkn$ frida-ps -Uai
devzkndeMacBook-Pro:redPackageRebort devzkn$ frida --codeshare mrmacete/objc-method-observer -U -p 10490
/*
 * To observe a single class by name:
 *     observeClass('NSString');
 *
 * To dynamically resolve methods to observe (see ApiResolver):
 *     observeSomething('*[* *Password:*]');
 */

```


### 分析案例


>* devzkndeMacBook-Pro:bin devzkn$ frida-ps -Ua

```
682  knip       com.Benqumark.knip

```

>* devzkndeMacBook-Pro:bin devzkn$ kndump knip

```

exit 0devzkndeMacBook-Pro:bin devzkn$ cat kndump
#!/bin/sh
# iphone 的配置Start Cydia and add Frida’s repository by navigating to Manage -> Sources -> Edit -> Add and entering https://build.frida.re
# frida-ps -Uai 查看，来获取参数
# devzkndeMBP:bin devzkn$ frida-ps -Ua
  # PID  Name       Identifier        
# -----  ---------  ------------------
# 14790  App Store  com.apple.AppStore
# usage: devzkndeMacBook-Pro:~ devzkn$ kndump 邮件
# ./dump.py 'App Store'
# dump app   
echo "" > ~/.ssh/known_hosts
cd ~/decrypted/frida-ios-dump-master 
rm -rf ./Payload
./dump.py $1
open .

-rw-r--r--  1 devzkn  staff  52334498 Apr  1 12:24 knip.ipa

```



>* [可以dump混编的](https://github.com/zhangkn/KNBin/blob/master/swiftOCclass-dump)

```
devzkndeMBP:bin devzkn$ swiftOCclass-dump  --arch arm64 /Users/devzkn/decrypted/AppStoreV10.2/Payload/AppStore.app/AppStore -H -o  /Users/devzkn/decrypted/AppStoreV10.2/head

swiftOCclass-dump knip  -H -o  /Users/devzkn/decrypted/knip/head



devzkndeMacBook-Pro:knip.app devzkn$ otool -hv knip
Mach header
      magic cputype cpusubtype  caps    filetype ncmds sizeofcmds      flags
   MH_MAGIC     ARM         V7  0x00     EXECUTE    61       6300   NOUNDEFS DYLDLINK TWOLEVEL WEAK_DEFINES BINDS_TO_WEAK PIE


devzkndeMacBook-Pro:knip.app devzkn$ otool -l knip

```

>* otool -l

```
name /System/Library/Frameworks/AVFoundation.framework/AVFoundation (offset 24)

name /System/Library/Frameworks/Accounts.framework/Accounts (offset 24)
name /System/Library/Frameworks/AudioToolbox.framework/AudioToolbox (offset 24)
name /System/Library/Frameworks/CFNetwork.framework/CFNetwork (offset 24)

 name /System/Library/Frameworks/CoreData.framework/CoreData (offset 24)
 name /System/Library/Frameworks/CoreGraphics.framework/CoreGraphics (offset 24)

 name /System/Library/Frameworks/CoreMedia.framework/CoreMedia (offset 24)

 name /System/Library/Frameworks/CoreTelephony.framework/CoreTelephony (offset 24)

 name /System/Library/Frameworks/CoreText.framework/CoreText (offset 24)

 name /System/Library/Frameworks/ImageIO.framework/ImageIO (offset 24)

 name /System/Library/Frameworks/MobileCoreServices.framework/MobileCoreServices (offset 24)

 name /System/Library/Frameworks/QuartzCore.framework/QuartzCore (offset 24)

 name /System/Library/Frameworks/SafariServices.framework/SafariServices (offset 24)

 name /System/Library/Frameworks/Social.framework/Social (offset 24)

 name /System/Library/Frameworks/StoreKit.framework/StoreKit (offset 24)

 name /System/Library/Frameworks/UserNotifications.framework/UserNotifications (offset 24)
<!-- IAD是苹果推出的广告平台，它可以帮助开发者从应用程序中获取收入。 -->
 name /System/Library/Frameworks/iAd.framework/iAd (offset 24)

 name /System/Library/Frameworks/MediaPlayer.framework/MediaPlayer (offset 24)

 name /System/Library/Frameworks/JavaScriptCore.framework/JavaScriptCore (offset 24)

 name /System/Library/Frameworks/CoreMotion.framework/CoreMotion (offset 24)

 name /System/Library/Frameworks/Photos.framework/Photos (offset 24)
<!-- 在 iOS 8 出现之前，开发者只能使用 AssetsLibrary 框架来访问设备的照片库，而在 iOS8 出现之后，苹果提供了一个名为 PhotoKit 的框架 -->

 name /System/Library/Frameworks/AssetsLibrary.framework/AssetsLibrary (offset 24)

<!-- https://developer.apple.com/documentation/opengles -->
<!-- OpenGL ES provides a C-based interface for hardware-accelerated 2D and 3D graphics rendering -->
<!-- 在iOS平台上进行OpenGL ES 开发，OpenGLES.framework和QuartzCore.framework这两个库是必须的 -->

 name /System/Library/Frameworks/OpenGLES.framework/OpenGLES (offset 24)

 name /System/Library/Frameworks/CoreVideo.framework/CoreVideo (offset 24)

 name /System/Library/Frameworks/VideoToolbox.framework/VideoToolbox (offset 24)

```



### 参考资料

- [frida-ios-dump-master](https://github.com/AloneMonkey/frida-ios-dump) 就是在[dump-ios](https://codeshare.frida.re/@lichao890427/dump-ios/)基础之上进行改造的
<br>


