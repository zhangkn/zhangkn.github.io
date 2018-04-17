---
layout: post
title: launchApplicationWithIdentifier
date: 2018-04-17
tag: iOSre
site: https://zhangkn.github.io
---

###  前言


>* [code:自动解锁以及打开特定的app](https://github.com/jbtewaks/launchappandautounlock)


### 正文



>* launchApplicationWithIdentifier

```
NSTimer *timer ;

%hook SpringBoard
//applicationDidFinishLaunching
-(void)applicationDidFinishLaunching: (id)application
{
        %orig;
//创建一个定时器，定时检测   
	timer = [NSTimer scheduledTimerWithTimeInterval:60*2 target:self selector:@selector(checkHeart) userInfo:nil repeats:YES];
}

%new
- (void)checkHeart
{
	//定时检测微信是否开启
    [[UIApplication sharedApplication] launchApplicationWithIdentifier:@"com.kntencent.xin" suspended:0];
}

%end
```


### 与其他的tweak 结合起来使用，合并到同一个deb 包中


>* 放到layout对应的目录

```
 5 files changed, 3 insertions(+), 1 deletion(-)
 create mode 100755 Layout/Library/MobileSubstrate/DynamicLibraries/SBLockScreenViewController.dylib
 create mode 100644 Layout/Library/MobileSubstrate/DynamicLibraries/SBLockScreenViewController.plist
```

### see also