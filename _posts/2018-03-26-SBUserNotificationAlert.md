---
layout: post
title: SBUserNotificationAlert
date: 2018-03-26
tag: iOSre
site: https://zhangkn.github.io
---


###   前言


本文的重点是借助弹框信息，处理各个进程的错误。例如PPPD的MS-CHAP authentication failed: bad username or password


### 正文


>* [SBUserNotificationAlert](http://developer.limneos.net/?ios=11.1.2&framework=SpringBoard&header=SBUserNotificationAlert.h)

```

-(void)setAlertHeader:(NSString *)arg1 ;


```


### VPN 连接

>* pppd

```
MS-CHAP authentication failed: bad username or password

VPN Connection
```

### /System/Library/CoreServices/SpringBoard.app/SpringBoard




### see also

- [SBAlertItemsController](https://github.com/pNre/NoAnnoyance/blob/master/SpringBoard.xm)


```

%hook SBAlertItemsController

- (void) activateAlertItem:(id)alert {
    if ([alert isKindOfClass:[%c(SBLaunchAlertItem) class]]) {
        int _type = MSHookIvar<int>(alert, "_type");
        char _isDataAlert = MSHookIvar<char>(alert, "_isDataAlert");
        char _usesCellNetwork = MSHookIvar<char>(alert, "_usesCellNetwork");
        if (_type == 1) {
            BOOL cellPrompt = (_isDataAlert != 0 && _usesCellNetwork != 0);
            BOOL dataPrompt = (_isDataAlert != 0 && _usesCellNetwork != 1);
            if (cellPrompt || dataPrompt) {
                [self deactivateAlertItem:alert];
                return;
                
            }
            
        }
    }
    if ([alert isKindOfClass:[%c(SBUserNotificationAlert) class]]) {
        if ([[alert alertMessage] isEqual:CELLULAR_DATA_IS_TURNED_OFF_FOR_APP_NAME_string]) {
            [self deactivateAlertItem:alert];
            return;
        }
    }
    %orig;
}

%end


```


- [ 免费应用内测托管平台、iOS应用Beta测试分发、Android应用内测分发](https://fir.im/)


- [iOS检测系统弹窗并自动关闭](https://www.jianshu.com/p/c79e795c3f5b)

```

- (void)willActivate
{
NSLog(@"🐶 ++++++++++++ willActivate 准备 点击了++++++");
%orig;

dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{



id (*returnSheet)(id,SEL) = (id(*)(id,SEL))objc_msgSend;
id sheet = returnSheet(self,@selector(alertSheet));


void (*clickButton)(id,SEL,int,_Bool) = (void(*)(id,SEL,int,_Bool))objc_msgSend;

clickButton(sheet,@selector(dismissWithClickedButtonIndex:animated:),1,1);


void (*alertButton)(id,SEL,id,_Bool) = (void(*)(id,SEL,id,_Bool))objc_msgSend;
alertButton(self,@selector(alertView:clickedButtonAtIndex:),sheet,1);

void (*cancelSend)(id,SEL) = (void(*)(id,SEL))objc_msgSend;
cancelSend(self,@selector(cancel));

});

}
```

- [NS_ENUM 和 NS_OPTIONS的区别](https://blog.csdn.net/u013230511/article/details/41965393)

```
<!-- NS_ENUM 、CF_ENUM是一样的，NS_OPTIONS、CF_OPTIONS是一样的 -->

<!-- https://opensource.apple.com/source/CF/CF-744/CFNumberFormatter.h.auto.html -->

typedef CF_ENUM(CFIndex, CFNumberFormatterStyle) {	// number format styles
	kCFNumberFormatterNoStyle = 0,
	kCFNumberFormatterDecimalStyle = 1,
	kCFNumberFormatterCurrencyStyle = 2,
	kCFNumberFormatterPercentStyle = 3,
	kCFNumberFormatterScientificStyle = 4,
	kCFNumberFormatterSpellOutStyle = 5
};

<!-- typedef signed long CFIndex; -->

typedef CF_ENUM(CFIndex, CFNumberFormatterStyle) {	// number format styles
	kCFNumberFormatterNoStyle = 0,
	kCFNumberFormatterDecimalStyle = 1,
	kCFNumberFormatterCurrencyStyle = 2,
	kCFNumberFormatterPercentStyle = 3,
	kCFNumberFormatterScientificStyle = 4,
	kCFNumberFormatterSpellOutStyle = 5,
	kCFNumberFormatterOrdinalStyle API_AVAILABLE(macos(10.11), ios(9.0), watchos(2.0), tvos(9.0)) = 6,
	kCFNumberFormatterCurrencyISOCodeStyle API_AVAILABLE(macos(10.11), ios(9.0), watchos(2.0), tvos(9.0)) = 8,
	kCFNumberFormatterCurrencyPluralStyle API_AVAILABLE(macos(10.11), ios(9.0), watchos(2.0), tvos(9.0)) = 9,
	kCFNumberFormatterCurrencyAccountingStyle API_AVAILABLE(macos(10.11), ios(9.0), watchos(2.0), tvos(9.0)) = 10,
};

/*	CFNumberFormatter.h


```

- [html5 在手机桌面添加图标 有点像web app JavaScript提示用户添加到主屏幕](http://caibaojian.com/add-to-home-screen.html)


```




```

- [iOS App创建桌面快捷方式](http://www.cocoachina.com/ios/20150827/13243.html)

```

Safari有一个“添加至屏幕”的功能，其实就是在桌面上添加了一个网页书签，App可以使用这个功能来实现创建桌面快捷方式。


<!-- 一、运用基本技术点 -->

JavaScript

Data URI Schema

Socket基本知识

Base64编码

<!-- 二、基本原理 -->

程序内部创建一个简单的Web站点，通过这个站点调用Safari，站点将自定义的Html页面返回给Safari，此时利用Safari的“添加至主屏幕”功能，将自定义的Html制作成桌面书签，当用户点击桌面图标时，则运行自定义的Javascript来进行跳转至App。


<!-- 三、什么是 data URI scheme？节省了一个 HTTP 请求 -->

直接把图像的内容崁入网页里面，这个密技的官方名称是 data URI schema 

<!-- Data URI scheme 的语法 -->

data – 取得数据的协定名称

image/png – 数据类型名称

base64 – 数据的编码方法

iVBOR…. – 编码后的数据

: , ; – data URI scheme 指定的分隔符号



<!-- 四、什么是 Base64 编码？ -->

把一些 8-bit 数据翻译成标准 ASCII 字符


<!-- 五、Socket基本知识 -->

使用CocoaHttpServer创建一个本地的站点


https://github.com/robbiehanson/CocoaHTTPServer

 _httpServer = [[HTTPServer alloc] init];
    [_httpServer setType:@"_http._tcp."];
    NSString *webPath = [[[NSBundle mainBundle] resourcePath] stringByAppendingPathComponent:@"Web"];
//    DDLogInfo(@"Setting document root: %@", webPath);
    [_httpServer setDocumentRoot:webPath];
    [self startServer];

<!-- 创建HttpServer -->

- (void)startServer
{
    // Start the server (and check for problems)
    NSError *error;
    if([_httpServer start:&error])
    {
        DDLogInfo(@"Started HTTP Server on port %hu", [_httpServer listeningPort]);
        // open the url.
        NSString *urlStrWithPort = [NSString stringWithFormat:@"http://localhost:%d",[_httpServer listeningPort]];
        [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlStrWithPort]];
    }
    else
    {
        DDLogError(@"Error starting HTTP Server: %@", error);
    }
}




<!-- 六、实现 -->


<!--         [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlStrWithPort]]; -->


https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html





1) 检测Web应用程序当前是否运行在全屏状态，只要检测window.navigator.standalone是否为true就可以了

2) 检测到Web应用程序运行在非全屏状态时就可以提示用户把Web应用程序的图标添加到主屏幕。

3) http://software.hixie.ch/utilities/cgi/data/data



```

- [Configuring Web Applications](https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)

```
iOS App创建桌面快捷方式  https://github.com/lijianfeigeek/iOS-desktop
https://github.com/cubiq/add-to-homescreen/zipball/master

mobile-bookmark-bubble: https://code.google.com/archive/p/mobile-bookmark-bubble/


```

- [addtohomescreen](https://zhangkn.github.io/add-to-homescreen/src/addtohomescreen.js)

```
<!-- http://cubiq.org/add-to-home-screen?utm_source=caibaojian.com -->

// default options
ath.defaults = {
    appID: 'org.cubiq.addtohome',       // local storage name (no need to change)
    fontSize: 15,               // base font size, used to properly resize the popup based on viewport scale factor
    debug: false,               // override browser checks
    logging: false,             // log reasons for showing or not showing to js console; defaults to true when debug is true
    modal: false,               // prevent further actions until the message is closed
    mandatory: false,           // you can't proceed if you don't add the app to the homescreen
    autostart: true,            // show the message automatically
    skipFirstVisit: false,      // show only to returning visitors (ie: skip the first time you visit)
    startDelay: 1,              // display the message after that many seconds from page load
    lifespan: 15,               // life of the message in seconds
    displayPace: 1440,          // minutes before the message is shown again (0: display every time, default 24 hours)
    maxDisplayCount: 0,         // absolute maximum number of times the message will be shown to the user (0: no limit)
    icon: true,                 // add touch icon to the message
    message: '',                // the message can be customized
    validLocation: [],          // list of pages where the message will be shown (array of regexes)
    onInit: null,               // executed on instance creation
    onShow: null,               // executed when the message is shown
    onRemove: null,             // executed when the message is removed
    onAdd: null,                // when the application is launched the first time from the homescreen (guesstimate)
    onPrivate: null,            // executed if user is in private mode
    privateModeOverride: false, // show the message even in private mode (very rude)
    detectHomescreen: false     // try to detect if the site has been added to the homescreen (false | true | 'hash' | 'queryString' | 'smartURL')
};

```

