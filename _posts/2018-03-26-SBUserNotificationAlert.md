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

