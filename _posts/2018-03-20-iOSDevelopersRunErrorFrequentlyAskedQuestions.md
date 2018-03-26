---
layout: post
title: iOSDevelopersRunErrorFrequentlyAskedQuestions
date: 2018-03-20
tag: iOSre
site: https://zhangkn.github.io
---


### 前言


### 正文


>* framework not found IOKit

```
#import <IOKit/IOKitLib.h>

意味着/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/Frameworks/IOKit.framework/Headers 目录下找不到IOKitLib.h
```

>* Preparing to run Xcode Build Phase...
Signing /Users/devzkn/Library/Developer/Xcode/ with codesign... iPhone Developer: ambiguous (matches "iPhone Developer:  and "iPhone Developer:  ()" in /Users/devzkn/Library/Keychains/login.keychain-db)
Failed.

```
searching for "iPhone" in Keychain and removing all expired certificates,只留下一个有效的即可
```


###  dpkg

>* dpkg-deb: error: control directory has bad permissions 700 (must be >=0755 and <=0775)

```
devzkndeMacBook-Pro:DEBIAN devzkn$ ls -lrt
total 40
-rwxr-xr-x  1 devzkn  staff   153 Mar 22 14:03 prerm
-rwxr-xr-x  1 devzkn  staff  5480 Mar 22 16:17 postinst
-rwxr-xr-x  1 devzkn  staff  3026 Mar 22 16:17 preinst
-rw-r--r--  1 devzkn  staff   287 Mar 22 16:17 control

<!-- 需要修改为 -->
 -rwxr-xr-x  1 devzkn  staff   302 Mar 19 10:26 control

<!-- 解决方法 -->
 devzkndeMacBook-Pro:DEBIAN devzkn$ chmod +x control

 # devzkndeMacBook-Pro: devzkn$ chmod -R 0755 Package

```




### see also 

- [IOS高级开发～开机启动&无限后台运行&监听进程](https://blog.csdn.net/bao990423420/article/details/14123445)

- [逆向分析网络协议 iOS 篇](https://mp.weixin.qq.com/s?__biz=MzIwMTYzMzcwOQ==&mid=403204191&idx=1&sn=514664cc05597f8b76730cbf9f3a57f5&scene=23&srcid=0309yRmQiN6spspIEAk8t6kO#rd)

```
<!--1、 Class-dump   -A             show implementation addresses -->

devzkndeMacBook-Pro:KNMNV5.4.0 devzkn$  /Users/devzkn/Downloads/class-dump --arch armv7 KNMN.decrypted -H  -o ./header

Class-Dump XApp -H -A -S -o headers/

<!-- devzkndeMacBook-Pro:~ devzkn$ class-dump --arch armv7 tmp -H -A -S -o ./header -->


+ (id)ret32bitString;	// IMP=0x00013887
+ (id)setupHttpSign;	// IMP=0x0001395f

地址空间布局随机化 (ASLR) 可防止对内存损坏缺陷的利用。
<!-- image list -o -f -->

<!--2、 br s -a "0x00000000000f8000+0x000000010024b99c" -->

执行 c 或 continue 让 app 跑起来，触发断点的地址，就会发现断点停下来的。

<!-- po $arg1 -->

 OC 方法 $arg1 就是 self

<!-- 3、断点变 Log -->
breakpoint command add 1

> po $arg1
> c
> DONE

```

- [iOS 10-10.1.1重新越獄與誤點清除所有內容和設定解決方法](https://mrmad.com.tw/ios10-1-1-re-jailbreak-device-after-erase-all-content-and-settings)

```
<!-- 「設定」>「一般」>「重置」>「清除所有內容和設定」所引起 -->
造成Cydia的資料夾/var/lib 和 /var/log/apt整個被刪除   确实没有了

cp -R /var/mobile/Media/Books/lib /var

mkdir /var/log/apt

<!-- 没有cydia app -->
devzkndeMacBook-Pro:var devzkn$ scp -r /Users/devzkn/code/re/yalu102/yalu102/bootstrap/Applications/Cydia.app usb2222:/Applications/

<!-- 缺少cydia 目录 -->
devzkndeMacBook-Pro:~ devzkn$ scp -r  usb2222:/usr/libexec/cydia ~/code/re
devzkndeMacBook-Pro:~ devzkn$ scp -r  ~/code/re/cydia usb2222:/usr/libexec

<!-- /usr/libexec/cydo 修改为正确权限 -->
-rwsr-sr-x 1 root wheel  52416 Mar  9  2016 cydo

06555

-r-sr-sr-x 1 root wheel 133808 Mar 21 14:17 cydo

<!-- uikit tool 中包含uicahe 命令 进行icon的刷新 -->
```

