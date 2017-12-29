---
layout: post
title: CycriptTricks
date: 2017-12-29
tag: iOSre
site: https://zhangkn.github.io
---



### 前言


### 

>* List all subclasses
```
cy# [c for each (c in ObjectiveC.classes) if (class_getSuperclass(c) && [c isSubclassOfClass:UIViewController])]
[c for each (c in ObjectiveC.classes) if (class_getSuperclass(c) && [c isSubclassOfClass:UIView])]
```

### 参考
- [CycriptTricks](http://iphonedevwiki.net/index.php/Cycript_Tricks)

