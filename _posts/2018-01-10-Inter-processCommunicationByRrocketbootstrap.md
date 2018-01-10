---
layout: post
title: Inter-processCommunicationByRrocketbootstrap
date: 2018-01-10
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

### Install this from Cydia
直接搜索librocketbootstrap安装即可
>* 获取librocketbootstrap 
```
iPhone:/var/log root# ls -l /usr/lib/librocketbootstrap.dylib
-rwxr-xr-x 1 root wheel 221776 Feb  6  2017 /usr/lib/librocketbootstrap.dylib*
```



### Q&A 

>* 使用yalu102 激活了之后，无法连接ssh
```
#/usr/local/bin/dropbear 此次越狱工具默认安装了 SSH。采用了 Dropbear 取代 Openssh。
因此在部署安装使用yalu102的时候记得修改dropbear.plist的信息：ProgramArguments的127.0.0.1:22 直接改为22。
如果部署完成，直接修改沙盒的信息的话，记得重启设备,且必须卸载Openssh。
```

### 参考
- [iOS Tweak 进程间通讯](https://www.jianshu.com/p/8a3c492cb5fb)
- [RocketBootstrap](http://iphonedevwiki.net/index.php/RocketBootstrap#How_to_use_this_library)
- [Updating_extensions_for_iOS_7](http://iphonedevwiki.net/index.php/Updating_extensions_for_iOS_7)
- [yalu102 使用Xcode8](https://github.com/kpwn/yalu102)
- [iOS10.2 SSH连接越狱设备](https://bingozb.github.io/21.html)
- [http://free.zhzhao.com/ 参考他人的代码](http://free.zhzhao.com/)
- [iOS10下打印NSLog syslog信息](https://www.jianshu.com/p/9120e46f98b1)
- [ios10-2-soca-nslog](http://iosre.com/t/ios10-2-soca-nslog/6955/15)
- [syslogflipswitch](http://cydia.saurik.com/package/com.ichitaso.syslogflipswitch/)
- [https://ichitaso.com/](https://ichitaso.com/)
- [Does syslogd work on 9.3.3 ](https://www.reddit.com/r/jailbreak/comments/50niif/question_does_syslogd_work_on_933/)
- [cydia 搜索安装com.ichitaso.syslogflipswitch](http://cydia.saurik.com/package/com.ichitaso.syslogflipswitch/)
