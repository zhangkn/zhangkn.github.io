---
layout: post
title: antiDebugger
date: 2017-12-18
tag: iOSre
site: https://zhangkn.github.io
---
### 前言

总结入门级的东西的时候，代码互相抄很正常，但是解释的部分最好自己写。

软件的逆向工程指的是通过分析一个程序或系统的功能、结构或行为，将它的技术实现或设计细节推导出来的过程。
当我们因为工作需要，或是对一个软件的功能很感兴趣，却又拿不到它的源代码时，往往可以通过逆向工程的方式对它进行分析，探索它的实现原理。
一个生动的比喻：照着配方包饺子,是正向开发 吃着饺子推配方（API 调用顺序）,是逆向工程。




### iOS 逆向分析方法




>* 1、网络分析

抓包工具有 tcpdump, WireShark, Charles

>* 2、[静态分析](https://zhangkn.github.io/2017/01/iOS_Wifilist/#gsc.tab=0)

在app没运行的情况下，借助反汇编工具（hopper等）查看代码内部结构、借助otool工具查看二进制文件信息、class-dump  目标对象的 class 信息等分析过程。分析的信息包括网络协议、数据格式。

>* 3、 动态分析(动态调试、代码跟踪)
通过借助lldb、Cycript、frida、AFLEXLoader、运行时常用的API等工具进行调试、分析代码，获取内存状态甚至修改内存的过程。改过程包括反反调试等。



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
恶意代码分析、漏洞研究、COTS验证、隐私保护



### 安全

- 使用二进制协议传输数据
- 类名方法名混淆、越狱检测、核心代码使用swift、c\c++ 实现
- 反调试





### class-dump的原理

利用 Objective-C 语言的 runtime 特性，将存储在 Mach-O 文件中的 @interface 和 @protocol 信息提取出来，并生成相应的 .h 文件

### 参考资源
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
 


###  参考博客

- [jmpews](http://jmpews.github.io/2017/08/09/darwin/%E5%8F%8D%E8%B0%83%E8%AF%95%E5%8F%8A%E7%BB%95%E8%BF%87/)
- [/iOS-Reverse-Engineering-presentation](https://aozhimin.github.io/iOS-Reverse-Engineering-presentation/#/title) aozhimin
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
