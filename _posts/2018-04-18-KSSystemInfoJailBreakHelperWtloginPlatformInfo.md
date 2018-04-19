---
layout: post
title: KSSystemInfoJailBreakHelperWtloginPlatformInfo
date: 2018-04-18
tag: iOSre
site: https://zhangkn.github.io
---

### 前言


>* KSSystemInfo 、JailBreakHelper、 WtloginPlatformInfo

```
JailBreakHelper

KSSystemInfo

WtloginPlatformInfo

SafeDeviceData

<!-- [SafeDeviceData uuid](void * self, void * _cmd) { -->

OpenUDID

<!-- [OpenUDID _generateFreshOpenUDID]( -->


```





### see also

- [Jekyll](https://blog.hidva.com/2016/02/24/Jekyll%E6%96%87%E6%A1%A3/)

```
<!-- Jekyll markdown 渲染器 -->

# 指定使用 kramdown 作为 markdown 渲染器
markdown: kramdown

kramdown:
    input: GFM

<!-- github page -->


<!-- Catalog -->

+## Catalog
 
 
 1.  [Foreword](#foreword)
 2.  [CommonJS & Node](#commonjs--node)
-3.  [History](#node)
+3.  [History](#history)
 4.  [RequireJS & AMD](#requirejs--amd)
 5.  [SeaJS & CMD](#seajs--cmd)
 6.  [AMD vs CMD](#amd-vs-cmd)


<!-- https://github.com/hidva/hidva.github.io/commit/ad039574431f8b74ad95665f0914448bb641c95a -->



```

- [The Ultimate iOS Crash Reporter](https://github.com/kstenerud/KSCrash)

```

https://github.com/WxHook/MMStructure


https://github.com/jonnermut/KSCrash/blob/master/Source/KSCrash/Recording/KSSystemInfo.m

<!-- 旧版登录失败 -->

用6.6.0登录完子之后再用6.5.8的覆盖安装，再重新登录一下

```
- [iOS逆向 - xx的越狱检测分析](http://www.bijishequ.com/detail/365974)

```
<!-- bundleID、uuid、jailbroken -->

+ (id) [KSSystemInfo systemInfo]  方法的返回值


    CFBundleExecutable = WeChat;
    CFBundleExecutablePath = "/var/mobile/Containers/Bundle/Application/DE497A13-E76F-42B8-B476-A9934B486EF4/WeChat.app/WeChat";
    CFBundleIdentifier = "com.tencent.xin";
    CFBundleName = WeChat;
    "app_uuid" = "AAAAA633-1E42-32DE-B943-2C16A47C7A3E";
    "cpu_arch" = arm64;
    "device_app_hash" = 9eb422144a96af8f91XXXXxxxxxdaf0d4742321b;
    jailbroken = 0;
    machine = "iPhone6,2";
    memory = { size = 1048576000; };
    "system_name" = "iPhone OS";
    "system_version" = "9.2.1";
    ... 
    后面省略一千个字段
    ...

<!-- sub_ 开头的一看就是 c 语言的函数 -->

调用 _dyld_image_count() 获得加载的动态库的数量，_dyld_get_image_name() 获得名字，然后遍历他们的名字，看看有没有 “MobileSubstrate” 关键字，有的话就是越狱的


#import <mach-o/dyld.h>  

int count = _dyld_image_count();
for (int i=0; i<count; i++) {
    printf("%s", _dyld_get_image_name(i));
    // 如果 _dyld_get_image_name() 里面包含 MobileSubstrate 就是越狱了
}


<!-- 用 + [KSSystemInfo isJailbroken] 来判断是否越狱的 -->

<!-- JailBreakHelper -->

继续往下看 - [JailBreakHelper IsJailBreak] 方法的代码，还原里面一个数组的代码大概是

NSArray *paths = @[
    @"/etc/ssh/sshd_config",
    @"/usr/libexec/ssh-keysign",
    @"/usr/sbin/sshd",
    @"/bin/sh",
    @"/bin/bash",
    @"/etc/apt",
    @"/Applications/Cydia.app/",
    @"/Library/MobileSubstrate/MobileSubstrate.dylib"];



http://www.bijishequ.com/authorarticle.html?author=ck2016

<!-- KSSystemInfo应该是闪退的时候上传的，有个类叫KSCrash -->

 KSCrash +(id)sharedInstance { return nil; }

KSCrash /KSCrash/KSCrash/KSSystemInfo.m: https://github.com/jonnermut/KSCrash


```