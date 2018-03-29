---
layout: post
title: ConfiguringWebApplications
date: 2018-03-28
tag: ios
site: https://zhangkn.github.io
---


### 前言



### 正文



>* [Apple URL Scheme Reference.](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007899)

>* [例子](https://github.com/zhangkn/zhangkn.github.io/blob/master/_includes/addTohomeScreen.html)

```
<!-- IOS Safari提供了两个私有接口： apple-touch-icon 和 apple-touch-icon-precomposed。 -->

<link rel="apple-touch-icon-precomposed" href="apple-touch-icon-precomposed-iphone.png" />

1)通过 apple-touch-icon 添加的图标是会带IOS图标统一的高光效果

2) 通过 apple-touch-icon-precomposed 添加的图标则是设计原图，不带高光渐变效果的。


```


>* apple-mobile-web-app-capable

```

<meta name="apple-mobile-web-app-capable" content="yes">


通过只读属性window.navigator.standalone来确定网页是否以全屏模式显示


```


>* apple-mobile-web-app-status-bar-style  


```
<!-- 配合apple-mobile-web-app-capable 使用。 -->

1) 如果content设置为default，则状态栏正常显示。

2) 如果设置为blank，则状态栏会有一个黑色的背景。

3) 如果设置为blank-translucent，则状态栏显示为黑色半透明
```


>* apple-touch-icon： 图标是在 body.onload 时被渲染的

```
<!-- 没有指明 <sizes> 属性的大小，则默认值为57x57。 -->

<link rel="apple-touch-icon" sizes="72x72" href="touch-icon-ipad.png" />
<link rel="apple-touch-icon" sizes="114x114" href="touch-icon-iphone-retina.png" />
<link rel="apple-touch-icon" sizes="144x144" href="touch-icon-ipad-retina.png" />

<!-- 如果整个页面都没有指定任何的 apple-touch-icon 的图标的话，IOS则会自动去网站根目录寻找有 apple-touch-icon 和 apple-touch-icon-precomposed 前缀的图标文件。 -->

<!--图片格式  -->

1) HTTP REQUEST

次新的图片都需要请求服务器，从服务器下载

2)绝对路径、相对于当前页面的相对路径以及相对于网站根目录的路径 纯静态的图片

3) base64格式的图片


这是一个包含png文件头的长字符串，它可以是一张从静态图片转换的，也可以是从服务器返回的，还可以是canvas生成的

<img src="data:image/png;base64,(xxxxxx)" </img>

<!-- 更新桌面启动图标 -->
webapp里所有的js逻辑都只有在页面打开状态下才能运行，所以动态修改桌面启动图标的方法只有在每次点开后才能生效

不适合比如天气、新闻、twitter等即时性很高、后台主动推送的场景

<!-- 选择canvas作为动态图片来源 -->

使用base64的优点是，可以选择由canvas来动态生成，并且不需要有网络请求，直接在本地完成。

var canvas = document.createElement('canvas');
canvas.width = 144;
canvas.height = 144;
var context = canvas.getContext("2d");
var baseImg = canvas.toDataURL();  


<!-- 更新桌面图标的方法都应该绑定在 body.onload  -->




<!-- 通过js动态创建和修改指定桌面图标的 <link> 标签 -->

0) 从safari里将页面添加主屏幕时，IOS会检查页面里的 <link> 标签，读取图片的地址然后生成启动图标


```

>* [Viewport Settings for Web Applications](https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/UsingtheViewport/UsingtheViewport.html#//apple_ref/doc/uid/TP40006509-SW19)


>* allintext:iPhone存储web app icon的路径?

```
/var/mobile/Library/Prefrences

:/var/preferences root# rm -rf *.networkextension*

find / -name "*" | xargs grep "iosre" > ./cqtest.txt

find . -name "*" | xargs grep "iosre" > ./cqtest.txt


devzkndeMacBook-Pro:com.wl..git devzkn$ scp usb2222:/private/var/mobile/Library/Logs/CrashReporter/SpringBoard-2018-03-23-153316.ips ~


```

### see also


- [ 解决：ios系统通过safari添加到主屏幕后，打开子链接还会跳转到safari](https://blog.csdn.net/lilinoscar/article/details/70270374)

```




```

- [iOS web app 桌面图标动态更新的解决方案。](https://github.com/hellometers/tick)

```

<!-- http://developer.apple.com/library/ios/#documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html -->

"Web Clip"

它的作用类似于桌面浏览器的书签，用户通过点击Icon能直接快速打开这个url的网站


<!-- IOS Safari提供了两个私有接口： apple-touch-icon 和 apple-touch-icon-precomposed。
 -->



1) 桌面图标在页面里的声明仅仅存在 <link> 中，理论上我们只要动态修改 <link> 标签的图片地址就能实现动态的图标。

<!-- JS创建的link -->

function addlink(){    
        $('head').append('<link rel="apple-touch-icon-precomposed" href="../img/demo.png" />');        
        $('.lay-page').append('<br>已创建link~ 添加到主屏幕检查');

}


<!-- webapp里的图标是在body.onload的时候被渲染的-->

行不通：通过js在一个canvas中绘出新的图案，再将这个canvas转化成base64的图片，动态创建到一个 <link> 标签添加至head 

<!-- BodyLoadSetIcon -->

        var img = new Image();
        var canvas = document.createElement('canvas');
        canvas.width = 144;
        canvas.height = 144;
        var context = canvas.getContext("2d");
        context.fillStyle="#ef4372";
        context.fillRect(0,0,144,144);
        context.textAlign="center";
        context.fillStyle = '#ffffff';
        context.textBaseline = "middle";
        context.font = "bold 90px HelveticaNeue";
        context.fillText(num,70, 70);
        var baseImg = canvas.toDataURL();            
        $('head').append('<link id="touchIcon" rel="apple-touch-icon-precomposed" href="' + baseImg + '" />');
        $("body").append('<img src="'+baseImg +'" />');




http://tmt.io/tick/js/tick.js?20131225

https://github.com/hellometers/tick




```

- [手机淘宝发现『添加店铺到桌面』的功能](http://mithvv.logdown.com/posts/275471/ios-safari-to-the-main-play-screen-feature-1)

```


<!-- 效果：Safari-添加到主屏幕功能 + App URL scheme 轻松实现。 -->

1） 在店铺页面有一个『添加本店铺到桌面的按钮』，点击后跳到 Safari 打开网页 A----data:text/html; 内嵌网页的方式，可能是为了离线

2) 在网页 A 提示用户点击工具栏分享按钮，再选择『添加到主屏幕』 --- 提示用户添加到桌面（注意指定网页 icon 的图片、标题）  ConfiguringWebApplications


3) 手机主界面出现该店铺的『网页快捷方式』--- safari 中打开网页是显示提示，点击主界面图标时如何控制跳转到应用,利用是否全屏显示standalone判断
window.navigator.standalone 参数，如果是 true （从主界面进入）则跳转到对应的 scheme, 如：taobao://shop.m.taobao.com/shop/shop_index.htm?user_id=37141631


4）点击『网页快捷方式』后，打开 Safari，短暂白屏后跳到手机淘宝应用自动打开此店铺 ---在应用内解析 scheme 作处理


<!-- 此效果可以说使用 https://github.com/zhangkn/AddIconToScreen、https://github.com/lijianfeigeek/iOS-desktop-->

JavaScript

Data URI Schema   --- 打开对应的店铺

Socket基本知识

Base64编码


<!-- 其它的可能性-->

1）查系统 scheme，快速进入设置 WIFI、VPN 界面，快速拨打电话

2）icon 与二维码结合使用：对于频繁需要在手机中让别人扫二维码的需要，可以直接扫主界面 icon（就是一个二维码）。这个能是最快展示二维码的方式，连应用都不用打开；


<!-- see also -->
1) https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html

2) iOS web app 桌面图标动态更新的解决方案。


```

- [webkit-webapp](http://www.cnblogs.com/pifoo/archive/2011/05/28/webkit-webapp.html)

```
<!-- 7. 判断是否为iPhone： -->

// 判断是否为 iPhone ： 
function isAppleMobile() { 
    return (navigator.platform.indexOf('iPad') != -1); 
}; 



```

- [ConfiguringWebApplications](https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)


- [add-to-home-screen](http://cubiq.org/add-to-home-screen?utm_source=caibaojian.com)

```
// set a web app capable website  Hiding Safari User Interface Components

<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">



```


- [AddIconToScreen](https://github.com/ldhlfzysys/AddIconToScreen/blob/master/AddIconToHome/Web/content.html)

```

<!-- Linking to Other Native Apps -->
<!-- <a href="tel:1-408-555-5555">Call me</a> -->



<body>
    <a href="addicon://" id="qbt" style="display:none"></a>
    <span id="msg"></span>
</body>


<!-- content -->


<html>
<head>
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    <meta content="text/html charset=UTF-8" http-equiv="Content-Type" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no" />
    <link rel='apple-touch-icon' href='data:image/jpeg;base64,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'>
    <title>我的应用</title>
</head>
<body>
    <a href="addicon://" id="qbt" style="display:none"></a>
    <span id="msg"></span>
</body>
<script>
    if (window.navigator.standalone == true)
    {
        var lnk = document.getElementById("qbt").click();
    }
    else
    {
        document.getElementById("msg").innerHTML='<div style="font-size:12px">这里可以添加引导页面</div>';
    }
</script>
</html>

```

- [https://github.com/lijianfeigeek/iOS-desktop](http://www.cocoachina.com/ios/20150827/13243.html)

