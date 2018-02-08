---
layout: post
title: CustomHTTPProtocol
date: 2017-12-16
tag: iOSre
site: https://zhangkn.github.io
---

### 前言
在每一个 HTTP 请求开始时，URL 加载系统创建一个合适的 NSURLProtocol 对象处理对应的 URL 请求;
而我们需要做的就是写一个继承自 NSURLProtocol 的类，并通过 - registerClass: 方法注册我们的协议类;
然后 URL 加载系统就会在请求发出时使用我们创建的协议对象对该请求进行处理。
这样，我们需要解决的核心问题就变成了如何使用 NSURLProtocol 来处理所有的网络请求，

本文主要是对官网提供的[CustomHTTPProtocol](https://developer.apple.com/library/content/samplecode/CustomHTTPProtocol/CustomHTTPProtocol.zip) 进行分析它是如何实现 intercept the HTTP/HTTPS requests。
```
CustomHTTPProtocol shows how to use an NSURLProtocol subclass to intercept the HTTP/HTTPS requests made by a high-level subsystem that does not otherwise expose its network connections.  
```

目的是让我们以后更快速利用NSURLProtocol解决以下几个问题

- 如何决定哪些请求需要当前协议对象处理？
```
<tt>+canInitWithRequest:</tt>
```
- 对当前的请求对象需要进行哪些处理？

```
+ (NSURLRequest *)canonicalRequestForRequest:(NSURLRequest *)request;
```
- NSURLProtocol 如何实例化？

```
- (instancetype)initWithRequest:(NSURLRequest *)request cachedResponse:(nullable NSCachedURLResponse *)cachedResponse client:(nullable id <NSURLProtocolClient>)client NS_DESIGNATED_INITIALIZER;
```

### NSURLProtocol
 NSURLProtocol 是[URL Loading System的](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)的组成部分。

![](/images/posts/{{page.title}}/nsobject_hierarchy_2x.png)


```
/*! 
    @method registerClass:
    @abstract This method registers a protocol class, making it visible
    to several other NSURLProtocol class methods.
    @discussion When the URL loading system begins to load a request,
    each protocol class that has been registered is consulted in turn to
    see if it can be initialized with a given request. The first
    protocol handler class to provide a YES answer to
    <tt>+canInitWithRequest:</tt> "wins" and that protocol
    implementation is used to perform the URL load. There is no
    guarantee that all registered protocol classes will be consulted.
    Hence, it should be noted that registering a class places it first
    on the list of classes that will be consulted in calls to
    <tt>+canInitWithRequest:</tt>, moving it in front of all classes
    that had been registered previously.
    <p>A similar design governs the process to create the canonical form
    of a request with the <tt>+canonicalRequestForRequest:</tt> class
    method.
    @param protocolClass the class to register.
    @result YES if the protocol was registered successfully, NO if not.
    The only way that failure can occur is if the given class is not a
    subclass of NSURLProtocol.
*/
+ (BOOL)registerClass:(Class)protocolClass;
```



### CustomHTTPProtocol


#### CustomHTTPProtocol.h
```

@protocol CustomHTTPProtocolDelegate;

/*! An NSURLProtocol subclass that overrides the built-in HTTP/HTTPS protocol to intercept 
 *  authentication challenges for subsystems, ilke UIWebView, that don't otherwise allow it.  
 *  To use this class you should set up your delegate (+setDelegate:) and then call +start. 
 *  If you don't call +start the class is completely benign.
 *
 *  The really tricky stuff here is related to the authentication challenge delegate 
 *  callbacks; see the docs for CustomHTTPProtocolDelegate for the details.
 */

@interface CustomHTTPProtocol : NSURLProtocol

/*! Call this to start the module.  Prior to this the module is just dormant, and 
 *  all HTTP requests proceed as normal.  After this all HTTP and HTTPS requests 
 *  go through this module.
 */

+ (void)start;
```
#### CustomHTTPProtocol.m

```
+ (void)start
{
    [NSURLProtocol registerClass:self];
}
```
>* 修改NSURLRequest 对象，在CanonicalRequestForRequest 方法中，可以修改 request headers。

```
+ (NSURLRequest *)canonicalRequestForRequest:(NSURLRequest *)request
{
    NSURLRequest *      result;
    
    assert(request != nil);
    // can be called on any thread
    
    // Canonicalising a request is quite complex, so all the heavy lifting has 
    // been shuffled off to a separate module.
    
    result = CanonicalRequestForRequest(request);

    [self customHTTPProtocol:nil logWithFormat:@"canonicalized %@ to %@", [request URL], [result URL]];
    
    return result;
}
```

### AppDelegate

```
- (BOOL)application:(UIApplication *)application willFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    #pragma unused(application)
    #pragma unused(launchOptions)
    WebViewController *   webViewController;
    
    assert(self.window != nil);
    
    sAppStartTime = [NSDate timeIntervalSinceReferenceDate];
    
    self.credentialsManager = [[CredentialsManager alloc] init];

    // Prepare the globals needed by our logging code.  The call to -threadInfoForCurrentThread 
    // sets up the main thread's thread info record and ensures it has a thread number of 0.

    self.threadInfoByThreadID = [[NSMutableDictionary alloc] init];
    (void) [self threadInfoForCurrentThread];
    
    // Start up the core code.  Change the if expression to NO to disable the CustomHTTPProtocol for 
    // comparative testing and so on.
    
    [CustomHTTPProtocol setDelegate:self];
    if (YES) {
        [CustomHTTPProtocol start];
    }
    
    // Create the web view controller and set up the UI.  We do this after setting 
    // up the core code in case this triggers any HTTP requests.
    // 
    // By default the Test button is not shown because this sample is focused on UIWebView.  
    // If you want to runs tests with NSURL{Session,Connection}, change the if expression to 
    // show the Test button and then configure the test by changing the code in -testAction:.
    
    webViewController = [[UIStoryboard storyboardWithName:@"Main" bundle:[NSBundle bundleForClass:[self class]]] instantiateViewControllerWithIdentifier:@"webView"];
    assert(webViewController != nil);
    webViewController.delegate = self;
    if (NO) {
        webViewController.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:@"Test" style:UIBarButtonItemStyleBordered target:self action:@selector(testAction:)];
    }
    [((UINavigationController *) self.window.rootViewController) pushViewController:webViewController animated:NO];

	[self.window makeKeyAndVisible];
    
    return YES;
}
```


### CanonicalRequest

#### CanonicalRequest.h
```
extern NSMutableURLRequest * CanonicalRequestForRequest(NSURLRequest *request)
```
#### CanonicalRequest.m

```
extern NSMutableURLRequest * CanonicalRequestForRequest(NSURLRequest *request)
```
```
static void CanonicaliseHeaders(NSMutableURLRequest * request)
```


### 动手实践

[写一个tweak ，修改请求的HTTPHeaderField](https://github.com/zhangkn/KNCustomHTTPProtocol)。


### Q&A

>* createEncodedCachedResponseAndRequestForXPCTransmission
```
 ERROR: createEncodedCachedResponseAndRequestForXPCTransmission - Invalid protocol-property list - CFURLRequestRef. protoProps=<CFBasicHash 0x14526560 [0x39487460]>{type = mutable dict, count = 3,
    entries =>
        0 : <CFString 0xa8f968 [0x39487460]>{contents = "networkProtocolCount"} = <CFString 0xa81038 [0x39487460]>{contents = "1"}
        1 : <CFString 0xa8e4c8 [0x39487460]>{contents = "network-send-type"} = <CFString 0xa91228 [0x39487460]>{contents = "spdy"}
        2 : <CFString 0xa8f258 [0x39487460]>{contents = "connection_register"} = <TBSDKRequestDelegate: 0x1452d710>
    }
```

>* NSURLProtocol
```
%hook NSURLProtocol


+ (void)setProperty:(id)value 
forKey:(NSString *)key 
inRequest:(NSMutableURLRequest *)request{
    
    %log();
    %orig;
}

%end
```



### 注意

>* 1、NSURLProtocol 只能拦截 UIURLConnection、NSURLSession 和 UIWebView 中的请求;
对于 WKWebView 中发出的网络请求也无能为力，如果真的要拦截来自 WKWebView 中的请求，还是需要实现 WKWebView 对应的 WKNavigationDelegate，并在代理方法中获取请求。

>* 2、[OHHTTPStubs](https://github.com/zhangkn/OHHTTPStubs) 就是基于NSURLProtocol基础上实现的

```
提供不需要后端 API 的情况下，在本地对 HTTP 请求进行拦截，返回想要的 json 数据 ，常常运用于HTTP Mock 中。
- OHHTTPStubsProtocol 拦截 HTTP 请求
- OHHTTPStubs 单例管理 OHHTTPStubsDescriptor 实例
- OHHTTPStubsResponse 伪造 HTTP 响应
```

###  参考资源

- [samplecode](https://developer.apple.com/library/content/samplecode/CustomHTTPProtocol/CustomHTTPProtocol.zip)
- [Document](https://developer.apple.com/library/content/samplecode/CustomHTTPProtocol/Introduction/Intro.html#//apple_ref/doc/uid/DTS40013653-Intro-DontLinkElementID_2)
- [URL Loading System](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)
- [iOS 开发中使用 NSURLProtocol 拦截 HTTP 请求](https://draveness.me/intercept)
- [iOS检测系统弹窗并自动关闭](https://www.jianshu.com/p/c79e795c3f5b)
