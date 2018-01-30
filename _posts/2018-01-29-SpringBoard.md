---
layout: post
title: SpringBoard
date: 2018-01-29
tag: iOSre
site: https://zhangkn.github.io
---

### 前言




### 正文

>*  hook SpringBoard

```
%hook SpringBoard
-(void)applicationDidFinishLaunching: (id)application
{
    @autoreleasepool {
        %orig;

    }
}
%end
```


### see also
- [liberios jailbreakiOS11.1.1Liberios](https://mrmad.com.tw/liberios
- [可以越狱成功的ipa](http://pangu8.com/jailbreak/11/Liberios.ipa)
- [Electra_JB](https://www.reddit.com/r/Electra_JB/)
- [以后关注这个就知道最新版的cydia有没有出来](https://twitter.com/saurik/status/952725123601084416)
- [100行Python代码自动抢火车票！](https://github.com/zhangkn/tickets)
- [RevealLoader](https://github.com/heardrwt/RevealLoader)
```
Depends: mobilesubstrate, preferenceloader, applist
```
- [动态加载 FLEX 的越狱插件 - FLEXLoader](http://www.joeyio.com/2014/08/12/tweak-flexloader/)
- [iphonedevwiki-PreferenceLoader](http://iphonedevwiki.net/index.php/PreferenceLoader)
- [PreferenceLoader](http://www.swiftyper.com/2017/06/04/inspect-third-party-app-using-flexloader/)
```
1、PreferenceLoader 是一个 MobileSubstrate 提供的工具，它可以让开发者在系统设置界面添加应用程序入口。
2、在 iOS 设备的 /Library/PreferenceLoader/Preferences 下放入一个 plist 和三个图标文件（对应不同分辨率）。其中，plist 文件用来指定设置界面的展示内容，而图标文件则是用于在系统设置的入口处显示。
3、参考 RevealLoader 修改 plist 文件内容.(https://github.com/zhangkn/RevealLoader/tree/master/layout/Library/PreferenceLoader/Preferences)比较重要的是两个字段是 ALSettingsPath 和 ALSettingsKeyPrefix
```
- [AppList 项目地址](https://github.com/rpetrich/AppList)
- [PreferenceLoader 使用例子](https://github.com/zhangkn/RevealLoader/tree/master/layout/Library/PreferenceLoader/Preferences)
- [http://clang.llvm.org/docs/](http://clang.llvm.org/docs/)
- [http://rheard.com/blog/](http://rheard.com/blog/)