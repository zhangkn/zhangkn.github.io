---
layout: post
title: DevelopmentSkillsIntroduced
date: 2018-04-11
tag: work
site: https://zhangkn.github.io
---


###  [前言](https://zhangkn.github.io/about/)


>* [充满激情的逆向工程师、C/C++/java/Object-C/Swift开发者、安全研究员。](https://about.me/kunnan)

```
	1、自学Object-C、Swift、lua、iOS逆向开发(其中包括阅读ARM汇编)。

	2、熟悉java开发、有前后端开发经验者;

	3、对待工作主动积极，责任心强，对代码规范有轻微强迫症，能良好处理人际关系。 爱好游泳、羽毛球、篮球。

	4、https://zhangkn.github.io、https://about.me/kunnan
```

### 正文


### 逆向技能

>* iOS 逆向开发,熟悉iphone/tweak、iphone/tool、cydia的repo 制作

```
利用lldb、hopper、theos、MonkeyDev、运行时常用的API 进行开发。结合lldb、Cycript、hopper、KNHook、以及AFLEXLoader进行分析快速找到入口。

```
>* [iphone/tool 项目例子：MobileWiFi.framework的使用](https://github.com/sbtweaks/wifiutil)

>* [iphone/tweak类型举例： tweak集成CocoaAsyncSocket](https://github.com/zhangkn/KNCocoaAsyncSocketDemo)

>* [分析工具: KNHook ](https://github.com/zhangkn/hookClass)

>* 常见的自动化的功能的实现

```
自动设置永不锁屏、自动处理进程通信的问题、网络异常的时候自动关闭VPN（防止VPN有效性过期）,

并且自动断开Wi-Fi，再次重新连接（防止IP有效期超时）

```


>* 具体的通用技能

```
Theos（iphone/tool、iphone/tweak）、socat、class dump、Cycript、lua、AFlexLoader、Hopper、usbMuxd、CocoaAsyncSocket、tcpdump、Wireshark(rvictl)、nm、symbolicatecrash、host cydia repo 、jshook、debugserver

```

>* iOS PrivateFrameworks(iPhone:~ root# ls -lrt /System/Library/PrivateFrameworks)

```

1) MobileWiFi.framework:   启动处理Wi-Fi的连接断开

2） SpringBoardServices.framework

3） <Foundation/NSTask.h>： task launch


```

>* ios CoreServices

```
1) /System/Library/CoreServices/SpringBoard.app/SpringBoard

```

>* MacOSX.sdk/usr/include

```
/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include


1) spawn.h: 

int	posix_spawnp(pid_t * __restrict, const char * __restrict,
const posix_spawn_file_actions_t *,
const posix_spawnattr_t * __restrict,
char *const __argv[ __restrict],
char *const __envp[ __restrict])

2)stdio.h: popen(const char *, const char *)

```

>* jailbreak toolkit 

```

1)  Electra iOS 11.0 - 11.1.2 jailbreak toolkit:定制一些自己的功能 https://github.com/jailbreakTool/KNelectra

2) yalu102 iOS 10.0.0 -> iOS 10.2 : 修改jailbreak.m、dropbear，适配Xcode9 ；https://github.com/jailbreakTool/KNyalu102

https://zhangkn.github.io/2018/04/SystemIsUnavailable/

```


### 正向技能

>* 熟悉Swift/C/C++/Objective C/Java，熟练使用Xcode开发工具；

```
1） 消息推送机制、KVC、KVO、GCD、block、RAC(ReactiveCocoa)、CocoaPod、IAP内购、Method Swizzling、lldb、UIBackgroundModes、NSPredicate；

2） iOS CocoaTouchStaticLibrary: 探索了出了一个提高静态库工程开发的搭建方法 https://github.com/zhangkn/KNCocoaTouchStaticLibrary
```

>* 深入理解MRC和ARC内存管理机制；

```

1) 需要释放的资源：imageCache、queue、operations、view、通知监听者的移除、销毁soundID 

2） 释放的方法：dealloc 、applicationDidReceiveMemoryWarning、didReceiveMemoryWarning

3）凡是函数名中带有create、copy、new、retain等字眼的，都应该在不需要这个数据的时候进行release。
   GCD的数据类型在ARC环境下不需要进行release；[而CF的数据类型在ARC、MRC环境下都需要做release的](http://blog.csdn.net/z929118967/article/details/74332012) 

4）  内存管理的补充：foundation框架（OC语言）、core foundation 框架（C语言,例如通讯录就是基于这个框架）

```

>* [熟悉iOS中多线程和GCD的使用](https://github.com/idetool/JSPatch/blob/master/Extensions/JPDispatch.m)

```
0）queue： dispatch_get_global_queue、dispatch_get_main_queue、dispatch_queue_create

1）dispatch：dispatch_after、dispatch_async、dispatch_sync、dispatch_barrier_async（ 场景：在写的过程中不能被读：要等到当前所有并发的block都执行完毕，才会单独执行这个barrier block代码块，等到这个barrier block执行完毕，再继续正常处理其他并发bloc）

2）dispatch_group：dispatch_group_t、dispatch_group_async、dispatch_group_wait、dispatch_group_notify、dispatch_group_enter、dispatch_group_leave-- 简单的场景：请求多个接口，再进行View的渲染

3) dispatch信号量（dispatch semaphore）:
//    dispatch_semaphore_t  semaphore = dispatch_semaphore_create(0);
//    dispatch_semaphore_wait(semaphore,dispatch_time(DISPATCH_TIME_NOW, (int64_t)( 16 * NSEC_PER_SEC)));//保证是同步的
dispatch_semaphore_signal(semaphore);//在特定的条件激活信号，往下执行code；有些场景使用递归实现更好

```

>* 熟练使用AFNetworking, MBProgressHUD, MJRefresh, Masonry, SDWebImage等第三方库；熟悉CocoaPods管理工具；熟练使用git和svn等版本管理工具。

>* 具有良好的编码习惯, 掌握常用的数据结构和算法；

```
良好的英文开发文档阅读能力、面向对象编程、链式编程以及常用的设计模式:MVVM、职责链模式
```

>* 对前端后台开发技术也有一定的了解；

```
jQuery、JSP、css、html、数据库(sql基本操作)、j2ee(Spring + Struts +Hibernate)、tomcat
```

>* [unix](https://github.com/zhangkn/KNBin)

```
1） unix（文件目录结构、df、dm、sort、find、grep、cat、vi、ssh、scp）

2） 自己写的shell工具：https://github.com/zhangkn/KNBin

```

>* [个人技术主页](https://zhangkn.github.io/tags/)



### 工作经历


	主要负责: vpn类app、导购类app、AppStore应用协助审核，马甲包制作； App代码加密和混淆工具开发； AppStore排行榜提升，ASO关键词优化； 


#### wl


>* 2017/07 -- 至今

```
<!-- Anti ptrace-->

1)开发工具：Cycript、LLDB、logos Tweak、hopper、MonkeyDev、AFLEXLoader、dumpdecrypted、debugserver、ssh、class_dump、hook

<!-- 简单的例子 -->
- 举例： Anti ptrace https://github.com/zhangkn/KNAlipayWalletTweakDemo

- tweak 项目例子： 快速搭建CocoaAsyncSocket：https://github.com/zhangkn/KNCocoaAsyncSocketDemo

- [iphone/tool 项目例子：MobileWiFi.framework的使用](https://github.com/sbtweaks/wifiutil)

- 阿里百川SDK 的集成

- 收获： Swift、lua、iOS逆向开发(其中包括阅读ARM汇编)

```

#### hisun


1) 负责iOS的app包括：和包商户版、和包客户端、和聚宝、和包刷卡、商户接入和包支付能力项目 以及为和飞信app 提供安全支付组件

2) 对iOS商户和包支付插件项目进行代码重构，进行底层的网络框架进行封装。独立设计iOS应用架构。

3) 对ICS 金融平台有较深刻的理解，能熟练开发与第三方系统的接入接出接口。

4) 主要职责：完成app的设计、开发、测试、修改bug等工作，包括业务需求的沟通，功能模块详细设计，业务功能实现与单元测试。

5) 除了担任iOS工程师角色外，还担任过项目经理，以及维护支付平台的UI后台管理系统。UI对Portal、IPOS、SMS、Wap、CAS、IVR一共6个渠道提供了规则的配置。


>* 2016/02 -- 2017/07

```

<!-- 和飞信（iOS APP） -->

1) 担任iOS工程师角色。
负责和飞信中的扫码付、支付密码管理、绑卡、充流量、发红包功能，以静态库的形式提供 。

2) 负责书写接入文档，维护技术方案文档，解决联调问题。

3) 此项目的使用的开源框架为： ASIHttprequest、SDWebImage、TouchXML。

4) 其中touchXML 适用于解析后台的XML格式报文。

5）技术上的心得：使用GCD和宏实现单例；使用随机数利用CC_MD5方法生成唯一字符串，存储于钥匙串中（ generate the UDID like Apple does）。

6）截图反馈功能的实现，https://github.com/zhangkn/IOSStudy；图片的上传地址进行更换就可以应用到其他项目中（主要采用自定义View实现）。

```


>* 2015/05 -- 2015/07

```
<!-- 搭建一个提高开发效率的iOS静态库工程-->

1）搭建一个提高开发效率的iOS静态库工程，详见：http://blog.csdn.net/z929118967/article/details/73872024。

 这段期间的技术提升：对自定义控件、消息推送、通知、键盘处理、屏幕适配、iPad的开发等实用技术进行锻炼；加强自身对UIKit以及Map Kit的理解以及谓词NSPredicate技术的应用。


2）体会了真正的MVC思想：视图不依赖于具体的数据类型，而是依赖于遵守特定协议的数据源。

M 和V 是不存在依赖关系：就像UIKit中的UItableview一样，什么样的数据M，UItableview都可以展示，只要M遵守实现了UITableViewDataSource协议。

详见：http://blog.csdn.net/z929118967/article/details/74066993

```

>* 2015/03 -- 2017/08

```
<!-- 和包支付能力SDK（iOS CocoaTouchStaticLibrary） -->


1) 担任iOS工程师角色。负责整个静态库工程的搭建，以及demo工程的创建。

2)负责和包商户接入支付能力组件的问题解决。

3)维护接入文档，以及技术方案文档。

4)收获：探索了出了一个提高静态库工程开发的搭建方法，并在GitHub创建了对应的开源项目。

5) 此项目模板完美解决静态库工程和demoApp工程的集成，提高开发调试效率,方便静态库的源码和demo源码的管理维护。(其实也可以采用xcworkspace 进行对xcodeproj的管理)

具体详见我的个人博客文章： http://blog.csdn.net/z929118967/article/details/73872024

对应的代码保存在GitHub： https://github.com/zhangkn/KNCocoaTouchStaticLibrary 

https://github.com/zhangkn/KNAPP

```

>* 2015/01 -- 2017/07

```
<!-- 和包商户版（iOS app） -->

1) 负责整体app开发和维护 。主要功能有消息中心，订单查询，订单消息推送功能、收款。

2) 技术收获：AFN、持久化框架是CoreData存储消息。订单列表的展示使用了谓词技术（https://blog.csdn.net/z929118967/article/details/74066170），实现按照日期进行排序。

3) 开发过程使用PushMeBaby工具调试消息推送，实现了订单的消息推送功能。采用Charles工具进行问题分析，进行报文分析，修改报文进行开发以及单元测试。

4) 通过此项目熟练的掌握了苹果的消息推送机制。提升了商户收款的流程。主要有扫码付、动态密码支付、客户端支付方式进行收款。
从此项目嵌入的“我要开店”、“商户资料维护”H5功能模块;
总结出app嵌入H5页面的通用模版。具体详见：http://blog.csdn.net/z929118967/article/details/73971895

```

>* 2014/01 -- 2017/07

```
<!-- 和包支付（和包官方iOS客户端） -->
1）技术收获：ASI实现网络请求功能、采用静态库的形式提供安全支付组件给主端集成。优化静态库工程的网络框架的代码，解决了由于更换第三方开源框架导致业务逻辑代码大范围修改的问题。

2）收获了支付产品的业务知识：支付方式主要有有账户和无账户体系。这两个体系下有借记卡和贷记卡支付，以及快捷和无磁有密支付方式。

3）熟练掌握了静态库工程的开发技能。并使用自己搭建的一套快速高效实现功能的静态库工程。并应用于和包商户版app、和飞信、商户的安全支付组建等项目。
总结出一套，集成方唤起支付插件，以及支付插件自己退出界面的方法。

```

>* 2013/11 -- 2014/01

```
<!-- UI后台管理系统 -->
熟悉并掌握了支付平台的支付流程、基础模块以及客户端模块的业务。对JS、JSP、SQL、Linux技术的进一步巩固
```

>* 2013/09 -- 2014/02

```
<!-- 和包支付平台 -->
担任java工程师。在无线渠道部门，设计和开发移动客户端的接口，来满足app需求的开发。
通过此角色，能熟练使用高阳的ICS金融平台,对职责链模式有更深刻的理解。掌握了支付密码、登陆密码的加密、转加密流程。

```


### 求职意向

>* 期望职业：IOS逆向工程师

>* 目前状况：目前暂无跳槽打算

>* 期望月薪：xx000-xx000元/月

>* 工作地区：长沙

>* 工作性质：全职


### see also

- [proginn](https://www.proginn.com/wo/140195)
- [zhaopin](https://m.zhaopin.com/Resume/ResumePreview/?resumeNum=JM295406342R90250000000&language=1)
- [A tool kit to generate snapshots with off-screen view and off-screen web view. 一个生成长图/快照的框架。](https://github.com/ShannonChenCHN/SCSnapshotManager)



