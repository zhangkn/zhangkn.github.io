---
layout: post
title: codeSnippets
date: 2018-01-21
tag: OC
site: https://zhangkn.github.io
---


### 前言



### 正文


### [dispatch信号量（dispatch semaphore）和 dispatch_sync(dispatch_get_main_queue() ----威力强大，建议使用递归，在block中触发退出条件](https://github.com/zhangkn/KNcodeSnippets/blob/master/KNcodeSnippets/KNdispatch_semaphore_t.m)

>* 使用dispatch信号量是如何实现同步

```
   /**
     二、dispatch信号量（dispatch semaphore）和 dispatch_sync(dispatch_get_main_queue()   ----威力强大，建议使用递归，在block中触发退出条件
     1. 创建信号量，可以设置信号量的资源数。0表示没有资源，调用dispatch_semaphore_wait会立即等待。
     
     dispatch_semaphore_t semaphore = dispatch_semaphore_create(0);
     2. 等待信号，可以设置超时参数。该函数返回0表示得到通知，非0表示超时。
     
     dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER);
     
     3. 通知信号，如果等待线程被唤醒则返回非0，否则返回0。
     
     dispatch_semaphore_signal(semaphore);
     
     
     一、 使用dispatch信号量是如何实现同步：
     
     尤其用于将异步的block 变成同步中
     */
    
+ (void)run{
    
    while (@"condition") {
        
        dispatch_semaphore_t  semaphore = dispatch_semaphore_create(0);
        dispatch_semaphore_wait(semaphore,dispatch_time(DISPATCH_TIME_NOW, (int64_t)( 16 * NSEC_PER_SEC)));//保证是同步的
        
        
        [self setupSwitchIp:^(NSString *errorMsg, id result) {
            
            dispatch_semaphore_signal(semaphore);//激活信号
            
        }];
        
    }
    
}
```

>* 使用递归，在block中触发退出条件（最多尝试切换IP动作 _KNwannaTryTimes次）

```
    int _KNtryTimes;
    int  _KNwannaTryTimes;//递归退出条件，即调用自己几次
+ (void)setupRecursion{
    
    // 调用递归方法
    [self runOnce];
}
    
    + (void) runOnce{//多次执行runOnce，采用递归的方式
        _KNtryTimes++;
      
        [self setupproce];
    }
    + (void)setupproce{
        
        [self setupSwitchIp:^(NSString *errorMsg, id result) {
            if (errorMsg == nil && result != nil) {
                
                // 退出递归结束
                
            }else{
                
                if (_KNwannaTryTimes > 0 && _KNtryTimes >= _KNwannaTryTimes){//递归结束
                    
                    return;
                    
                }else{
                    
                    [self runOnce];//递归
                    
                }
                
            }
            
        }];
    }
```


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
- [iOS多线程的初步研究（十）-- dispatch同步  ](http://www.cnblogs.com/sunfrog/p/3313424.html)
- [ 打码图像识别tensorflow](https://github.com/tensorflow)
- [学习资料资源入口](http://iosre.com/t/topic/4680)
- [C0F3 is a Jailbreak for 10.0 - 10.3.3 & 11.0 - 11.1.2](https://github.com/zhangkn/C0F3.git)
- [iOS内核调试教程](http://jaq.alibaba.com/community/art/show?spm=a313e.7916642.220000NaN1.3.50a8eb88EUySUE&articleid=1320)
- [https://open.alipay.com](https://open.alipay.com)
- [https://docs.alipay.com/mini/ide](https://docs.alipay.com/mini/ide)
- [Victor Zhang](http://www.googleplus.party/)
