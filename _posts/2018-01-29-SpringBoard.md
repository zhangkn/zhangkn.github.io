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
