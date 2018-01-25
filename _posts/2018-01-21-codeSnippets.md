---
layout: post
title: codeSnippets
date: 2018-01-21
tag: OC
site: https://zhangkn.github.io
---


### 前言



### codeSnippets


>* [从html页面获取URL的正则表达式](https://github.com/zhangkn/KNcodeSnippets/blob/master/KNcodeSnippets/KNRegex.m)

```
UIWebView *webView =  [self webView];
NSString *lJs = @"document.documentElement.innerHTML";//获取当前网页的html
[webView evaluateJavaScriptFromString:lJs completionBlock:^(NSString *currentHTML, NSError *err){

  NSString *parten = @"[a-zA-z]+://mp.weixin.qq.com/s?.__biz=(.+)";//       	NSString *parten = @"<h(.+?)hrefs=\"(.+?)\">(.+?)</h4>";

  NSError* error = NULL;
  NSRegularExpression *regex = [NSRegularExpression regularExpressionWithPattern:parten options:nil error:&error];
  NSArray* array = [regex matchesInString:currentHTML options:NSMatchingCompleted range:NSMakeRange(0, [currentHTML length])];

}];//evaluateJavaScript stringByEvaluatingJavaScriptFromString
```




### see also
- [ 打码图像识别tensorflow](https://github.com/tensorflow)
- [学习资料资源入口](http://iosre.com/t/topic/4680)
- [C0F3 is a Jailbreak for 10.0 - 10.3.3 & 11.0 - 11.1.2](https://github.com/zhangkn/C0F3.git)
- [iOS内核调试教程](http://jaq.alibaba.com/community/art/show?spm=a313e.7916642.220000NaN1.3.50a8eb88EUySUE&articleid=1320)
- [https://open.alipay.com](https://open.alipay.com)
- [https://docs.alipay.com/mini/ide](https://docs.alipay.com/mini/ide)
- [Victor Zhang](http://www.googleplus.party/)
