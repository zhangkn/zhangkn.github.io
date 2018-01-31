---
layout: post
title: CycriptTricks
date: 2017-12-29
tag: iOSre
site: https://zhangkn.github.io
---



### 前言

Cycript 是一个能够理解Objective-C语法的javascript解释器， 它能够挂钩正在运行的进程， 以在运行时修改很多东西, 一般我们用于动态调试应用, 如果要调试的代码是用C编写的,通过[lldb](/images/posts/antiDebugger/lldb-commands-map.png)来调试.

>* 刚开始分析逆向的时候，常常利用它进行控制器class的定位;完成类似功能的工具有[AFlexLoader](https://zhangkn.github.io/2017/12/KNAFlexLoader/)、[Passionfruit](https://zhangkn.github.io/2017/12/Passionfruit/)

>* Powerful private methods
```
_ivarDescription
_shortMethodDescription
nextResponder
_autolayoutTrace
recursiveDescription
_methodDescription
```

>* 安装
```
iPhone:~ root# apt-get install cycript
```

###  动态库的注入方式

>* cycript注入动态库的方式

```

在挂载的进程上创建一个挂起的线程, 然后在这个线程里申请一片用于加载动态库的内存,然后恢复线程,动态库就被注入(通过 taskfor_pid函数获取目标进程句柄，然后通过在进程内创建新线程并执行自己的代码。)
```
>* 通过环境变量DYLD_INSERT_LIBRARIES 注入
```
DYLD_INSERT_LIBRARIES=/PathFrom/dumpdecrypted.dylib /PathTo
#New Run Script Phase：
cd ${TARGET_BUILD_DIR}
export DYLD_INSERT_LIBRARIES=./libKNoke.dylib && /Applications/QKNQ.app/Contents/MacOS/QKNQ
```
>* [二次打包动态库的注入,避免每次从环境变量注入--偏静态：通过LC_LOAD_DYLIB实现dylib的加载](https://github.com/Tyilo/insert_dylib)

```
#通过修改可执行文件的Load Commands来实现的. 在Load Commands中增加一个LC_LOAD_DYLIB , 写入dylib路径
Usage: insert_dylib dylib_path binary_path [new_binary_path]

1、现在iOS上的绝大多数以root权限运行的App，都是通过setuid + bash来实现的
2、App运行所需要的信息，一般都存放在其MachO头部43中，其中dylib的信息是由load commands指定的
这些信息是以静态的方式存放在二进制文件里（不是由DYLD_INSERT_LIBRARIES动态指定），而又是由dyld动态加载的，所以我们给它起了个“偏静态”的名字--在此App得到执行时，dyld会查看其MachO头部中的load commands，并把里面LC_LOAD_DYLIB相关的dylib给加载到进程的内存空间
```
[insert_dylib:# Command line utility for inserting a dylib load command into a Mach-O binary ](https://github.com/Tyilo/insert_dylib)
```
修改App可执行文件的头部，给它添加这么一个load command，并指定load我们构造的dylib就好
```



### cycript的常用命令

>* List all subclasses
```
cy# [c for each (c in ObjectiveC.classes) if (class_getSuperclass(c) && [c isSubclassOfClass:UIViewController])]
[c for each (c in ObjectiveC.classes) if (class_getSuperclass(c) && [c isSubclassOfClass:UIView])]
```
>* 格式化
```
.toString()
```

>* 根据地址获取对象。
```
#address
```
>* bundleIdentifier
```
[[NSBundle mainBundle] bundleIdentifier]
```

>* 可执行文件路径
```
[[NSFileManager defaultManager] URLsForDirectory:NSDocumentDirectory inDomains:NSUserDomainMask][0]
```
>* Inject Into Processes
```
iPhone:~ root# cycript -p Mookn
```

>* Objective-C Messages
```
cy# [UIApp description]
@"<DFApplication: 0x4870560>"
cy# [#0x4870560 _ivarDescription].toString()
# 查看 _delegate
cy# *UIApp
isa:UIApplication,_hasAlternateNextResponder:false,_hasInputAssistantItem:false,_delegate:(typedef void*)(0x1467c58e0),
```
>* JavaScript Extensions
```
cy# [for (x of [1,2,3]) x+1]
[2,3,4]
```
>* Bridged Object Model
```
cy# choose(CALayer) instanceof Array
true
```

>* Foreign Function Calls
```
cy# var a = malloc(128)
(typedef void*)(0x8b00b90)
```

>* Magical Tab-Complete
```
cy# a = ({m: 4, b: 5})
{m:4,b:5}
cy# a["m"]
4
```

>* C++11 Lambda Syntax
```
cy#  [&](int a)->int{return a}
(extern "C" int 80904192(int))
```

### 私有API的使用

>* 打印出当前界面的view层级
```
cy# UIApp.keyWindow.recursiveDescription().toString()
```

>* 通过view的nextResponder方法，可以找出它的下一个事件传递来源，常用来确定它所属的视图控制器
```
cy# [#0x181009f0 nextResponder]
```

>* _shortMethodDescription 也可以用于lldb的断点调试，避免地址计算
```
# 1、iPhone端开启debugserver的端口监听
iPhone:~ root# debugserver *:12345 -a "WeKNChat"
# 2、Mac端LLDB的接入：在terminal上输入lldb命令，然后输入下方的地址进行连接。因为我们使用usbmuxd进行了端口的转发，因此可以使用本地的环回测试地址来进行debugserver的连接。
process connect connect://127.0.0.1:12345
# 3、找到需要下断点的类，然后在LLDB命令行输入_shortMethodDescription
(lldb) po [CMesKNsageMgr _shortMethodDescription]
#进行断点
(lldb) b 0x52260f9
#resuming Process
# 查看register
(lldb) register read --all
#补充 ： 打印数据模型内容很有用的私有函数方法[模型对象 _ivarDescription]；
```

>* 快捷的获取ViewControlle(Shortcut to find the ViewController’s class name on the keyWindow): _printHierarchy
```
#支持iOS8之后
cy# [[[UIWindow keyWindow] rootViewController] _printHierarchy].toString()
```
>* _ivarDescription: Prints all names and values of instance variables of a specified object
```
cy# [#0x5822600 _ivarDescription].toString()
```
>* _autolayoutTrace:基于layout展示View架构 
```
cy# [[UIApp keyWindow] _autolayoutTrace].toString()
```
>* choose():get an array of existing objects of a certain class(当前堆栈中查找到特定类的对象数据)
```
cy# a = choose(UILabel).toString()
```





### 参考
- [CycriptTricks](http://iphonedevwiki.net/index.php/Cycript_Tricks)
- [debs](http://www.cycript.org/debs/?C=M;O=D)
- [iOS Reverse Engineering Dev Community](https://github.com/iOS-Reverse-Engineering-Dev)
- [steventroughtonsmith](https://github.com/steventroughtonsmith)
- [Swift-Apps-Reverse-Engineering](https://github.com/zhangkn/Swift-Apps-Reverse-Engineering/blob/master/Reverse%20Engineering%20Swift%20Applications.pdf)
- [iosdevlog](http://iosdevlog.com/)
- [JiaXianhua blog](https://jiaxianhua.github.io/)
- [weak_classdump](https://github.com/limneos/weak_classdump)
- [Powerful private methods for debugging in Cycript & LLDB](http://iosre.com/t/powerful-private-methods-for-debugging-in-cycript-lldb/3414)

- [颤抖吧，■■■■■■■■！手把手教你hook以root权限运行的App](http://iosre.com/t/igrimace-hook-root-app/440)
```
现在iOS上的绝大多数以root权限运行的App，都是通过setuid + bash来实现的
App运行所需要的信息，一般都存放在其MachO头部43中，其中dylib的信息是由load commands指定的
这些信息是以静态的方式存放在二进制文件里（不是由DYLD_INSERT_LIBRARIES动态指定），而又是由dyld动态加载的，所以我们给它起了个“偏静态”的名字
```
- [snakeninny](http://iosre.com/u/snakeninny/activity/topics)
- [mikulove](https://mikulove.com/)
### 附

>* 查找进程名称
```
iPhone:~ root# ps -e |grep  /var/mobile*
```
- [iOS逆向记录(三)](https://www.jianshu.com/p/7ab7234f5187)


