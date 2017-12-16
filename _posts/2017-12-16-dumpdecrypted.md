---
layout: post
title: dumpdecrypted
date: 2017-12-16
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

本文重点推荐使用[frida-ios-dump-master](https://github.com/zhangkn/frida-ios-dump)，而非dumpdecrypted.dylib。


### frida-ios-dump-master 

使用frida-ios-dump-master 只需先用frida-ps查看applications Name ，之后执行dump.py 即可在dump.py 目录下生成砸壳之后的ipa包。

Frida环境的搭建可以看下[这篇文章](https://zhangkn.github.io/2017/12/frida/#gsc.tab=0)

### dumpdecrypted.dylib 
dumpdecrypted的原理是让app预先加载一个解密的dumpdecrypted.dylib，然后在程序运行后，将代码动态解密，最后在内存中dump出来整个程序。

砸壳的步骤：

>*1、找到app二进制文件对应的目录； 
```
ps -e|grep /var/mobile/Container*
```
>*2、找到app document对应的目录； 
```
cycript -p 
```
```
cy# [[NSFileManager defaultManager] URLsForDirectory:NSDocumentDirectory inDomains:NSUserDomainMask][0]
```
>*3、将砸壳工具dumpdecrypt.dylib拷贝到ducument目录下； //目的是为了获取写的权限 
```
devzkndeMacBook-Pro:dumpdecrypted-master devzkn$ scp ./dumpdecrypted.dylib root@192.168.2.212://var/mobile/Containers/Data/Application/91E7D6CF-A3D3-435B-849D-31BB53ED185B/Documents
```

>*4、砸壳；利用环境变量 DYLD_INSERT_LIBRARY 来添加动态库dumpdecrypted.dylib
```
DYLD_INSERT_LIBRARIES=dumpdecrypted.dylib /var/mobile/Containers/Bundle/Application/01ECB9D1-858D-4BC6-90CE-922942460859/KNWeChat.app/KNWeChat
```
第一个path为dylib，目标path 为app二进制文件对应的目录













