---
layout: post
title: usefulConfig
date: 2018-03-26
tag: tool
site: https://zhangkn.github.io
---

## 前言




## 正文


### tweak


>* /Package/Library/MobileSubstrate/DynamicLibraries/.plist

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Filter</key>
  <dict>
    <key>Executables</key>
    <array>
      
      <string>nesessionmanager</string>
            <string>racoon</string>
            <string>configd</string>
            <string>mDNSResponder</string>
            <string>networkd</string>
    </array>
        
        <key>Bundles</key>
        <array>
            <string>com.apple.nesessionmanager</string>
        </array>
  </dict>
</dict>
</plist>
```





### see also


- [得到App 逆向分析学习过程](https://www.jianshu.com/p/a011e6c8ba51)

```

<!-- ./class-dump -S -s -H /Users/hz/Desktop/得到/Payload/LuoJiFM-IOS.app/LuoJiFM-IOS -o ~/Desktop/DeDao -->




<!-- 网络请求是用的 DataService -->

<!-- DataServiceUtils 这个类是算加密, header之类的 -->

<!-- DDDataService 这个 应该是 DataServiceV2 的升级版 -->


<!-- FMEventDataService 封装的进行网络请求的地方 -->







```

- [微信共存防封版伪代码分析](https://www.jianshu.com/p/e797ba55e336)

```

<!-- addone.dylib. -->

CHLoadClass_(0xe0f8, objc_getClass("NSBundle"));
CHLoadClass_(0xe104, objc_getClass("UIDevice"));//修改设备名称
CHLoadClass_(0xe110, objc_getClass("NSDictionary"));
CHLoadClass_(0xe11c, objc_getClass("MMCrashReportExtLogMgr"));//崩溃记录
CHLoadClass_(0xe128, objc_getClass("JailBreakHelper"));//越狱检测
CHLoadClass_(0xe134, objc_getClass("ASIdentifierManager"));//修改广告标识

<!-- UIDevice -->

int __ZL21$UIDevice_name_methodP8UIDeviceP13objc_selector(void * arg0, void * arg1) {
    r0 = @"iPhone";
    return r0;
}


<!-- 防封补丁源码 -->


#import <Foundation/Foundation.h>
#import "CaptainHook/CaptainHook.h"
#import <AdSupport/AdSupport.h>

CHDeclareClass(ASIdentifierManager)

//广告标识符伪装
CHMethod0(NSUUID *, ASIdentifierManager, advertisingIdentifier)
{
    NSUUID *advertisingIdentifier;
    NSString *key = @"idfa";
    
    NSString *idfa = [[NSUserDefaults standardUserDefaults] stringForKey:key];
    
    if (idfa && idfa.length)
    {
        advertisingIdentifier = [[NSUUID alloc] initWithUUIDString:idfa];
    }
    else
    {
        advertisingIdentifier = [NSUUID UUID];
        
        [[NSUserDefaults standardUserDefaults] setObject:advertisingIdentifier.UUIDString forKey:key];
    }
    
    return advertisingIdentifier;
}

@class BaseAuthReqInfo, BaseRequest, ManualAuthAesReqData;

CHDeclareClass(ManualAuthAesReqData);

//bundleId 伪装(待完善)
CHMethod1(void, ManualAuthAesReqData, setBundleId, NSString *, bundleId)
{
    if ([bundleId isEqualToString:[NSBundle mainBundle].bundleIdentifier])
    {
        bundleId = @"com.tencent.xin";
    }
    
    CHSuper1(ManualAuthAesReqData, setBundleId, bundleId);
}

//clientSeqId 伪装
CHMethod1(void, ManualAuthAesReqData, setClientSeqId, NSString *, clientSeqId)
{
    NSString *key = @"clientSeqId";
    NSString *clientSeqId_fist = [[NSUserDefaults standardUserDefaults] stringForKey:key];
    if (!clientSeqId_fist || clientSeqId_fist.length == 0)
    {
        clientSeqId_fist = [[NSUUID UUID].UUIDString stringByReplacingOccurrencesOfString:@"-" withString:@""];
        [[NSUserDefaults standardUserDefaults] setObject:clientSeqId_fist forKey:key];
    }
    
    NSString *newClientSeqId;
    
    if ([clientSeqId containsString:@"-"])
    {
        NSRange range = [clientSeqId rangeOfString:@"-"];
        NSString *clientSeqId_last = [clientSeqId substringFromIndex:range.location];
        
        newClientSeqId = [NSString stringWithFormat:@"%@%@", clientSeqId_fist, clientSeqId_last];
    }
    else
    {
        newClientSeqId = clientSeqId_fist;
    }
    
    CHSuper1(ManualAuthAesReqData, setClientSeqId, newClientSeqId);
}

