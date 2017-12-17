---
layout: post
title: CustomHTTPProtocol
date: 2017-12-16
tag: iOSre
site: https://zhangkn.github.io
---

### 前言
本文主要是对官网提供的[CustomHTTPProtocol](https://developer.apple.com/library/content/samplecode/CustomHTTPProtocol/CustomHTTPProtocol.zip) 进行分析它是如何实现 intercept the HTTP/HTTPS requests。
```
CustomHTTPProtocol shows how to use an NSURLProtocol subclass to intercept the HTTP/HTTPS requests made by a high-level subsystem that does not otherwise expose its network connections.  
```

### NSURLProtocol
 NSURLProtocol 是[URL Loading System的](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)的组成部分。

![](/images/posts/{{page.title}}/nsobject_hierarchy_2x.png)


###  参考资源

- [samplecode](https://developer.apple.com/library/content/samplecode/CustomHTTPProtocol/CustomHTTPProtocol.zip)
- [Document](https://developer.apple.com/library/content/samplecode/CustomHTTPProtocol/Introduction/Intro.html#//apple_ref/doc/uid/DTS40013653-Intro-DontLinkElementID_2)
- [URL Loading System](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)
- [iOS 开发中使用 NSURLProtocol 拦截 HTTP 请求](https://draveness.me/intercept)

