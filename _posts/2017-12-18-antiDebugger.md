---
layout: post
title: antiDebugger
date: 2017-12-18
tag: anti
site: https://zhangkn.github.io
---
### 前言


软件的逆向工程指的是通过分析一个程序或系统的功能、结构或行为，将它的技术实现或设计细节推导出来的过程。
当我们因为工作需要，或是对一个软件的功能很感兴趣，却又拿不到它的源代码时，往往可以通过逆向工程的方式对它进行分析，探索它的实现原理。
一个生动的比喻：照着配方包饺子,是正向开发 吃着饺子推配方（API 调用顺序）,是逆向工程。




### iOS 逆向分析方法


从二进制文件入手，尝试从汇编代码中理清原始逻辑，并修改/绕过以达到目的。

>* 1、网络分析
```
抓包工具有 tcpdump, WireShark, Charles
```
>* 2、[静态分析](https://zhangkn.github.io/2017/01/iOS_Wifilist/#gsc.tab=0)
```
在app没运行的情况下，借助反汇编工具（hopper\otool等）查看代码内部结构、语法、过程、接口，以及二进制文件结构信息、class-dump目标对象的class 信息等分析过程。
关键的地方做到准确提取信息：包括网络协议、数据格式、代码逻辑、控制流、表达式、接口和数据流。
#“目标软件的二进制代码到汇编代码的翻译过程，我们称之为“反汇编”。”
```
>* 3、 动态分析(动态调试、代码跟踪)
```
“分析者可以在程序的执行过程中观察程序的执行流程和计算结果。”通常我通过借助lldb、Cycript、frida、AFLEXLoader、运行时常用的API等工具进行调试、分析代码，获取内存状态甚至修改内存，该过程包括反反调试、查看寄存器内容、跟踪函数执行结果等。
```

### 整体套路

 以下步骤没有绝对的顺序
 - 先dumpdecrypted（砸壳）、class-dump（导出OC头文件）寻找分析切入点；砸壳步骤可以使用[frida-ios-dump-master](https://zhangkn.github.io/2017/12/dumpdecrypted/#gsc.tab=0) 一次性完成。
 - 用cycript、Frida、AFLEXLoader、[hookClass](https://github.com/zhangkn/hookClass/tree/master/hookClass/KNHookClass) 定位目标视图，获取目标的VC、delegate
 - 使用IDA、hopper、lldb 配合使用分析代码调用逻辑
 - 编写tweak（借助theos、MonkeyDev进行编写）


 


### Mac 端的工具

>* Theos
Theos 是一个基于 Unix 平台(OS X，iOS…)和大多数的 Linux 平台下进行越狱开发的集成开发环境，由 Dustin Howett 大神开发并开源到 GitHub 上，它的特点是搭建简单、Logos语法简单、编译发布简单

当然我还挺喜欢用MonkeyDev（支持CocoaPods) 开发iPhone tool 、iPhone tweak。

>* [IDA](https://down.52pojie.cn/Tools/Disassemblers/)
恶意代码分析、漏洞研究、COTS验证、隐私保护;

>* otool 
Otool可以提取并显示目标文件的相关信息:包括头部，加载命令，各个段，共享库，动态库。

>* Charles、Wireshark

```
1、Wireshark可以详细的看到网络请求的三次握手，并且可支持spdy、tcp等等的网络协议抓包。

2、比较重要的是Cookie，网站为了辨别用户身份、进行 session 跟踪而储存在用户本地终端上的数据；
<!-- 3、同时Cookie在爬虫的时候也是一个必不可少的东西，那么如何自动化获取Cookie？ -->
a.清除当前浏览器保存的Cookie，并重启浏览器
b.重启之后我们访问www.csdxxxn.net , 比如 Cookie在Response的Headers->set-cookie
c.做登录操作并携带Cookie在请求Headers中，登录成功之后该Cookie就会生
<!-- 某些请求携带sign参数的,例如wx,有时候不是直接的破解而是函数的Hook -->
iOS是通过IDA找到sign函数,并使用cycript调用;-- 简单的举例
4、charlesV4.2 的辅助功能：调试我们的接口
Compose:可直接自定义对应的请求，并执行该请求。这个对我们抓包用处很大。我们可以从中得到该接口的必填参数、验证我们生成的Sign等等的。
Repeat：很简单就是执行一次重复请求操作
Repeat Advanved：重复请求的高级操作，可自定义重复的次数、每隔多少秒执行、启动请求的线程数。这个功能对于我们的接口的压测是很有用的。

```






### 安全

- 使用二进制协议传输数据
- 类名方法名混淆、越狱检测、核心代码使用swift、c\c++ 实现
- 反调试





### class-dump的原理

利用 Objective-C 语言的 runtime 特性，将存储在 Mach-O 文件中的 @interface 和 @protocol 信息提取出来，并生成相应的 .h 文件

>* [dump swift code embedded with ObjC code](https://github.com/zhangkn/MonkeyDev/blob/master/bin/class-dump)
```
devzkndeMacBook-Pro:KNMNV5.4.0 devzkn$  /Users/devzkn/Downloads/class-dump --arch armv7 KNMN.decrypted -H  -o ./header
```

### 总结

>*  思路
```
换个思路，直接访问手机的站点:手机做的站点其性能比较重要，往往做得比较简单，而且容易暴露一些API接口
```


### 把函数名隐藏在结构体里,以函数指针成员的形式存储
>* KNUtil.h

```
@interface KNUtil : NSObject
/**
 把函数名隐藏在结构体里，以函数指针成员的形式存储。
 编译后，只留了下地址，去掉了名字和参数表，提高了逆向成本和攻击门槛.
 */
typedef struct _util {
    void (*cign)(char *kns[],unsigned int kncount, const char *knkey, unsigned char *knput);
}CNtKNil_t ;
#define SharedUtilStruct ([KNUtil sharedUtil])//提供给外围的接口
+ (CNtKNil_t *)sharedUtil;
```
>* KNUtil.m
```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
//留给读者自己实践
```
>* 外围调用
```
SharedUtilStruct->cign(key ,count,knkey, knput);
```


### 参考资源
- [《iOS逆向工程-介绍》](https://blog.tylinux.com/2017/07/24/reverse-engineering-000/)
```
逆向工程则是从最后的二进制文件入手，通过反汇编、静态分析、动态调试等手段，达到了解程序执行逻辑，修改或者重新实现这一逻辑的目的。
软件开发商通常会对软件进行混淆、加密、加壳等操作，在真正分析软件的逻辑之前，通常需要先对应用进行脱壳、反混淆等操作。
```
- [ SOLVED : Classdump Error: Cannot find offset for address XXXXX in stringAtAddress:](http://iosre.com/t/solved-classdump-error-cannot-find-offset-for-address-xxxxx-in-stringataddress/10626)
- [ios-auditor](http://riusksk.me/2016/06/23/ios-auditor/)
- [restore-symbol](https://github.com/tobefuturer/restore-symbol)
- [正确获取struct结构体的内容](http://iosre.com/t/struct/9585/1)
- [theiphonewiki](https://www.theiphonewiki.com/)
- [Objective-C Runtime](https://developer.apple.com/documentation/objectivec/objective_c_runtime)
- [nshipster](http://nshipster.com/xctool/)
- [blackhat](https://www.blackhat.com/asia-18/briefings/schedule/index.html)
- [Swizzler2](https://github.com/vtky/Swizzler2)
- [hiding-content-git-escape-sequence-twistlock-labs-experiment](https://www.twistlock.com/2017/12/13/hiding-content-git-escape-sequence-twistlock-labs-experiment/)
- [BAD FOR ENTERPRISE ATTACKING BYOD ENTERPRISE MOBILE SECURITY SOLUTIONS](http://7xibfi.com1.z0.glb.clouddn.com/uploads/default/original/2X/2/2a09f6db6d0f0a0cbefdfddf545cbc3c0fdcce8e.pdf)
- [关于反调试&反反调试那些事](http://bbs.iosre.com/t/topic/8179)
- [tweak、 lldb python for anti anti debug](https://github.com/AloneMonkey/AntiAntiDebug) 对应的文章[antiantidebug](http://www.alonemonkey.com/2017/05/25/antiantidebug/)
- [越狱开发防护与破解](http://luoxianming.cn/2016/11/15/yueyutools3prevention/)
- [HookZz](https://github.com/jmpews/HookZz)
- [HookZzModules](https://github.com/jmpews/HookZzModules/tree/master/AntiDebugBypass)
- [反调试与绕过的奇淫技巧](http://iosre.com/t/topic/9351)
- [动手写一个简单的Mac内核反反调试扩展](http://www.alonemonkey.com/2017/11/20/get-start-antidebug-kext/)
- [AppleTrace dance with MonkeyDev](http://everettjf.com/2017/10/12/appletrace-dancewith-monkeydev/)
- [HookFramework 架构设计](http://iosre.com/t/hookframework/9350)
- [使用HookZz快速逆向(Hack objc_msgSend) 理清逻辑](http://iosre.com/t/hookzz-hack-objc-msgsend/9422)
- [反调试及绕过](http://jmpews.github.io/2017/08/09/darwin/%E5%8F%8D%E8%B0%83%E8%AF%95%E5%8F%8A%E7%BB%95%E8%BF%87/)
- [AppleTrace](https://gitter.im/appletrace/AppleTrace)
- [octrace](https://github.com/wadahana/octrace)
- [checker_dev_manual](https://clang-analyzer.llvm.org.cn/checker_dev_manual.html)
- [The LLVM Compiler Infrastructure](http://llvm.org/)
- [MakePackageInstall](https://github.com/zhangkn/Lai/blob/master/MakePackageInstall)   指定  debug=0
- [A theos example of running App as root](https://github.com/zhangkn/RootApp) setuid(0); 是重点
- [INJECTING MISSING METHODS AT RUNTIME](https://www.hopperapp.com/blog/?author=1)
- [setting-up-ios-hacking-env](https://togetherwehack.com/2017/09/19/setting-up-ios-hacking-env/)
- [unknowncheats](https://www.unknowncheats.me/forum/index.php)
- [http://rehints.com/](http://rehints.com/)
- [damnvulnerableiosapp](http://damnvulnerableiosapp.com/)
- [introducing-the-ios-reverse-engineering-toolkit](https://www.veracode.com/blog/2014/03/introducing-the-ios-reverse-engineering-toolkit)
- [how-to-reverse-engineer-os-x-and-ios-software](https://www.apriorit.com/dev-blog/363-how-to-reverse-engineer-os-x-and-ios-software) 
- [基于OLLVM定制的开源混淆工具Hikari](http://iosre.com/t/ollvm-hikari/10475)
- [在iOS 7中获取UDID的3种可能方法](http://bbs.iosre.com/t/ios-7-udid-3/45)
 


###  参考博客
- [LLDB Cheat Sheet](https://gist.github.com/ryanchang/a2f738f0c3cc6fbd71fa)
- [Patching for process attaching](http://iphonedevwiki.net/index.php/Debugserver#Patching_for_process_attaching)
- [jmpews](http://jmpews.github.io/2017/08/09/darwin/%E5%8F%8D%E8%B0%83%E8%AF%95%E5%8F%8A%E7%BB%95%E8%BF%87/)
- [/iOS-Reverse-Engineering-presentation](https://aozhimin.github.io/iOS-Reverse-Engineering-presentation/#/title) aozhimin
- [Pegasus间谍套件内部原理及流程剖析](https://bbs.pediy.com/thread-212483.htm)
- [IOSurfaceRootUserClient Port UAF](http://blog.pangu.io/author/windknown/)
- [v0rtex](https://siguza.github.io/v0rtex/)
- [Through the mach portal](https://bugs.chromium.org/p/project-zero/issues/attachment?aid=280146)
- [ios-resources](https://github.com/Siguza/ios-resources)
- [给 frida 做了个图形界面，动态分析 iOS 应用](http://iosre.com/t/frida-ios/9815)
- [idb](https://github.com/dmayer/idb)
- [iSECPartners](https://github.com/iSECPartners)
- [黑盒iOS安全分析](https://github.com/iSECPartners/Introspy-iOS)
- [ntrospy-Analyzer](https://github.com/iSECPartners/Introspy-Analyzer)
- [82flex](https://82flex.com/)
- [objective-c-internals](http://algorithm.com.au/downloads/talks/objective-c-internals/objective-c-internals.pdf)
- [algorithm](http://algorithm.com.au/blog/)
- [FRIEND](https://github.com/alexhude/FRIEND)
- [Hopper Disassembler批量导出反编译的伪代码](http://www.poboke.com/study/bulk-export-pseudo-code-in-hopper-disassembler.html)
- [LLDB Python Reference](http://lldb.llvm.org/python-reference.html)
- [http://www.tkkk.fun/](http://www.tkkk.fun/)
- [iOS非主流逆向初探](http://whenbar.com/2017/04/01/iOS%E9%9D%9E%E4%B8%BB%E6%B5%81%E9%80%86%E5%90%91%E5%88%9D%E6%8E%A2/)
- [iOS程序破解——ARM汇编基础](http://www.cnblogs.com/mddblog/p/4951650.html)
- [iOS APP的加固保护原理](https://mp.weixin.qq.com/s/gthDSLw45GW3oVlsAOm-dQ)
- [swiftyper](http://www.swiftyper.com/tags/%E9%80%86%E5%90%91/)
- [使用 Xcode 调试第三方应用](http://www.swiftyper.com/2017/07/02/attach-third-app-using-xcode/)
- [AntiClassDumpImplementationNotes](https://hikariproject.github.io/2017/12/21/AntiClassDumpImplementationNotes/)
- [专访沙梓社：做个“Think Different”的技术牛人](http://www.csdn.net/article/2015-05-18/2824691-developer)
```
根据图纸制作实物的这个过程是正向工程，而根据实物倒推图纸的这个过程则是逆向工程
```


- [TiGa-vid1](http://www.woodmann.com/TiGa/videos/TiGa-vid1.htm)
- [TiGa的IDA系列教程](http://www.woodmann.com/TiGa/idaseries.html)
- [Lena151的逆向工程教程](https://tuts4you.com/e107_plugins/download/download.php?list.17)

- [iOS app开发安全攻略](https://www.jianshu.com/p/2aef92f6849b)
- [gf&zjの盗梦空间'links](https://www.gfzj.us/links/)
- [wxapkg解包](http://lrdcq.com/me/read.php/66.htm)
### github

- [snakeninny](https://github.com/snakeninny)

- [gitbook](https://www.gitbook.com/@tomatobin)
- [osx & ios re 101](https://github.com/michalmalik/osx-re-101)
- [charles-hacking](https://github.com/zhangkn/charles-hacking)
- [restore-symbol](https://github.com/tobefuturer/restore-symbol)
### gist.github
- [t1t_hack.js](https://gist.github.com/feix/6dd1f62a54c5efa10f1e1c24f8efc417)
- [unwxapkg.py](https://gist.github.com/feix/32ab8f0dfe99aa8efa84f81ed68a0f3e)



### 安全论坛

- [OSG-TranslationTeam](https://bbs.pediy.com/thread-212685.htm)
- [基于OLLVM定制的开源混淆工具Hikari](http://iosre.com/t/work-in-progress-ollvm-hikari/10475)
- [woodmann](http://www.woodmann.com/)

### 附录

syscall hook
- [offensive-volatility-messing-with-os-x](http://siliconblade.blogspot.jp/2013/07/offensive-volatility-messing-with-os-x.html)
- [defcon-17-bosse_eriksson-kernel_patching_on_osx](https://www.defcon.org/images/defcon-17/dc-17-presentations/defcon-17-bosse_eriksson-kernel_patching_on_osx.pdf)
- [hatena](http://d.hatena.ne.jp/hon53/20100926/1285476759)
- [SysScan-Singapore-Targeting_The_IOS_Kernel](https://papers.put.as/papers/ios/2011/SysScan-Singapore-Targeting_The_IOS_Kernel.pdf)
- [us-15-Diquet-TrustKit-Code-Injection-On-iOS-8-For-The-Greater-Good](https://www.blackhat.com/docs/us-15/materials/us-15-Diquet-TrustKit-Code-Injection-On-iOS-8-For-The-Greater-Good.pdf)


ios kext load

- [anyKextLoader](https://github.com/LinusHenze/anyKextLoader)
- [trident-kloader](https://github.com/Jailbreaks/trident-kloader)
- [ios-kern-utils](https://github.com/saelo/ios-kern-utils)
- [kexty](https://github.com/xerub/kexty)

### 参考图
>* [sec_skills](https://github.com/feicong/sec_skills)
![](/images/posts/{{page.title}}/ios_skills.png)

>* lldb
![](/images/posts/{{page.title}}/lldb-commands-map.png)

>* SSL相信大家都不陌生。其实Https就是在Http基础上通过SSL协议进行加密之后的网络传输。
![](/images/posts/{{page.title}}/ssl.png)

- [理解 HTTPS 原理，SSL/TLS 协议](https://www.w3cschool.cn/cuhkj/cuhkj-gkna2671.html)
- [https://www.w3cschool.cn/project/iosre](https://www.w3cschool.cn/project/iosre)
- [https://www.w3cschool.cn/iosre/](https://www.w3cschool.cn/iosre/)