//deviceName 伪装
CHMethod1(void, ManualAuthAesReqData, setDeviceName, NSString *, deviceName)
{
    //设置为默认名称
    deviceName = @"iPhone";
    
    CHSuper1(ManualAuthAesReqData, setDeviceName, deviceName);
}

//过日志记录
@class MMCrashReportExtLogMgr;

CHDeclareClass(MMCrashReportExtLogMgr);

CHMethod2(void, MMCrashReportExtLogMgr, addLogInfo, int *, arg1, withMessage, const char *, arg2)
{
    return;
}

//过越狱检测
@class JailBreakHelper;

CHDeclareClass(JailBreakHelper);

CHMethod0(BOOL, JailBreakHelper, HasInstallJailbreakPluginInvalidIAPPurchase)
{
    return NO;
}

CHMethod1(BOOL, JailBreakHelper, HasInstallJailbreakPlugin, id, arg1)
{
    return NO;
}

CHMethod0(BOOL, JailBreakHelper, IsJailBreak)
{
    return NO;
}

//所有被hook的类和函数放在这里的构造函数中
CHConstructor
{
    @autoreleasepool
    {
        CHLoadLateClass(ASIdentifierManager);
        CHHook0(ASIdentifierManager, advertisingIdentifier);
        
        CHLoadLateClass(ManualAuthAesReqData);
        CHHook1(ManualAuthAesReqData, setBundleId);
        CHHook1(ManualAuthAesReqData, setClientSeqId);
        CHHook1(ManualAuthAesReqData, setDeviceName);
        
        CHLoadLateClass(MMCrashReportExtLogMgr);
        CHHook2(MMCrashReportExtLogMgr, addLogInfo, withMessage);
        
        CHLoadLateClass(JailBreakHelper);
        CHHook0(JailBreakHelper, HasInstallJailbreakPluginInvalidIAPPurchase);
        CHHook1(JailBreakHelper, HasInstallJailbreakPlugin);
        CHHook0(JailBreakHelper, IsJailBreak);
    }
}



```

- [JSPatch 实现原理详解](https://github.com/bang590/JSPatch/wiki/JSPatch-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86%E8%AF%A6%E8%A7%A3)

```
<!-- 基础原理 : Objective-C Runtime  -->

https://jspatch.com/Index/sdk


JS 传递字符串给 OC，OC 通过 Runtime 接口调用和替换 OC 方法

<!-- 方法调用 :引入 JSPatch 后，可以通过以上 JS 代码创建了一个 UIView 实例，并设置背景颜色和透明度，涵盖了 require 引入类，JS 调用接口，消息传递，对象持有和转换，参数转换这五个方面-->

require('UIView')
var view = UIView.alloc().init()
view.setBackgroundColor(require('UIColor').grayColor())
view.setAlpha(0.5)



<!-- 1.require -->

在JS全局作用域上创建一个同名变量

<!-- 2.JS接口 -->

1)在require生成类对象时，把类名传入OC，OC 通过runtime方法找出这个类所有的方法返回给 JS，JS 类对象为每个方法名都生成一个函数，函数内容就是拿着方法名去 OC 调用相应方法

2)CoffieScript/JSX 都可以用 JS 实现一个解释器实现自己的语法

3)调用一个不存在方法时，能转发到一个指定函数去执行:简单的字符串替换，把 JS 脚本里的方法调用都替换掉

最后的解决方案是，在 OC 执行 JS 脚本前，通过正则把所有方法调用都改成调用 __c() 函数，再执行这个 JS 脚本，做到了类似 OC/Lua/Ruby 等的消息转发机制：

<!-- 3.消息传递 -->

JavaScriptCore 的接口

1)OC 端在启动 JSPatch 引擎时会创建一个 JSContext 实例，JSContext 是 JS 代码的执行环境，可以给 JSContext 添加方法，JS 就可以直接调用这个方法：

JSContext *context = [[JSContext alloc] init];
context[@"hello"] = ^(NSString *msg) {
    NSLog(@"hello %@", msg);
};
[_context evaluateScript:@"hello('word')"];     //output hello word



2)JS 通过调用 JSContext 定义的方法把数据传给 OC，OC 通过返回值传会给 JS。调用这种方法，它的参数/返回值 JavaScriptCore 都会自动转换，OC 里的 NSArray, NSDictionary, NSString, NSNumber, NSBlock 会分别转为JS端的数组/对象/字符串/数字/函数类型。



<!-- 4.对象持有/转换 -->

<!-- 5.类型转换 -->




```

```

<!-- 方法替换 -->

<!-- 1.基础原理 -->


OC上，每个类都是这样一个结构体：


struct objc_class {
  struct objc_class * isa;
  const char *name;
  ….
  struct objc_method_list **methodLists; /*方法链表*/
};

其中 methodList 方法链表里存储的是 Method 类型：


typedef struct objc_method *Method;
typedef struct objc_ method {
  SEL method_name;
  char *method_types;
  IMP method_imp;
};

