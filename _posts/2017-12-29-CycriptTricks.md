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


>* 动态库的注入方式
```
# 二次打包动态库的注入
通过修改可执行文件的Load Commands来实现的. 在Load Commands中增加一个LC_LOAD_DYLIB , 写入dylib路径
# cycript注入动态库的方式
在挂载的进程上创建一个挂起的线程, 然后在这个线程里申请一片用于加载动态库的内存,然后恢复线程,动态库就被注入
```

>* Powerful private methods
```
_ivarDescription
_shortMethodDescription
nextResponder
_autolayoutTrace
recursiveDescription
_methodDescription
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




### 参考
- [CycriptTricks](http://iphonedevwiki.net/index.php/Cycript_Tricks)
- [debs](http://www.cycript.org/debs/?C=M;O=D)
- [iOS Reverse Engineering Dev Community](https://github.com/iOS-Reverse-Engineering-Dev)
- [steventroughtonsmith](https://github.com/steventroughtonsmith)
- [Swift-Apps-Reverse-Engineering](https://github.com/zhangkn/Swift-Apps-Reverse-Engineering/blob/master/Reverse%20Engineering%20Swift%20Applications.pdf)
- [iosdevlog](http://iosdevlog.com/)
- [JiaXianhua blog](https://jiaxianhua.github.io/)
- [weak_classdump](https://github.com/limneos/weak_classdump)


### 附

>* 查找进程名称
```
iPhone:~ root# ps -e |grep  /var/mobile*
```



