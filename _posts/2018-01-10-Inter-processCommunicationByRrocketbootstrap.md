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

# 获取直接修改对应的配置信息
iPhone:~ root# ps -e |grep yalu*
 1174 ??         0:00.80 /var/containers/Bundle/Application/B831448D-BCD0-4F29-BDA6-9FC03903D30C/yalu102.app/yalu102

#plutil plist 内容
iPhone:/var/containers/Bundle/Application/B831448D-BCD0-4F29-BDA6-9FC03903D30C/yalu102.app root# plutil dropbear.plist
{
    KeepAlive = 1;
    Label = ShaiHulud;
    Program = "/usr/local/bin/dropbear";
    ProgramArguments =     (
        "/usr/local/bin/dropbear",
        "-F",
        "-R",
        "-p",
        22
    );
    RunAtLoad = 1;
}
# dropbear 参数
 iPhone:/var/containers/Bundle/Application/B831448D-BCD0-4F29-BDA6-9FC03903D30C/yalu102.app root# ps -e |grep dropbear
  228 ??         0:00.05 /usr/local/bin/dropbear -F -R -p 22
```

>* scp   的使用-- 利用wget 安装scp
```
#wget + 空格 + 要下载文件的url路径
# cydia里面安装wget
#  安装scp,默认安装在当前目录
wget mila432.com/scp
ldid -S scp
# chmod +x scp
chmod 777 scp
mv scp /usr/bin/scp
```


>* [deviceconsole](https://github.com/rpetrich/deviceconsole)  查看log

```
devzkndeMacBook-Pro:deviceconsole-master devzkn$ /Users/devzkn/Library/Developer/Xcode/DerivedData/deviceconsole-gfjsthvevyxiqyahugfpmzjlseye/Build/Products/Debug/deviceconsole --help
Usage: /Users/devzkn/Library/Developer/Xcode/DerivedData/deviceconsole-gfjsthvevyxiqyahugfpmzjlseye/Build/Products/Debug/deviceconsole [options]
Options:
 -d			Include connect/disconnect messages in standard out
 -u <udid>		Show only logs from a specific device
 -p <process name>	Show only logs from a specific process

Control-C to disconnect
Mail bug reports and suggestions to <ryan.petrich@medialets.com>
```

>* [deviceconsole 的改进版](https://github.com/zhangkn/deviceconsole)
```
devzkndeMacBook-Pro:deviceconsole-master devzkn$ knlog -h
knlog: invalid option -- h
Usage: knlog [options]
Options:
-i | --case-insensitive     Make filters case-insensitive
-f | --filter <string>      Filter include by single word occurrences (case-sensitive)
-x | --exclude <string>     Filter exclude by single word occurrences (case-sensitive)
-p | --process <string>     Filter by process name (case-sensitive)
-u | --udid <udid>          Show only logs from a specific device
-s | --simulator <version>  Show logs from iOS Simulator
     --debug                Include connect/disconnect messages in standard out
     --use-separators       Skip a line between each line
     --force-color          Force colored text
     --message-only          Display only level and message
Control-C to disconnect
devzkndeMacBook-Pro:aso.git devzkn$ knlog -i -f AppStore
Jan 10 18:57:45 iPhone appstored(libsystem_network.dylib)[587] <Info>: nw_endpoint_resolver_start_next_child [6 su.itunes.apple.com:443 in_progress resolver (satisfied)] starting child endpoint 123.53.139.209:443
Jan 10 18:57:45 iPhone appstored(libsystem_network.dylib)[587] <Info>: nw_endpoint_resolver_start_next_child [6 su.itunes.apple.com:443 in_progress resolver (satisfied)] starting next child endpoint in 250ms
Jan 10 18:57:45 iPhone appstored(libsystem_network.dylib)[587] <Info>: nw_endpoint_handler_start [6.2 123.53.139.209:443 initial path (null)]
Jan 10 18:57:45 iPhone appstored(libsystem_network.dylib)[587] <Info>: nw_connection_endpoint_report [6.2 123.53.139.209:443 initial path (null)] reported event path:start
Jan 10 18:57:45 iPhone appstored(libsystem_network.dylib)[587] <Info>: nw_connection_endpoint_report [6.2 123.53.139.209:443 waiting path (satisfied)] reported event path:satisfied
```

>*ldid 
```
devzkndeMacBook-Pro:bin devzkn$ which ldid
/opt/iOSOpenDev/bin/ldid
```

### 参考
- [iOS10 经过yalu越狱后无法ssh登录和无法scp拷贝问题](http://blog.csdn.net/dianshanglian/article/details/62422627)
- [http://pangu.io/ ios 9+](http://pangu.io/)
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
- [An iOS system log tailer that doesn't suck. Improved](https://github.com/MegaCookie/deviceconsole)
- [How to SSH via WiFi/USB on Yalu Jailbreak iOS 10](https://yalujailbreak.net/ssh-ios-10-tutorial/)
- [在iOS 7中获取UDID的3种可能方法](http://bbs.iosre.com/t/ios-7-udid-3/45)
- [CFInternal.h](https://opensource.apple.com/source/CF/CF-744.18/CFInternal.h)
```
#define CONST_STRING_DECL(S, V) const CFStringRef S = (const CFStringRef)__builtin___CFStringMakeConstantString(V);
```