Method 保存了一个方法的全部信息，包括 SEL 方法名，type 各参数和返回值类型，IMP 该方法具体实现的函数指针。



<!-- 替换 UIViewController 的 -viewDidLoad: 方法为例 -->

static void viewDidLoadIMP (id slf, SEL sel) {
   JSValue *jsFunction = …;
   [jsFunction callWithArguments:nil];
}

Class cls = NSClassFromString(@"UIViewController");
SEL selector = @selector(viewDidLoad);
Method method = class_getInstanceMethod(cls, selector);

//获得viewDidLoad方法的函数指针
IMP imp = method_getImplementation(method)

//获得viewDidLoad方法的参数类型
char *typeDescription = (char *)method_getTypeEncoding(method);

//新增一个ORIGViewDidLoad方法，指向原来的viewDidLoad实现
class_addMethod(cls, @selector(ORIGViewDidLoad), imp, typeDescription);

//把viewDidLoad IMP指向自定义新的实现
class_replaceMethod(cls, selector, viewDidLoadIMP, typeDescription);


https://blog.nelhage.com/2010/10/amd64-and-va_arg


```


- [玩转 Hook 新思路：JSPatch](https://dskcpp.github.io/2016/04/25/JSPatch/)

```
<!-- JSPatch 是一个iOS 动态更新框架，只需在项目中引入极小的引擎，就可以使用JavaScript 调用任何Objective-C 原生接口，获得脚本语言的优势：为项目动态添加模块，或替换项目原生代码动态修复bug -->


<!-- 1、 打开Xcode, 新建Project, 选择 iOSOpenDev 中 CaptainHook Tweak, -->
添加依赖框架：JavaScriptCore.framework, libz.1.2.8.tbd

https://link.jianshu.com/?t=http://jspatch.com/Index/sdk


<!-- jsPatch.mm -->

//需要hook的方法


#import <JSPatchPlatform/JSPatch.h>


CHDeclareClass(MicroMessengerAppDelegate);

////JSPatch初始化

CHMethod(2, BOOL, MicroMessengerAppDelegate, application, id, arg1, didFinishLaunchingWithOptions, id, arg2)
{
    BOOL supBool = CHSuper(2, MicroMessengerAppDelegate, application, arg1, didFinishLaunchingWithOptions, arg2);
    [JSPatch startWithAppKey:@"xxx"];
    [JSPatch sync];
    return supBool;
}

//所有被hook的类和函数放在这里的构造函数中

__attribute__((constructor)) static void entry()
{
    CHLoadLateClass(MicroMessengerAppDelegate);
    CHHook(2, MicroMessengerAppDelegate, application, didFinishLaunchingWithOptions);
}


<!-- 方法调用 main.js-->

require('MMTableViewSectionInfo, MMTableViewCellInfo, MMTableViewInfo')

//打印所有 ViewController
defineClass('UIViewController', {
viewDidAppear:function(animated) {
            console.log(self)
        }
})    

//给 “我” 这个界面添加一个 cell
defineClass('MoreViewController', {
        addSettingSection:function() {
            self.ORIGaddSettingSection()
            var section = MMTableViewSectionInfo.sectionInfoDefaut()
            var cell = MMTableViewCellInfo.switchCellForSel_target_title_on(null, self, "Hello JSPatch", false)
            section.addCell(cell)
            var info = self.valueForKey("m_tableViewInfo")
            info.addSection(section)
    }   
})

```


- [ln](http://www.cnblogs.com/peida/archive/2012/12/11/2812294.html)

```



必要参数:

-b 删除，覆盖以前建立的链接

-d 允许超级用户制作目录的硬链接

-f 强制执行

-i 交互模式，文件存在则提示用户是否覆盖

-n 把符号链接视为一般目录

-s 软链接(符号链接)

-v 显示详细的处理过程


实例5：给目录创建软链接

# 先备份 cp -r /Library/MobileSubstrate/DynamicLibraries ~/
     # rm -rf /Library/MobileSubstrate/DynamicLibraries
    # aso11 代码： dylib   干脆给他建立个软连接算了        /bin/ln -s   /usr/lib/TweakInject /Library/MobileSubstrate/DynamicLibraries 
    # cp -r  ~/DynamicLibraries /Library/MobileSubstrate/DynamicLibraries


<!-- ln -sv /opt/soft/test/test3 /opt/soft/test/test5 -->

ln -sv   /usr/lib/TweakInject /Library/MobileSubstrate/DynamicLibraries      这个相当于创建同名的软连接


WL-47:~ root# ls -lrt /Library/MobileSubstrate/DynamicLibraries
lrwxr-xr-x 1 root wheel 25 Feb 13 12:48 /Library/MobileSubstrate/DynamicLibraries -> ../../usr/lib/TweakInject


```