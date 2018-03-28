---
layout: post
title: SBUserNotificationAlert
date: 2018-03-26
tag: iOSre
site: https://zhangkn.github.io
---


###   前言


本文的重点是借助弹框信息，处理各个进程的错误。例如PPPD的MS-CHAP authentication failed: bad username or password


### 正文


>* [SBUserNotificationAlert](http://developer.limneos.net/?ios=11.1.2&framework=SpringBoard&header=SBUserNotificationAlert.h)

```

-(void)setAlertHeader:(NSString *)arg1 ;


```


### VPN 连接

>* pppd

```
MS-CHAP authentication failed: bad username or password

VPN Connection
```

### /System/Library/CoreServices/SpringBoard.app/SpringBoard




### see also

- [SBAlertItemsController](https://github.com/pNre/NoAnnoyance/blob/master/SpringBoard.xm)


```

%hook SBAlertItemsController

- (void) activateAlertItem:(id)alert {
    if ([alert isKindOfClass:[%c(SBLaunchAlertItem) class]]) {
        int _type = MSHookIvar<int>(alert, "_type");
        char _isDataAlert = MSHookIvar<char>(alert, "_isDataAlert");
        char _usesCellNetwork = MSHookIvar<char>(alert, "_usesCellNetwork");
        if (_type == 1) {
            BOOL cellPrompt = (_isDataAlert != 0 && _usesCellNetwork != 0);
            BOOL dataPrompt = (_isDataAlert != 0 && _usesCellNetwork != 1);
            if (cellPrompt || dataPrompt) {
                [self deactivateAlertItem:alert];
                return;
                
            }
            
        }
    }
    if ([alert isKindOfClass:[%c(SBUserNotificationAlert) class]]) {
        if ([[alert alertMessage] isEqual:CELLULAR_DATA_IS_TURNED_OFF_FOR_APP_NAME_string]) {
            [self deactivateAlertItem:alert];
            return;
        }
    }
    %orig;
}

%end


```


- [ 免费应用内测托管平台、iOS应用Beta测试分发、Android应用内测分发](https://fir.im/)


- [iOS检测系统弹窗并自动关闭](https://www.jianshu.com/p/c79e795c3f5b)

```

- (void)willActivate
{
NSLog(@"🐶 ++++++++++++ willActivate 准备 点击了++++++");
%orig;

dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{



id (*returnSheet)(id,SEL) = (id(*)(id,SEL))objc_msgSend;
id sheet = returnSheet(self,@selector(alertSheet));


void (*clickButton)(id,SEL,int,_Bool) = (void(*)(id,SEL,int,_Bool))objc_msgSend;

clickButton(sheet,@selector(dismissWithClickedButtonIndex:animated:),1,1);


void (*alertButton)(id,SEL,id,_Bool) = (void(*)(id,SEL,id,_Bool))objc_msgSend;
alertButton(self,@selector(alertView:clickedButtonAtIndex:),sheet,1);

void (*cancelSend)(id,SEL) = (void(*)(id,SEL))objc_msgSend;
cancelSend(self,@selector(cancel));

});

}
```

- [NS_ENUM 和 NS_OPTIONS的区别](https://blog.csdn.net/u013230511/article/details/41965393)

```
<!-- NS_ENUM 、CF_ENUM是一样的，NS_OPTIONS、CF_OPTIONS是一样的 -->

<!-- https://opensource.apple.com/source/CF/CF-744/CFNumberFormatter.h.auto.html -->

typedef CF_ENUM(CFIndex, CFNumberFormatterStyle) {	// number format styles
	kCFNumberFormatterNoStyle = 0,
	kCFNumberFormatterDecimalStyle = 1,
	kCFNumberFormatterCurrencyStyle = 2,
	kCFNumberFormatterPercentStyle = 3,
	kCFNumberFormatterScientificStyle = 4,
	kCFNumberFormatterSpellOutStyle = 5
};

<!-- typedef signed long CFIndex; -->

typedef CF_ENUM(CFIndex, CFNumberFormatterStyle) {	// number format styles
	kCFNumberFormatterNoStyle = 0,
	kCFNumberFormatterDecimalStyle = 1,
	kCFNumberFormatterCurrencyStyle = 2,
	kCFNumberFormatterPercentStyle = 3,
	kCFNumberFormatterScientificStyle = 4,
	kCFNumberFormatterSpellOutStyle = 5,
	kCFNumberFormatterOrdinalStyle API_AVAILABLE(macos(10.11), ios(9.0), watchos(2.0), tvos(9.0)) = 6,
	kCFNumberFormatterCurrencyISOCodeStyle API_AVAILABLE(macos(10.11), ios(9.0), watchos(2.0), tvos(9.0)) = 8,
	kCFNumberFormatterCurrencyPluralStyle API_AVAILABLE(macos(10.11), ios(9.0), watchos(2.0), tvos(9.0)) = 9,
	kCFNumberFormatterCurrencyAccountingStyle API_AVAILABLE(macos(10.11), ios(9.0), watchos(2.0), tvos(9.0)) = 10,
};

/*	CFNumberFormatter.h


```

- [html5 在手机桌面添加图标 有点像web app JavaScript提示用户添加到主屏幕](http://caibaojian.com/add-to-home-screen.html)


```




```

- [iOS App创建桌面快捷方式](http://www.cocoachina.com/ios/20150827/13243.html)

```

Safari有一个“添加至屏幕”的功能，其实就是在桌面上添加了一个网页书签，App可以使用这个功能来实现创建桌面快捷方式。


<!-- 一、运用基本技术点 -->

JavaScript

Data URI Schema

Socket基本知识

Base64编码

<!-- 二、基本原理 -->

程序内部创建一个简单的Web站点，通过这个站点调用Safari，站点将自定义的Html页面返回给Safari，此时利用Safari的“添加至主屏幕”功能，将自定义的Html制作成桌面书签，当用户点击桌面图标时，则运行自定义的Javascript来进行跳转至App。


<!-- 三、什么是 data URI scheme？节省了一个 HTTP 请求 -->

直接把图像的内容崁入网页里面，这个密技的官方名称是 data URI schema 

<!-- Data URI scheme 的语法 -->

data – 取得数据的协定名称

image/png – 数据类型名称

base64 – 数据的编码方法

iVBOR…. – 编码后的数据

: , ; – data URI scheme 指定的分隔符号



<!-- 四、什么是 Base64 编码？ -->

把一些 8-bit 数据翻译成标准 ASCII 字符


<!-- 五、Socket基本知识 -->

使用CocoaHttpServer创建一个本地的站点


https://github.com/robbiehanson/CocoaHTTPServer

 _httpServer = [[HTTPServer alloc] init];
    [_httpServer setType:@"_http._tcp."];
    NSString *webPath = [[[NSBundle mainBundle] resourcePath] stringByAppendingPathComponent:@"Web"];
//    DDLogInfo(@"Setting document root: %@", webPath);
    [_httpServer setDocumentRoot:webPath];
    [self startServer];

<!-- 创建HttpServer -->

- (void)startServer
{
    // Start the server (and check for problems)
    NSError *error;
    if([_httpServer start:&error])
    {
        DDLogInfo(@"Started HTTP Server on port %hu", [_httpServer listeningPort]);
        // open the url.
        NSString *urlStrWithPort = [NSString stringWithFormat:@"http://localhost:%d",[_httpServer listeningPort]];
        [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlStrWithPort]];
    }
    else
    {
        DDLogError(@"Error starting HTTP Server: %@", error);
    }
}




<!-- 六、实现 -->


<!--         [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlStrWithPort]]; -->


https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html





1) 检测Web应用程序当前是否运行在全屏状态，只要检测window.navigator.standalone是否为true就可以了

2) 检测到Web应用程序运行在非全屏状态时就可以提示用户把Web应用程序的图标添加到主屏幕。

3) http://software.hixie.ch/utilities/cgi/data/data



```

- [Configuring Web Applications](https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)

```
iOS App创建桌面快捷方式  https://github.com/lijianfeigeek/iOS-desktop
https://github.com/cubiq/add-to-homescreen/zipball/master

mobile-bookmark-bubble: https://code.google.com/archive/p/mobile-bookmark-bubble/


```

- [iOS添加快捷方式到桌面#](https://www.jianshu.com/p/86a5e35ef2ab)

```
<!-- 如果非全屏，那么我们显示引导页，如果是全屏，我们就打开一个链接 -->

涉及：OpenUrl、iOS shceme、Data URI Scheme、JS、Socket#

<!-- 在Safari中，有一个功能叫：添加到主屏幕，而我们将使用这个入口去实现这个功能。 -->

<!-- [[UIApplication sharedApplication] openURL:[NSURLURLWithString:@"tel://xxx"]]; -->

<!-- JS -->

<script>
if (window.navigator.standalone == true)
{
    // 全屏状态的时候 模拟点击事件，即打开app---webKit---通过快捷方式打开的Safari是全屏的
    var lnk = document.getElementById("你的带scheme的<a>标签ID").click();
    //通过你所知道的方式打开一个scheme，上面这句话的链接标签如：<a href="tel://xxx">
}
else
{
    document.getElementById("msg").innerHTML='<div style="font-size:12px">
    可以插入引导页</div>';
    //这里你可以去加载你的引导页
}
</script>

<!-- 通过DataURI，我们可以把图片进行base64编码直接存储在页面中 -->

    <link rel='apple-touch-icon' href='data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAcFBQYFBAcGBgYIBwcICxILCwoKCxYPEA0SGhYbGhkWGRgcICgiHB4mHhgZIzAkJiorLS4tGyIyNTEsNSgsLSz/2wBDAQcICAsJCxULCxUsHRkdLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCz/wAARCAC0ALQDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDnUXgYxU6xmkTgD1q1GAeePxrzTvI0h4+tWI4znH609Yg54OKnjjycEg0hjEj7Zxip1jLHnnPanx24J+tXIogOcUxEUNsAwJyMVa2Z6gmlCgccYqUICOMcU7XYDQjbeADUirngnFUdV1SPSbUSNG8s8jCOGFOWlc9AKzvD7atB4nvLbVp1lkuLdLlI0+5DhipRfp8vNXGNyWzo1UE/dNSeXjnjHvVbUNUsNIgEl9dxQDsCck/QdT+FZn/CTTXrtFo+jXt5KFBLyxm3jX0yz/0Bp8oXN4JnqOPanhR04I96w9Bu9Zk1u/0/Vxaloo4pk+z5wgfcNpJ6/drphGcdM/Wny2EmQhMjgfjQY+hIz71OEyenNLsb1/OiwXIiq4A5xSeVxwas7AOoyaDGSOnH60nELlby+PUUnlnqRVkRhe1BQdCMUON9guVcZPrTTF9auGM9RnFNMTdf50lHQdyl5THpgD6UVe8k45AopezFzHmESMDxjFWEXj9cU+KIYAPH49anSE5AA6e9Q3c0CIZ6Hj61eij455pIbcdyM1bSLn/ClYBIkC59PWp1UgAilWPpwcVMqEduKpWEc54gbVrfVtMeyu44YJmaFxJHvQsRlc9xnBHB7ipIvEsdlcfZNdgOmzL0mOTBL/uvj9Dip/Et9pcenSWd7c+XNIoaNY1MkisDkMFHPBAP4VuW9tHqOkol7DHOs0Q8xHTAORzwen41qlpqQ32MXw/ZnVrv/hIbxDumXFnEx/1UXZv95uv0wKvav4fn1G9trmz1F9PljR4ndEDMyMQSBnocqOfrW1BaxwwJDDGsccYCqqjgAdBU6jBoTsxGNpXhPStLk8+G3EtyT81zMd8rH3Y/0rcEZ/OnKOB1GKkA56mn6iObvfD+qf29NqWk6nFbm5jSOVJrfzR8ucEYYY+8aPL8UWUJeafS75VG5iyPAQB+LCunHTpTjEGUqRkEUwON0Xx/oWo6ZFc3Wo2VjK+7MMlypZcEjnp1HP41tWGuaVqZP2HU7W6I6iORWP6Vd/sjTlXAsLUD/riv+Fct420rTwmkbLO3hml1KCPzUjCsF3ZOCOeQtO19hXsdeqA+g+tO2D60g3cYFOHPv9KQ7jSnPC0bAeo/WpACaXaewNAXIStIBUxGDjOTS4XpQBWKrnrRU+PY0UwPNokLKOM+9WooTj0/CkgiBHOefyq2ibeAMfSuRaGwsad+3061ajQDqB+FNRMjkGrUaFei8VVhCpHkensTipRFwP8AGlUDqRzVHXdVk0qzi+zw+ddXUq28CscLvbpk9gOtXGImznnt5vCuuGK1VNWfVJdxtmH+kKD1O/kFB/tYx613qx8YxWZoWhrpiyTyym81C45uLlxgsfQeijsK21UHvVvuQRBOADxT1UHG3r61IRtPXNPAyO+KadxDAh9c0oU9xT9uPU0oB4ycfWqQhMHHGMU7PHTFBA9M0nRuBQAqgY5qnq2jWGt2X2PULZLiDcH2tngjoRjvVvkngU4BiOgoEcu3hnUdOhxouu3ECr0gvB9oix6ZPzD86i/4S2bSbu3s/EWnvZyXMgihuIG82GVj2H8S/Qj8a6XUrr7Bpk92Lea58lC/lQjc7+wHrXOeF7dfELReKL6SOaeRStrAp3JaJnleR9/1P4UxNnUKOuPypckce9SBfSkZRnOcmkO4wHJ9BSEDP9alUcdMUNnGNvHrQFyIg+tFPwfWigZwcUY4zmrqRALyaZFHuUelXIk7AZrk6Gw2OMg4/WrKRkfWnLGDwCP5VYSId/0qkJsYnIxjOPesHxYV1GGLQba3W5vrv503EgW4B/1pI5GD0x1NbmpT/YNLuLwQySmGNnEaKSzYHQCuO8M69Pb6laxTaLevfazKWmuZk8pVAGdqhvmKoOPr9a1irohnbaPYy6fpkFrNdyXksa4aeX7zn1NX+M0oU45GaeuQc9MUxCKOeBSjGeQSaqanqljpNm13fXCQRKcbm6k+gHc+wrG0nxTc6l4hTT30iezhlt2uI5Z2AcqGC8oORknue1WkTc6YHA5GQKbnP0qhfa7pWl5N/qNrbEc4llCn8qhtvFOh3mnPfw6pbG1jYo0pfaoYDJGT9aAua21cctShR61yV38QdPiRZLWz1C/jeRYlkitysbMxwAGbAPJ7Zrq1finYGSBRj1pyrjpx+dIDke9LwOvWkTcD15NcjIR4V8Y+dwNJ1pwHPaC6xgH2DgY+o96675cf41wmsaRJr3jeXSNZ1O5WwkgW5s7e3cRq21hvDHGSQcEfX2qlqDZ3QIPfj2o2+1AwBgUA+2aQBRgelHPpRtz3oAMDuDRRjHc0UhnJQxnAOKtIrdAOKZEPlHGD9KtRj16VymzY+NT+VWY1yMECmoMHgY96njUE8nmqS0JHInAJHSuasLm1udf1DXLyeOO1tSbK1aRgqgLzIwz6tx/wGuoeDzomQkoHBG4HBH0rC0nwB4e0xUP2EXksfSW6YynPUnB4H4CtI26ibKLfEnw8t5JE8syQJEZftTR7YmGcDaTy2TwMDnFZviDxL4rks4J9Jso7CO6YrbJMvmzzYRnB29FBC9OTz2rspPC+jza0mryWMUl5HEIkd+QijPReg6nnFUvECAeIfDeAB/pr4H/bCStUkQZ+i6Ze6g0XiHxOqxzRpmC1bGy1XHLH/bPc9ulU761bVvFk9zp/ieygt5LZbaQQuGuFUEswQ5wM7hzyelVPiZZz6/KNLXxVpukWK/62KSXEkj9cMMjjBBx7/SvNV+FUe5y3jPQ1UMMETjlfXrwfaqST6ibPddH8MaBp6iSxsbdpO9wwEkjnvlzkk0228FeH7TUZr9NKhe6mkMrSSZkIYnJI3ZA/CofAeg2fh7wtFY2F+uow72czqwIZie2CQO1bt3MsEDDzY4pCp2GQgAtjikFzG8Q+GI9fmsXN9dWf2JzIv2cqCzEYzyDggZxj1rH8QaS/hzw7canY6nqomtdrjzLtpVPzDO5WyMYzXmsen/Fm8vJJlvLyDfuf95cqifQDPHsK7DwP4M8WfbbqTxXqjXVjdWhgNs1yZC27rnsCBnn3qrW6iuej2d7BeQ+ZbzJOgJUsjBhkcEce9WAffNcnp/gX/hHLNP8AhHbySC4TJdZyXiueSfnHY843Lz9ak03xvaTa6dC1K3k03WFAIgdg6yZz9xh16d8GoA6O8na2sZrhIzKYkZwgOC2BnFcbZf294tv9C16OztLCzgbzlY3BklkidMFcBcDOR1PUVt6p4njstTTSrSxuNS1J1DG3hAAjQ/xOzcKP507wTp17pXhWCzvoVt5Ink2xhw+1C7FRkegIFML3N3BHfFJk4p5+lNIJPtSAT60fWnDPfFIaAG5x3opdoPb9aKQHOQrkD5hircMf+1+VVoOVH+FXY1OK5UbslUetWEU49qijHTvU6Alsng1pHaxLJFH1/GplGPao1H51MoyOatEBnHauQ8QWkmueMtN0+1vJbV9Pie8kmhAJQsNiDkEc/Px6CuvOFU+lc74bIl1rxDcn/WfbhDn/AGUiTA/8eJ/GtEBgQb7Pwt4h1UWtve6lHezbnkiDY2sE3bRzgKu7FZfhrU7bW/GEdrY3/wDaywM5vXawijt9m3goQu7O7GOT0NdxqPgnQ9Sv5b6a3liu5cFpred4mJxjPykc1Ba/D3w5bFnNpJcsxyTcTPLk+uCcfpTuhMyLG4aw8W67b+HtPjuGC25mgMnkRrIQ+WB2nnaEzgVV0yDT73xJrkniu304X0bRbIppBJHHEYx9wuBxkHJx1rcv7qw8N6pp+m6bFZaf9pY3FyxVY1EKDBJ9ySoH41s3eh6VqzRzXtja3ZUfK0sSuQPYkUCPD4T4dvNWJsWFxqyzlXsGsle3mzJgIhA+XC/xfjXf3tvpfg/xfp89jaywwSWtw0sFqrvvIMYXCDjOW7DvXSy+B/DcoJGj2kbH+OGMRsP+BLg0aV4L0TQ9R+26dbyRTCNowDM7gBiCcBicdB0p3QGHB4+mj8UQabqel/2Xb3CEpJO5DI3VQ/G0Fh23EjHNUtD8KWfilp/FVxNdWt9eXLyW1zbylHSAfKgx0IIXPI70eOrm68Q3UHgxtNnha+uVJum2mNoUIdmXBznAx0HNegwQR2ttFBDGEijUIiKOFAHAo2Ec5o3hi/03xJcand6w1+s1sluA0IRvlYkFivBIyR0FdIQMcd6XORSZAFIBM/X8qaRzzmncZzn9KXB9RSsO43rwDRt9KXkemBQCO/6UBcYck9qKfhTRRoFznYScA5q5H0yarQrhRgfXNW4xiuVG5OmB9asqOlQJz0GT9KsL7itEiGPVVPWpAB7ikGOvrTweatLQVhrAGuSuZj4T8WPeTMo0fWXUSOeBb3AG0En+64AGfUe9df1PFRXVnb39pLbXcCTwSrteNxuUj3Bq0yR4dW6EHPelPXj9a5yLw9qOhkDQr0SWi9LK9Ysq+yScsv0OR9Kc/ikWBC61pt5px/56CPz4T/wNM4/ECgRqXunadqWwX1lbXWwnb50QfH0yKuqqKoCrtAGAB0Fee3b+BtRvpbqTxPJFJO29lXU3iU8j+HIA+ldOPGPhoKP+J9p2B/09J/jVWEbmar31/b6fZTXd3OkEEK7ndjgAVgS+OtJm/d6X9o1e4PAjsoi/5twoHuTSWvh+/wBau4tQ8SuCsT+ZBpsTZiiI6Fz/AMtG/QdvWglITw3bT6prFz4nvIXgE8fkWUMi7WjgBzuI7M55x2AFdSSMcc0HA/wpCfQUMaY0mmHPpTz7008c5pFbCE8YzxSZz3NBxjpmkJI9qAegdeg70ZPrS8kUAUWAQn3zRRgj+H9aKLInmMeJcAcVajAxwOfpUES8f4VbjQkdeK5ToJEX/IqZBg9KaiccCplHbrVpkscvJpwFAAI9KMY61oQLjilHA54pB9MUv40BuL/npXNeKvGVl4bgdBBLqF+EMi2lupZ9o6s2M7V9zUmpa7d3V9caZ4ftkuruDAmnmYrBbsRkBiOWbvtH4kVh+Hk8TaJayx3XhtL7UJ3L3F4L6MCYk8HkZAAwAMcVXmSZWs6D4g8UXGl65AunORFC8aoS8Qbzg5yerAKoyR71uaX4p0G51Y6VqWnwadqcbCNldEaJpOu1JBwTgg4ODz0rm7y58WeHNWisdM0j7Na6uzkW0U6TeSw+Z3iyVC8ZODxn8q6a10q6l8NyaTB4chgtmUk/2pOrmRz1ZhHuyxPOciq6DOvVEUBUQKAMYAxTh6dK4rwxcaj4UMPh/wASziZZH22F8CTHJnpCSeQw7Z6jvxXa4z2xSFuJzSdTzilwfWkxk9gaAG596O9KR75pM8YxjPvQJsQjJyKMZPalyD0zmjA7CgGJj2o/Kl5H0oGevagHoJj/ADmil6etFAcpkxDAGePwq0g6c/lVSLJAOM+9W0ZsAA/lXIjpJ1FTpnHFRRnjFTLwOTWkTNj16cn8qdjPNNB4Hel/CrJAgj8aZIdsbMcYHPSn9/SsPxpdyWXgrVJYW2zNCY4z6M5Cj9WFND6FbwMVTwhb38zKJL93u5HPGWkckfoQPwrpwQfrXJ65aRQ2PhvQ4lAia8hj2j+5Epk/9piurAGBgCqsQcFrVtq9z8YNCY3Ih0yK3lliVSCXIAEgYe+5PwFd6TXB694iSP4taFpcNu808UUizEEAKsoGCPXHl5P1ru+gqgMrxNp8Wq+GdQtJE3loGMfqrgZUg9iCARTfC98+p+E9KvZH8ySe1jd2x1baM/rmqWiknxZ4mRiSRPARk9FMK8D8c0nghBbaPd2A4Syv7iBB6L5hZR+TChiR0RHHBpP5U/8ACk7dKQdRoJ9KQjninMcCk/DigLjSuO1JgelS4x0NJg/nQPcaAD0o2n1pwAA+7RxQAzHuaKdz2Y4ooDQw4c4A3Crcbdu9U4D8o/wq2hGPSuRG+xZjPqc1YUd+KqoSTU6McYxWqZDLA6ZxTu3PNRp+NPB9BmqVhBjI9q5zxW4uL3RdJHIvL1Xcf7EQMh/VVFdLnI5rkPES3Nv4/wDDF8XjFlumtnyMkSOmV/Pbj8aqO4mP1i6aT4keHLFFDbI7m5fj7o2BAfzYj8a6zJHXiuP0IvqXxI8QaicNDZpFp0J9CB5kg/Nh+Vdhj6mnsK5yV09pH8U9P3PGs0mlzqMkbmxLGQB36bj+ddUMZwa4C38Oif4wXdzNKkggjS/RjF+8yymIR7ifuDaxwO59q9C280wOZ0/918QddjxgS29rN9T+8X/2UUeGTs1nxJB3XUd//fUMZpnmeV8WDAwx9o0kMp9dkpyP/HxR4edP+E38Uwpn5Jbdj+MIH/stMR0wB7UEc04nB5oPNIkZSH0p3FIaAEx6daTnNO4x0pDQO4LkjmjFApfxosK43aPWinFeaKNB3ZzcJHHWrkfKjvVSDkA+1Wo8e9caOgtRkAetTqfyquvHFTKccVaEydWxUgOeMGokPNSA4PWtUQOHHrWD41sri98J3P2QL9qgaO4iJO0Bo3D9e3ANbwz6Vl+Kra4vPCGrW1oXFxLaSKm1dxyVPAHrTW4jJ+GloIfA9pcbzJLevJdyMzZYs7E8n1xgfhXXgVxfhLxJ4bsPD+n6XHf29nLFCqtBcZhffj5vlfBJzmuri1C1njDR3ELg91cEU2tRWOD1FNaf4yWot2lt7c26HchXy5rdCTIG77t7qB9c16JjmuVurmOD4ladvI/0rTpoojnjcroxH5fyrp/MTHLDH1qxM5nxdIml6pouvPIEitrj7LNnoI5sLn8GCH86b4LgW6fVfERzv1e5Ypk/8sY8pHj6gE/jS+PL7SH8G6paX09s7y27rDCzKXeTadm1epOcYxW7otp9h0KxtfJWHyYETy16KQo4FAi0R6D8aTB9akOcU0mkCsRkGjGRT+vak+lILIZ360bjTiARzSHApisGP/1UmKcM4pDmkVYOPSikIPvRQOxzcIyv0qynSiiuRG5bi5YCrCdBRRViJk55pynKiiitEQx4GBTs80UUxI5TUrG11X4lWdtewR3ENtp0lwiSKGXezqhJB68D9TWhL4J8M3B3SaFYZJ5IgVf5CiimSiJ/h34Wllgm/smFHt23LsyueCMHHUe1WrrwP4auowsuj22P9hNn8sUUVSE9znotA0nRPippsen6dbW6y6bMWCxjqrpg/XkjNd2SaKKroJjaAKKKQxjHBHFBoooGthGGCBSEdqKKRIo6CnACiil1GG0UUUVQj//Z'>




https://github.com/ldhlfzysys/AddIconToScreen/blob/master/AddIconToHome/Web/content.html




```

- [webkit webApp 开发技术要点总结](http://www.cnblogs.com/pifoo/archive/2011/05/28/webkit-webapp.html)

```
<!-- 如何让Safari 知道？其实很简单，就一个meta， -->

<!-- https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html -->

<link rel="apple-touch-icon" href="touch-icon-iphone.png">
<link rel="apple-touch-icon" sizes="152x152" href="touch-icon-ipad.png">
<link rel="apple-touch-icon" sizes="180x180" href="touch-icon-iphone-retina.png">
<link rel="apple-touch-icon" sizes="167x167" href="touch-icon-ipad-retina.png">


<!-- Specifying a Launch Screen Image -->

<link rel="apple-touch-startup-image" href="/launch.png">






```

- [addtohomescreen](https://zhangkn.github.io/add-to-homescreen/src/addtohomescreen.js)

```
<!-- http://cubiq.org/add-to-home-screen?utm_source=caibaojian.com -->

// default options
ath.defaults = {
    appID: 'org.cubiq.addtohome',       // local storage name (no need to change)
    fontSize: 15,               // base font size, used to properly resize the popup based on viewport scale factor
    debug: false,               // override browser checks
    logging: false,             // log reasons for showing or not showing to js console; defaults to true when debug is true
    modal: false,               // prevent further actions until the message is closed
    mandatory: false,           // you can't proceed if you don't add the app to the homescreen
    autostart: true,            // show the message automatically
    skipFirstVisit: false,      // show only to returning visitors (ie: skip the first time you visit)
    startDelay: 1,              // display the message after that many seconds from page load
    lifespan: 15,               // life of the message in seconds
    displayPace: 1440,          // minutes before the message is shown again (0: display every time, default 24 hours)
    maxDisplayCount: 0,         // absolute maximum number of times the message will be shown to the user (0: no limit)
    icon: true,                 // add touch icon to the message
    message: '',                // the message can be customized
    validLocation: [],          // list of pages where the message will be shown (array of regexes)
    onInit: null,               // executed on instance creation
    onShow: null,               // executed when the message is shown
    onRemove: null,             // executed when the message is removed
    onAdd: null,                // when the application is launched the first time from the homescreen (guesstimate)
    onPrivate: null,            // executed if user is in private mode
    privateModeOverride: false, // show the message even in private mode (very rude)
    detectHomescreen: false     // try to detect if the site has been added to the homescreen (false | true | 'hash' | 'queryString' | 'smartURL')
};

```

