---
layout: post
title: Inter-processCommunicationByRrocketbootstrap
date: 2018-01-10
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

 These are some of the existing methods to implement IPC on iOS:
- [URL Scheme](http://blog.csdn.net/z929118967/article/details/77449097)
- [Keychain]
- [UIDocumentInteractionController]
- [利用socket进行本地通信](https://github.com/zhangkn/MPProcessMessage)
- [Mach Ports]
- [Pasteboard]
- [AppleEvents & AppleScript]
- [Distributed Objects]
- [XPC](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html)
- [Notifications](http://iphonedevwiki.net/index.php/Notifications)
- [libobjcipc](http://iphonedevwiki.net/index.php/Libobjcipc)
- [LightMessaging](https://github.com/rpetrich/LightMessaging)

>* Community Libraries to Help

```
1、RocketBootstrap： Service registration and lookup system for iOS 
2、OBJCIPC ：High-level API for hosting services inside apps (by Alan Yip/a1anyip)
3、LightMessaging： Header-only library for simple IPC
```

>* 本文重点讲解 RocketBootstrap的两种包装方式：CFMessagePort、CPDistributedMessagingCenter
```
Easy to use wrappers for CFMessagePort and CPDistributedMessagingCenter(todo: XPC) 
```


### UIPasteboard\NSPasteboard

>* [OpenUDID的使用例子](https://github.com/zhangkn/OpenUDID/blob/master/OpenUDID.m)
```
#if TARGET_OS_IPHONE || TARGET_IPHONE_SIMULATOR
#import <UIKit/UIPasteboard.h>
#import <UIKit/UIKit.h>
#else
#import <AppKit/NSPasteboard.h>
#endif
#if TARGET_OS_IPHONE || TARGET_IPHONE_SIMULATOR
        UIPasteboard* slotPB = [UIPasteboard pasteboardWithName:slotPBid create:NO];
#else
        NSPasteboard* slotPB = [NSPasteboard pasteboardWithName:slotPBid];
#endif
```

>* [//pasteboardWithName 方法的使用](https://github.com/zhangkn/KNcodeSnippets/blob/master/KNcodeSnippets/KNUIPasteboardIPC.m)

```
/**
 应用级别的，数据在属于自己的应用内部共享；
 (默认情况下是不会把数据写进沙盒的，也就是说（复制、剪切）粘贴内容会因为应用的退出而销毁掉，我们可以设置相关属性 persistent值为 YES让其进行数据的持久化存储起来)
 
 Ps：例如 persistent 是否进行数据持久化 还有 changeCount 改变次数（剪切板）系统重启方才重新计数
 
 */
- (NSObject *)model{
    
    if (_model == nil) {
        
        NSString *contentUserID =@"";
        UIPasteboard *pasteboardUserID = [UIPasteboard pasteboardWithName:KNpasteboardWithNameKeyUserID create:NO];
        
        if (pasteboardUserID){
            contentUserID = pasteboardUserID.string;////获取内容
        }
        
        _model = [[NSObject alloc]init];
//        _model.UserId =contentUserID;
        
        
    }
    return _model;
}
```



### [LightMessaging](http://iphonedevwiki.net/index.php/LightMessaging)

>* feature

```
 1、Mid-level API,
 2、Message-oriented 
 3、Zero copy, for certain message types
 4、No additional cost over standard mach calls 
 5、Easy integration with RocketBootstrap 
 6、No package dependencies

```
>*  using

```
 1、Start services with LMStartService

 2、Send messages with LMConnectionSendTwoWay (and friends)

 3、Send replies with LMSendReply (and friends)

 4、Bring your own security checks still ：Community developers, your input on API please!


```



### [librocketbootstrap](http://iphonedevwiki.net/index.php/RocketBootstrap#How_to_use_this_library)

>* feature
```
1、Uses iOS7’s security model: Privileged processes can register, any process can look up 
2、Works with existing mach-based IPC mechanisms
3、Similar to Apple’s bootstrap APIs: bootstrap_look_up becomes rocketbootstrap_look_up
bootstrap_register becomes rocketbootstrap_register
```
![](/images/posts/{{page.title}}/bootstrap_look_up.png)

```
4、Easy to use wrappers for CFMessagePort and CPDistributedMessagingCenter(todo: XPC) 
5、Bring your own security model by using audit_token_to_au32 to know who’s calling
6、Requires package dependency, commonly installed on users’ devices
```

>* 本文重点讲解 RocketBootstrap的两种包装方式：CFMessagePort、CPDistributedMessagingCenter
```
Easy to use wrappers for CFMessagePort and CPDistributedMessagingCenter(todo: XPC) 
```

>* [CFMessagePort: Only supports synchronous use](https://github.com/zhangkn/RocketBootstrap/blob/ios7/tests/tests.m)

```
//registerMsgCenter 基本可以解决双向通信；例子：避免重启其他进程从sb获取源地址信息的变更。 守护进行都可以注册成为服务
+ (kern_return_t)rockettest_messageport_server
{
    static CFMessagePortRef messagePort;//
    if (messagePort)
        return 0;
    messagePort = CFMessagePortCreateLocal(kCFAllocatorDefault, CFSTR("rockettest_messageport"), messagePortCallback, NULL, NULL);//CFSTR("rockettest_messageport")即server key,
    CFRunLoopSourceRef source = CFMessagePortCreateRunLoopSource(kCFAllocatorDefault, messagePort, 0);
    CFRunLoopAddSource(CFRunLoopGetCurrent(), source, kCFRunLoopCommonModes);
    CFRunLoopAddSource(CFRunLoopGetCurrent(), source, (CFStringRef)UITrackingRunLoopMode);
    return rocketbootstrap_cfmessageportexposelocal(messagePort);
}
typedef CFDataRef (*CFMessagePortCallBack)(CFMessagePortRef local, SInt32 msgid, CFDataRef data, void *info);
//回调的定义
static CFDataRef messagePortCallback(CFMessagePortRef local, SInt32 msgid, CFDataRef data, void *info)
{
    NSLog(@"rockettest_messageport_server: received %@", data);
    return CFDataCreate(kCFAllocatorDefault, (const UInt8 *)"bootstrap", 9);
}
```
- [CFMessagePort_Example](http://iphonedevwiki.net/index.php/RocketBootstrap#CFMessagePort_Example)

>* [CPDistributedMessagingCenter: Asynchronous, if you go through the trouble](http://iphonedevwiki.net/index.php/RocketBootstrap#CPDistributedMessagingCenter_Example)

```
Private API, does change between iOS versions  ： 目前发现这种方式只能在SpringBoard   注册为服务端，处理消息，；进程间其实是单向通信;  http://iphonedevwiki.net/index.php/RocketBootstrap#LightMessaging_example
#TweakDemo.xm  SpringBoard  接受消息
#import "rocketbootstrap.h"

#define kXPCCenterNameKey  @"kXPCCenterNameKey_83641"

%hook SpringBoard
- (void)applicationDidFinishLaunching:(id)application {
   %orig;
   CPDistributedMessagingCenter *c = [%c(CPDistributedMessagingCenter) centerNamed:kXPCCenterNameKey];
   rocketbootstrap_distributedmessagingcenter_apply(c);
   [c runServerOnCurrentThread];
   [c registerForMessageName:@"myMessageName" target:self selector:@selector(handleMessage:withUserInfo:)];
   NSLog(@"注册监听 start");
}

%new
- (void)handleMessage:(NSString *)name withUserInfo:(NSDictionary *)userInfo {
   NSLog(@"handleMessage withUserInfo:%@",userInfo);
   //TODO:something
}

%end
//在需要发送的地方(沙盒 app )，如此这般的写： 
%hook SomeClass

-(void)someMethod{
   %orig;
    NSMutableDictionary *userInfo = [@{} mutableCopy];
    [userInfo setObject:@"123" forKey:@"arg001"];
    [userInfo setObject:@"456" forKey:@"arg002"];
    NSLog(@"发送: %@=%@",kXPCCenterNameKey,userInfo);
    CPDistributedMessagingCenter *c = [%c(CPDistributedMessagingCenter) centerNamed:kXPCCenterNameKey];
    rocketbootstrap_distributedmessagingcenter_apply(c);
    [c sendMessageName:@"myMessageName" userInfo:userInfo];
}
%end
```

 
>* 获取librocketbootstrap ：Install this from Cydia 直接搜索rocketbootstrap安装即可

```
iPhone:/var/log root# ls -l /usr/lib/librocketbootstrap.dylib
-rwxr-xr-x 1 root wheel 221776 Feb  6  2017 /usr/lib/librocketbootstrap.dylib*

/Library/LaunchDaemons/com.rpetrich.rocketbootstrapd.plist
/usr/libexec/rocketd
launchctl load  /Library/LaunchDaemons/com.rpetrich.rocketbootstrapd.plist
launchctl unload /Library/LaunchDaemons/com.rpetrich.rocketbootstrapd.plist
```



### [IPC](http://iphonedevwiki.net/index.php/IPC)
allows processes to send each other messages and data



### [libobjcipc](http://iphonedevwiki.net/index.php/Libobjcipc)

>* [feature](http://iphonedevwiki.net/index.php/Libobjcipc)
```
• High level API provides service lookup and IPC, easy to use
• Background-launches and fakes app lifecycle for you 
• Message-oriented
• Mostly asynchronous
• “Open” security model 
• Requires separate package dependency, but very small
• Simple Objective-C APIs: 
Register using --registerIncomingMessageFromAppHandlerForMessageName:handler:
Send using -- sendMessageToAppWithIdentifier:messageName:dictionary:replyHandler:  
• Similar patterns for App to SpringBoard
```

### [XPC](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html)

XPC can be accessed through either the libxpc C API, or the NSXPCConnection Objective-C API.

>* feature
```
1、High level API, easy to use ：One of Apple’s many wrappers for Mach messages；
2、Message-oriented
3、Public API on OS X only
4、Asynchronous always, no synchronous versions
5、Service lookup is restricted on iOS 7+ 
```
>* [参考文章](http://blog.linhere.com/archives/552.html)
>* [Inter-Process Communication](http://nshipster.com/inter-process-communication/)



### [Tweaks on a multi-process iOS](https://rpetri.ch/jbcon2016/JailbreakCon2016.pdf)

>* Inter-Process Communication 
```
1、Mechanisms provided by the kernel to facilitate coordinated sharing of data and commands between processes
2、Used heavily in recent versions of iOS and OS X to implement system frameworks and APIs
```
>* Standard Techniques
```
1、Save to temp files：High level APIs, easy to use
2、Unix Domain Sockets： Low level API  ，Stream oriented, requires basic parsing to reconstruct
messages
```
>* Other standard techniques
```
• Shared Memory
• Signals
• Named pipes
• Network sockets 
```
>* Apple/iOS-specific Techniques
```
1、Darwin Notifications：No data, only a simple “go” message，Any process can post or observe ，Always asynchronous
2、Mach Ports ：seriously low level 
3、CPDistributedNotificationCenter： Private API, does change between iOS versions
4、CFMessagePort： Public API，Only supports synchronous use ，Service lookup is restricted on iOS 6+
5、XPC ： High level API, easy to use �，One of Apple’s many wrappers for Mach messages ；
```
>* Creative Techniques
```
1、Relax existing service permissions
2、Repurpose existing iOS services’ IPC channels：Service internals are frequently rewritten in new iOS versions
3、Delegate to someone else
```

>* Libraries that use IPC under the hood
```
1、http://iphonedevwiki.net/index.php/AppList
2、http://iphonedevwiki.net/index.php/Flipswitch
3、http://iphonedevwiki.net/index.php/Libactivator
4、https://github.com/r-plus/libcanopenurl
```

>* Community Libraries to Help
```
1、RocketBootstrap： Service registration and lookup system for iOS
```

>* Be aware of potential deadlocks
```
1、 SpringBoard will block on backboardd—don’t call from backboardd to SpringBoard! 
2、Communicate with these processes using one-way IPC, asynchronous IPC, or two-way IPC with timeouts 
3、Avoid the pitfall of accidentally sending blocking API calls to one’s own process
4、SpringBoard is usually a good choice for coordinator as it often has much work to do anyway
5、Batch all of the operations for a single user action into one IPC call, if possible 
6、Filter to only the data required before sending 
```

### Q&A 


>* 使用yalu102 激活了之后，无法连接ssh

```
#/usr/local/bin/dropbear 此次越狱工具默认安装了 SSH。采用了 Dropbear 取代 Openssh。
因此在部署安装使用yalu102的时候记得修改dropbear.plist的信息：ProgramArguments的127.0.0.1:22 直接改为22。
如果部署完成，直接修改沙盒的信息的话，记得重启设备。

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
```

>* dropbear 参数

```
 iPhone:/var/containers/Bundle/Application/B831448D-BCD0-4F29-BDA6-9FC03903D30C/yalu102.app root# ps -e |grep dropbear
  228 ??         0:00.05 /usr/local/bin/dropbear -F -R -p 22
```

>* 利用wget 安装scp( 解决：sh: scp: command not found)

```
#两台服务器都要安装scp才能传文件
#wget + 空格 + 要下载文件的url路径
# cydia里面安装wget
#  安装scp,默认安装在当前目录
wget mila432.com/scp
ldid -S scp
# chmod +x scp
chmod 777 scp
mv scp /usr/bin/scp
# 或者使用curl: curl -O mila432.com/scp ./scp
```

dyld: Library not loaded: /usr/lib/libssl.0.9.8.dylib 重新安装openssl

>* 浏览器下载scp 

```
 find . -name "scp" 
 
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
devzkndeMacBook-Pro:.git devzkn$ knlog -i -f AppStore
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

### See also
- [Distributed Notifications](http://nshipster.com/inter-process-communication/)
- [Inter-process communication](http://iphonedevwiki.net/index.php/Updating_extensions_for_iOS_7#Inter-process_communication)
- [IPC](http://iphonedevwiki.net/index.php/IPC)
- [libobjcipc An inter-process communication (between app and SpringBoard) solution for jailbroken iOS](https://github.com/a1anyip/libobjcipc)
- [Libobjcipc iphonedevwiki](http://iphonedevwiki.net/index.php/Libobjcipc)
- [多个App之间的数据传输](https://github.com/lzyroot/MPProcessMessage)
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

### xpc资料
- [Creating XPC Services](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html)
- [关于 XPC](https://objccn.io/issue-14-4/)
- [XPC Services xpc.h](https://developer.apple.com/documentation/xpc/xpc_services_xpc.h?preferredLanguage=occ)
- [usr/include/xpc](https://github.com/zhangkn/mac-headers/tree/master/usr/include/xpc)
- [iOS冰与火之歌 – 利用XPC过App沙盒](http://cb.drops.wiki/drops/papers-14170.html)
```
1、最简单最常见的IPC就是URL Schemes
2、XPC也是iOS IPC的一种，通过XPC，app可以与一些系统服务进行通讯，并且这些系统服务一般都是在沙盒外的，如果我们可以通过IPC控制这些服务的话，也就成功的做到沙盒逃逸了
```
- [iOS_ICE_AND_FIRE](https://github.com/zhengmin1989/iOS_ICE_AND_FIRE)
- [iOS冰与火之歌系列，一步一步学ROP系列，安卓动态调试七种武器系列等](https://github.com/zhengmin1989/MyArticles)
- [rpetri.ch](https://rpetri.ch/)
- [Mattt Thompson](http://nshipster.com/authors/mattt-thompson/)
- [Mattt](https://mat.tt/)
- [打码兔](http://dama2.com/)
- [iOS冰与火之歌番外篇 - 在非越狱手机上进行App Hook](http://geek.csdn.net/news/detail/56195)
- [ios安全攻防](http://www.droidsec.cn/category/ios%E5%AE%89%E5%85%A8%E6%94%BB%E9%98%B2/)
- [深夜福利！越狱iOS清痕暨■■■■■■■■核心代码の无料放送-clearTrace](http://bbs.iosre.com/t/ios-igrimace/448)

```
- (void)clearTrace
{
        // Kill all App Store Apps
        NSFileManager *fileManager = [NSFileManager defaultManager];
        if ([fileManager fileExistsAtPath:@"/var/mobile/Library/Caches/com.apple.mobile.installation.plist"])
        {
                NSDictionary *dictionary1 = [NSDictionary dictionaryWithContentsOfFile:@"/var/mobile/Library/Caches/com.apple.mobile.installation.plist"];
                NSDictionary *dictionary2 = [dictionary1 objectForKey:@"User"];
                NSEnumerator *enumerator = [dictionary2 keyEnumerator];
                NSString *key;
                while ((key = [enumerator nextObject]))
                {
                        NSDictionary *dictionary3 = [dictionary2 objectForKey:key];
                        NSString *targetApp = [dictionary3 objectForKey:@"CFBundleExecutable"];
                        system([NSString stringWithFormat:@"killall -9 %@", targetApp] UTF8String]);
                }
        }

        // Kill Safari
        system("killall -9 MobileSafari");

        // Enumerate all App Store Apps' directories and delete /Documents, /Library, /tmp, /StoreKit
        NSDirectoryEnumerator *dirEnum = [fileManager enumeratorAtPath:@"/var/mobile/Applications/"];
        NSString *file;
        while ((file = [dirEnum nextObject]))
                if ([file hasSuffix:@"/Documents"] || [file hasSuffix:@"/Library"] || [file hasSuffix:@"/tmp"] || [file hasSuffix:@"/StoreKit"])
                {
                        [fileManager removeItemAtPath:[NSString stringWithFormat:@"/var/mobile/Applications/%@", file] error:nil];
                        [fileManager createDirectoryAtPath:[NSString stringWithFormat:@"/var/mobile/Applications/%@", file] withIntermediateDirectories:NO attributes:[NSDictionary dictionaryWithObjectsAndKeys:@"mobile", NSFileOwnerAccountName, @"mobile", NSFileGroupOwnerAccountName, nil] error:nil];
                }

        // Delete cookies
        [fileManager removeItemAtPath:@"/var/mobile/Library/Cookies/Cookies.binarycookies" error:nil];
        [fileManager removeItemAtPath:@"/private/var/root/Library/Cookies/Cookies.binarycookies" error:nil];
        [fileManager removeItemAtPath:@"/var/mobile/Library/Safari/History.plist" error:nil];
        [fileManager removeItemAtPath:@"/var/mobile/Library/Safari/SuspendState.plist" error:nil];

        // Delete pasteboard contents and relaunch pasteboardd
        system("launchctl unload -w /System/Library/LaunchDaemons/com.apple.UIKit.pasteboardd.plist");

        dirEnum = [fileManager enumeratorAtPath:@"/var/mobile/Library/Caches/com.apple.UIKit.pboard/"];
        while ((file = [dirEnum nextObject]))
                [fileManager removeItemAtPath:[NSString stringWithFormat:@"/var/mobile/Library/Caches/com.apple.UIKit.pboard/%@", file] error:nil];

        system("launchctl load -w /System/Library/LaunchDaemons/com.apple.UIKit.pasteboardd.plist");

        // Delete entries in keychain-2.db
        sqlite3 *database;
        int openResult = sqlite3_open("/var/Keychains/keychain-2.db", &database);
        if (openResult == SQLITE_OK)
        {
                int execResult = sqlite3_exec(database, "DELETE FROM genp WHERE agrp<>'apple'", NULL, NULL, NULL);
                if (execResult != SQLITE_OK) NSLog(@"iOSRE: Failed to exec DELETE FROM genp WHERE agrp<>'apple', error %d", execResult);

                execResult = sqlite3_exec(database, "DELETE FROM cert WHERE agrp<>'lockdown-identities'", NULL, NULL, NULL);
                if (execResult != SQLITE_OK) NSLog(@"iOSRE: Failed to exec DELETE FROM cert WHERE agrp<>'lockdown-identities', error %d", execResult);

                execResult = sqlite3_exec(database, "DELETE FROM keys WHERE agrp<>'lockdown-identities'", NULL, NULL, NULL);
                if (execResult != SQLITE_OK) NSLog(@"iOSRE: Failed to exec DELETE FROM keys WHERE agrp<>'lockdown-identities'', error %d", execResult);

                execResult = sqlite3_exec(database, "DELETE FROM inet", NULL, NULL, NULL);
                if (execResult != SQLITE_OK) NSLog(@"iOSRE: Failed to exec DELETE FROM inet, error %d", execResult);

                execResult = sqlite3_exec(database, "DELETE FROM sqlite_sequence", NULL, NULL, NULL);
                if (execResult != SQLITE_OK) NSLog(@"iOSRE: Failed to exec DELETE FROM sqlite_sequence, error %d", execResult);

                sqlite3_close(database);
        }
        else NSLog(@"iOSRE: Failed to open /var/Keychains/keychain-2.db, error %d", openResult);
}
```
- [LSApplicationWorkspace](https://github.com/nst/iOS-Runtime-Headers/blob/master/Frameworks/MobileCoreServices.framework/LSApplicationWorkspace.h)
- [iOS10-Runtime-Headers/PrivateFrameworks/AppStoreDaemon.framework/ASDJobManager.h ASDJobManager](https://github.com/JaviSoto/iOS10-Runtime-Headers/blob/master/PrivateFrameworks/AppStoreDaemon.framework/ASDJobManager.h)
- [iOS Symbol Pool](https://ios.ddf.net/)
- [mengwuji](http://www.mengwuji.net/thread-2950-1-1.html)
- [Apple ID 网页协议注册机源代码 163邮箱](https://bbs.pediy.com/thread-223622.htm)
- [qimai](https://www.qimai.cn/search/index/country/cn/search/%E7%9C%81%E9%92%B1)


