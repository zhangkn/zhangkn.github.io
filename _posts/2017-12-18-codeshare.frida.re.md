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


### 参考资料

- [frida-ios-dump-master](https://github.com/AloneMonkey/frida-ios-dump) 就是在[dump-ios](https://codeshare.frida.re/@lichao890427/dump-ios/)基础之上进行改造的
<br>


