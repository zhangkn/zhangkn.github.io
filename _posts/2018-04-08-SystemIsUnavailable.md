---
layout: post
title: SystemIsUnavailable
date: 2018-04-08
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

用peopen替换system

### 正文

>* : 'system' is unavailable: not available on iOS

```

使用FILE	*popen(const char *, const char *) __DARWIN_ALIAS_STARTING(__MAC_10_6, __IPHONE_2_0, __DARWIN_ALIAS(popen)) __swift_unavailable_on("Use posix_spawn APIs or NSTask instead.", "Process spawning is unavailable.");

替代 int	 system(const char *) __DARWIN_ALIAS_C(system);


```

>*  [例子： yalu102 使用Xcode9 进行编译](https://github.com/iosjb/KNyalu102/blob/master/yalu102/jailbreak.m)

```
//               system("killall -9 cfprefsd");
                
                popen("killall -9 cfprefsd","r");

```

### see also
