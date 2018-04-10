---
layout: post
title: SystemIsUnavailable
date: 2018-04-08
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

>* 用peopen替换system

>* 使用posix_spawn替代system


### 正文

>* : 'system' is unavailable: not available on iOS

```

使用FILE	*popen(const char *, const char *) __DARWIN_ALIAS_STARTING(__MAC_10_6, __IPHONE_2_0, __DARWIN_ALIAS(popen)) __swift_unavailable_on("Use posix_spawn APIs or NSTask instead.", "Process spawning is unavailable.");

替代 int	 system(const char *) __DARWIN_ALIAS_C(system);


```


### 替代system的方案

>*  [例子： yalu102 使用Xcode9 进行编译：popen 替代 system](https://github.com/iosjb/KNyalu102/blob/master/yalu102/jailbreak.m)

```
//               system("killall -9 cfprefsd");
                
                popen("killall -9 cfprefsd","r");

```

>* [使用posix_spawn 替代system](https://github.com/iosjb/KNyalu102/blob/master/yalu102/system/utils.c)

```

int run(const char *cmd) {
    char *myenviron[] = {
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/bin/X11:/usr/games",
        "PS1=\\h:\\w \\u\\$ ",
        NULL
    };
    
    pid_t pid;
    char *rawCmd = fixedCmd(cmd);
    char *argv[] = {"sh", "-c", (char*)rawCmd, NULL};
    int status;
    status = posix_spawn(&pid, "/bin/sh", NULL, NULL, argv, (char **)&myenviron);
    if (status == 0) {
        if (waitpid(pid, &status, 0) == -1) {
            perror("waitpid");
        }
    } else {
        printf("posix_spawn: %s\n", strerror(status));
    }
    free(rawCmd);
    return status;
}

```




### see also

- [廣告計費的主要三種方式](https://transbiz.com.tw/ppc%E9%97%9C%E9%8D%B5%E5%AD%97%E5%BB%A3%E5%91%8A-cpc-cpm-cpa-%E5%BB%A3%E5%91%8A%E6%8A%95%E6%94%BE/)



![](/images/posts/{{page.title}}/ads.png)

```
<!-- 廣告計費的主要三種方式 -->

<!-- 1. PPC（pay-per-click） -->

PPC（pay-per-click）每次點擊付費，只有當消費者「點擊」廣告內容時，廣告業主才需要付給平台費用。

每消費者每點擊一次，所需花費的費用，則被稱作每次點擊成本（Cost Per Click, CPC）。

<!-- 2. PPM（pay-per-impression） -->

當使用者輸入相關關鍵字搜尋的時候，可以看到你的產品或服務出現在搜尋結果中，或是根據他過去的搜尋習慣，在平台上被動出現使用者可能感興趣的廣告訊息。


有別於上述的每次點擊收費，PPM是當你的廣告只要出現潛在消費者的面前，平台就會和你收取費用，通常是以千次為單位計算。我們會說，這個廣告的收費方式是以曝光量計費，而每當有一千個人看到你的廣告，你所需要支付的廣告費用，則被稱作CPM（Cost Per 1000 impression）每千次曝光成本。



<!-- 3. PPA（pay-per-acquisition） -->
消費者完成某一個指定動作，如「下載電子書」、「註冊成為會員」、「購買票卷」等情況下，廣告業主才需支付平台費用，而費用計算的方式則被稱作CPA（cost-per action）每次行動成本。



<!--  -->
```

```
<!-- 跟廣告投放相關的專有名詞-->
CPC（Cost Per Click）每次點擊成本：每次消費者點擊你的廣告，你所需要支付給平台的費用

CPM（Cost Per 1000 impression）每千次曝光成本：每一千個人看到你的廣告，你所需要支付的廣告費用

CPA（Cot Per Action）每次完成行動成本：讓消費者留下對品牌的實際印象、填寫表格甚至註冊成員會員等，正式進入到你的行銷漏斗中，所要花費的金額。

PPC（Pay Per Click）點擊付費式的廣告：當消費者點擊以後，業主要付費的廣告類型

CTR（Click-Through Rate）點擊率：人們看到你的廣告並且點擊廣告的比率。比方說，你的廣告曝光了1000次，但看到廣告並且點進去的人只有5個，那麼點擊率就是（5÷1000）×100%= 0.5%

CVR（Conversion Rate）廣告轉換率：人們點擊廣告以後轉換（成交）的次數。假設有20個人點擊你的廣告，但只有一個人購買你的商品或服務，那麼你的廣告轉換率就是（1÷20）×100%= 5%

ROI（Return on Investment）投資報酬率：廣告投放的成效，可以簡單地透過「廣告賺了多少淨利潤」，「在廣告投放花了多少錢」計算。比方說你透過廣告為該次的銷售賺得6萬元的淨利潤，在廣告投放上花了2萬元，那麼你的投資報酬率就是[（6-2）÷2]×100%= 200%


```

- [Python老兵的新征程](https://www.doyj.com/2016/05/01/python%E8%80%81%E5%85%B5%E7%9A%84%E6%96%B0%E5%BE%81%E7%A8%8B/amp/)


```

用pyenv来管理python的不同版本,--- 不太好用，同由于系统是系统是Python 2.7才采用它。

用pip做依赖管理

```

- [在线广告作弊手段一览](https://www.doyj.com/2011/11/11/%E5%9C%A8%E7%BA%BF%E5%B9%BF%E5%91%8A%E4%BD%9C%E5%BC%8A%E6%89%8B%E6%AE%B5%E4%B8%80%E8%A7%88/amp/)

```
<!--一、 CPM广告方式的作弊: 每當有一千個人看到你的廣告，你所需要支付的廣告費用，則被稱作CPM（Cost Per 1000 impression）每千次曝光成本。 -->
<!-- 页面内嵌入本站页面的iframe -->
在自己的网页上嵌入iframe, 大小为0x0或1×1，也就是用户不可见。通过iframe打开其他页面，在用户看不见的情况下刷流量

iframe打开和当前页一样的页面地址，或本站的其他页面----- 通过分析UV，独立IP等很容易就发现异常


<!-- 两个站点间互相嵌入对方站点页面的iframe -->

UV，独立IP等分析方法是不能发现异常的。


<!-- 双层iframe -->

些在线广告在显示时会判断浏览窗口大小，如果太小可能就不能显示。这时有些网站就采用了双层iframe技术来刷广告流量。 第一层1×1大小的iframe中又嵌入一个iframe，这个第二层iframe是正常浏览窗口大小，广告代码很难发现异常。


让主页面和两个iframe使用三个不同的域名，这样因为跨域的问题， 里面的js不可能得到最外层真正的页面地址， 想抓证据都抓不到。


<!-- IP屏蔽 -->

<!-- 购买垃圾流量 -->




<!-- 二、CPC作弊 -->
广告主只有当使用者实际上点击广告以拜访广告主的网站时，才需要支付费用

CPC作弊其实是很简单的，只要用iframe打开点击链接即可

<!-- CPA作弊 -->

有些网站广告按CPA结算，比如注册人数--- 自动注册机



```

- [【web开发 js回调】JavaScript回调函数的理解与使用](https://blog.csdn.net/dengpeng0419/article/details/68173118)



- [user-agent 的设置]

```


<!-- chrome:   针对特定网站进行设置 -->


chrome-extension://djflhoibgkdhkhhcedjiklpkjnoahfmg/options.html


https://zhangkn.github.io/  Mozilla/5.0 (iPhone; CPU iPhone OS 6_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/6.0 Mobile/10A5376e Safari/8536.25 

下载：https://plus.google.com/114188876079505437133/posts/KHKAwui59Nj

ps:Permanent spoofs always override any user-agent selection in the toolbar.

<!-- Safari -->
<!-- 2、 safari   设置CustomUserAgent -->
 defaults write com.apple.Safari CustomUserAgent "\"Mozilla/5.0 (iPhone; CPU iPhone OS 10_3 like Mac OS X) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.3 Mobile/14E277 Safari/603.1.30\""


```


- [chrome 模拟手机设备](https://www.googleadservices.com/pagead/aclk?sa=L&ai=CO95KbQDLWpy7D82K9wWG7IuwBvCXy9dQk4yhuIcHZBABIPirsl5gnQGgAf-Ere0DyAEDqQKxcDf7g5e0PqgDAcgDwQSqBKQBT9AR3oYEb4ixxUrgtsFIblulSNoUa_QgmoZZqq72ECP-kYdDBjlpDkw6Jx-IE0VATYLk_5wCxLL1H6kN5dOxhem6lcgNTiif16_AgrZuw5ix5LgCYfbO7dlV91jgZbZgJaK4LhsXyEL1SRpy1SrCq_dHcW4stDsTcAhBIXQK3u9DNB6xt4dbVoA8nB3JamW95pMK18bPPjdfQdNyi_RD3voWXULABIeqkYcziAWP56NWoAYD2AYCgAeH6f8wqAeOzhuoB9XJG6gHpr4b2AcBoAjsELAIAtIIBwiAARABGAKxCaAqTm05I51ygAoB2BMMghQTGhF6aGFuZ2tuLmdpdGh1Yi5pbw&num=1&cid=CAASEuRo9SijobMKnj5tK7PIhKNsAQ&sig=AOD64_3aTeRzSuI0OA7U4iZg1cptC8Gh2g&client=ca-pub-5776998009494842&nm=2&mb=2&bg=!iYqlipJEW0JxeMGbGvMCAAAATVIAAAASmQGvGPS24Wz1XVx1oqNkAqbsTQGBLBChZI1OIsawTMI6ckQpq_oe9iFPB13wawy_fZeGjlYH6FgPMNS5zyDFB1kwX50S6EuY_lEO4FoMgTU5wkUbNtU1JLe0wSzr479rSryWwdDdyJnu5raOXaLr9docAcpggcB2V-6ZchTcO3ABg014b9HIlasdmyCSfnA5OYCSqxL4ueQeGmGYS_RJrM159yPwu2PGsiIMoD9Lhitl1tSiEfyWwBw5JfJL8oMtOtzd4iSRuAJyEnJYIpgB0RcB_Rs4rWDan76kDGXU8aHaXkcYdKG5slxErJkDIdCmLofidSzmcVX2f25PbCNkRH3pIOm7lYqZgQdnchzW0c4ChdpQqRjhc-IDdxGzas3n1EJXc3PQgGsCDAqP3wrLEaZk__a8-o738ihZm3fbN5qtJtlIhXQlZK3FiTgZwshh6JRAWi16h0jMuAlOuoqQuP2DyHXb9BVnRT1sNy7WSwnsIyObP_j3fCGYnr5BgM0hwTHI1RqL9SLYsa-qZbqILZDadFg5Kypr-iA_ojulUUv7wl-BPWS4uHALEBJsUBqd_Og&adurl=http://www.plus500.com.sg/%3Fid%3D86735%26tags%3Dg_sr%252b180941711_cpi%252bSingaporeTopics_cp%252b13705172231_agi%252bFinance_agn%252bzhangkn.github.io_pl%252bm_de%252bUURL)

```
<!-- 供web手机端开发测试用 -->
option + command + i 

<!--1、 使用插件设置ua -->
<!-- https://chrome.google.com/webstore/detail/%E8%AE%BE%E5%A4%87%E6%A8%A1%E6%8B%9F%E5%99%A8/ifpngllemddebnolonloogahnoopbofg?hl=zh-CN -->


https://plus.google.com/114188876079505437133/posts/KHKAwui59Nj



<!-- safari 也可以设置对应的ua -->
https://safari-extensions.apple.com
safari修改user agent

已经搜到如下适用于OS X 10.7系统的解决方式：
      在终端中输入如下代码 defaults write com.apple.SafariCustomUserAgent"'UserAgent信息'"


例如切到iPad：

defaults write com.apple.Safari CustomUserAgent "\"Mozilla/5.0 (iPad; CPU OS 8_1 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12B410 Safari/600.1.4\""


Name: Value
Referer: https://zhangkn.github.io/2018/04/SystemIsUnavailable/
Host: zhangkn.github.io
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Connection: keep-alive
Accept-Language: en-us
Accept-Encoding: br, gzip, deflate

User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 10_3 like Mac OS X) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.3 Mobile/14E277 Safari/603.1.30



Cookie: Hm_lpvt_e7f6d46714fcd28fb24be011203bee5f=1523255984; Hm_lvt_e7f6d46714fcd28fb24be011203bee5f=1522846555,1522846817,1523255229,1523255480; _ga=GA1.3.1365453668.1513008145; _gid=GA1.3.1412130643.1523255229

<!-- 2、 safari   设置CustomUserAgent -->
 defaults write com.apple.Safari CustomUserAgent "\"Mozilla/5.0 (iPhone; CPU iPhone OS 10_3 like Mac OS X) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.3 Mobile/14E277 Safari/603.1.30\""


<!-- 3、Chrome 可以通过设置network conditions 进行ua 的设置 -->
使用 chrome 自带的修改 agent 的功能

例如：chrome -iPhone 

Mozilla/5.0 (iPhone; CPU iPhone OS 9_1 like Mac OS X) AppleWebKit/601.1 (KHTML, like Gecko) CriOS/65.0.3325.181 Mobile/13B143 Safari/601.1.46



<!-- 16768 ??         0:23.04 /Applications/Google Chrome.app/Contents/MacOS/Google Chrome -->

    <key>CFBundleIdentifier</key>
    <string>com.google.Chrome</string>


<!-- 三、使用Chrome修改user agent模拟微信内置浏览器 -->

Mozilla/5.0 (Linux; Android 4.4.4; HM NOTE 1LTEW Build/KTU84P) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/33.0.0.0 Mobile Safari/537.36 MicroMessenger/6. 0.0.54_r849063.501 NetType/WIFI

判断微信内置浏览器主要通过 MicroMessenger 字段，如果有这个字段，则判断该浏览器为微信客户端浏览器。






<!-- 没有效果 -->
defaults write com.google.Chrome CustomUserAgent "\"Mozilla/5.0 (iPhone; CPU iPhone OS 10_3 like Mac OS X) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.3 Mobile/14E277 Safari/603.1.30\""


user-agent: Mozilla/5.0 (iPhone; CPU iPhone OS 5_0 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9A334 Safari/7534.48.3


user-agent: Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Mobile Safari/537.36


User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36





Request URL: https://pagead2.googlesyndication.com/pub-config/r20160913/ca-pub-5776998009494842.js
Request Method: GET
Status Code: 200  (from disk cache)
Remote Address: 203.208.40.109:443
Referrer Policy: no-referrer-when-downgrade


age: 2430
alt-svc: hq="googleads.g.doubleclick.net:443"; ma=2592000; quic=51303432; quic=51303431; quic=51303339; quic=51303335,quic="googleads.g.doubleclick.net:443"; ma=2592000; v="42,41,39,35",hq=":443"; ma=2592000; quic=51303432; quic=51303431; quic=51303339; quic=51303335,quic=":443"; ma=2592000; v="42,41,39,35"
cache-control: public, max-age=43200
content-encoding: gzip
content-length: 88
content-type: text/javascript
date: Mon, 09 Apr 2018 02:49:12 GMT
expires: Mon, 09 Apr 2018 14:49:12 GMT
server: sffe
status: 200
x-content-type-options: nosniff
x-xss-protection: 1; mode=block



Provisional headers are shown
Referer: https://zhangkn.github.io/
User-Agent: Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Mobile Safari/537.36


<!-- Request URL: https://googleads.g.doubleclick.net/pagead/html/r20180402/r20170110/zrt_lookup.html -->

<!-- Request URL: https://pagead2.googlesyndication.com/pagead/js/r20180402/r20170110/show_ads_impl.js -->

Request URL: https://pagead2.googlesyndication.com/pagead/js/r20180402/r20170110/show_ads_impl.js
Request Method: GET
Status Code: 200  (from disk cache)
Remote Address: 203.208.50.90:443
Referrer Policy: no-referrer-when-downgrade


alt-svc: hq="googleads.g.doubleclick.net:443"; ma=2592000; quic=51303432; quic=51303431; quic=51303339; quic=51303335,quic="googleads.g.doubleclick.net:443"; ma=2592000; v="42,41,39,35",hq=":443"; ma=2592000; quic=51303432; quic=51303431; quic=51303339; quic=51303335,quic=":443"; ma=2592000; v="42,41,39,35"
cache-control: private, max-age=1209600
content-disposition: attachment; filename="f.txt"
content-encoding: gzip
content-length: 66894
content-type: text/javascript; charset=UTF-8
date: Sun, 08 Apr 2018 01:51:38 GMT
etag: 14776789531727904599
expires: Sun, 08 Apr 2018 01:51:38 GMT
p3p: policyref="https://www.googleadservices.com/pagead/p3p.xml", CP="NOI DEV PSA PSD IVA IVD OTP OUR OTR IND OTC"
server: cafe
status: 200
timing-allow-origin: *
x-content-type-options: nosniff
x-xss-protection: 1; mode=block


Provisional headers are shown
Referer: https://zhangkn.github.io/
User-Agent: Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Mobile Safari/537.36

<!-- analytics -->

Request URL: https://www.google-analytics.com/r/collect?v=1&_v=j66&a=1084747826&t=pageview&_s=1&dl=https%3A%2F%2Fzhangkn.github.io%2F&ul=zh-cn&de=UTF-8&dt=ReverseEngineering&sd=24-bit&sr=400x544&vp=400x544&je=0&_u=AACAAEAB~&jid=2021211169&gjid=1835717271&cid=841685145.1519869648&tid=UA-111030597-1&_gid=60404584.1523152297&_r=1&z=30747402


<!-- cse -->

Request URL: https://cse.google.com/cse/api/partner-pub-5776998009494842/cse/2886811629/queries/js?oe=UTF-8&callback=(new+PopularQueryRenderer(document.getElementById(%22queries%22))).render


Request URL: https://cse.google.com/cse/query_renderer.js


<!-- googleads -->


Request URL: https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-5776998009494842&output=html&adk=1812271804&adf=3025194257&lmt=1523244722&loeid=38893312%2C332260007&plat=2%3A16777216%2C16%3A8388608&format=0x0&url=https%3A%2F%2Fzhangkn.github.io%2F&ea=0&flash=0&pra=5&wgl=1&dt=1523253356540&bpp=102&bdt=457&fdt=113&idt=429&shv=r20180402&cbv=r20170110&saldr=aa&correlator=3615180665817&frm=20&ga_vid=841685145.1519869648&ga_sid=1523253357&ga_hid=1084747826&ga_fc=0&pv=2&iag=3&icsg=2&nhd=1&dssz=3&mdo=0&mso=0&u_tz=480&u_his=3&u_java=0&u_h=579&u_w=400&u_ah=579&u_aw=400&u_cd=24&u_nplug=0&u_nmime=0&adx=0&ady=0&biw=400&bih=579&abxe=1&scr_x=0&scr_y=0&eid=21061122%2C38893302%2C332260003%2C33895412%2C20040066&oid=3&rx=0&eae=2&fc=912&brdim=0%2C0%2C0%2C0%2C400%2C0%2C400%2C579%2C400%2C579&vis=1&rsz=%7C%7Cnr%7C&abl=CS&ppjl=u&fu=8216&bc=1&osw_key=2591979424&ifi=0&dtd=454
Request Method: GET
Status Code: 200 
Remote Address: 203.208.40.109:443
Referrer Policy: no-referrer-when-downgrade


alt-svc: hq="googleads.g.doubleclick.net:443"; ma=2592000; quic=51303432; quic=51303431; quic=51303339; quic=51303335,quic="googleads.g.doubleclick.net:443"; ma=2592000; v="42,41,39,35",hq=":443"; ma=2592000; quic=51303432; quic=51303431; quic=51303339; quic=51303335,quic=":443"; ma=2592000; v="42,41,39,35"
content-encoding: br
content-length: 47368
content-type: text/html; charset=UTF-8
date: Mon, 09 Apr 2018 05:55:57 GMT
p3p: policyref="https://googleads.g.doubleclick.net/pagead/gcn_p3p_.xml", CP="CURa ADMa DEVa TAIo PSAo PSDo OUR IND UNI PUR INT DEM STA PRE COM NAV OTC NOI DSP COR"
server: cafe
status: 200
timing-allow-origin: *
x-content-type-options: nosniff
x-xss-protection: 1; mode=block



:authority: googleads.g.doubleclick.net
:method: GET
:path: /pagead/ads?client=ca-pub-5776998009494842&output=html&adk=1812271804&adf=3025194257&lmt=1523244722&loeid=38893312%2C332260007&plat=2%3A16777216%2C16%3A8388608&format=0x0&url=https%3A%2F%2Fzhangkn.github.io%2F&ea=0&flash=0&pra=5&wgl=1&dt=1523253356540&bpp=102&bdt=457&fdt=113&idt=429&shv=r20180402&cbv=r20170110&saldr=aa&correlator=3615180665817&frm=20&ga_vid=841685145.1519869648&ga_sid=1523253357&ga_hid=1084747826&ga_fc=0&pv=2&iag=3&icsg=2&nhd=1&dssz=3&mdo=0&mso=0&u_tz=480&u_his=3&u_java=0&u_h=579&u_w=400&u_ah=579&u_aw=400&u_cd=24&u_nplug=0&u_nmime=0&adx=0&ady=0&biw=400&bih=579&abxe=1&scr_x=0&scr_y=0&eid=21061122%2C38893302%2C332260003%2C33895412%2C20040066&oid=3&rx=0&eae=2&fc=912&brdim=0%2C0%2C0%2C0%2C400%2C0%2C400%2C579%2C400%2C579&vis=1&rsz=%7C%7Cnr%7C&abl=CS&ppjl=u&fu=8216&bc=1&osw_key=2591979424&ifi=0&dtd=454
:scheme: https
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
accept-encoding: gzip, deflate, br
accept-language: zh-CN,zh;q=0.9
cookie: id=222438da12780072||t=1519869518|et=730|cs=002213fd48125a727410d4e5f4; DSID=ADyxukuHgiWLQ9dCOgzwZxqeATa_LjRGPnJjXGFnNvYvhZ2gxmslqGzJuVS5z1kLjoGK8qbegN-gfTBZmD6i2pC4v1SkkArnaa-ryLNSrYQtb1IJiB4Xj0A
referer: https://zhangkn.github.io/
upgrade-insecure-requests: 1
user-agent: Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Mobile Safari/537.36
x-client-data: CI62yQEIprbJAQjEtskBCKmdygEIqKPKARiSo8oB



client: ca-pub-5776998009494842
output: html
adk: 1812271804
adf: 3025194257
lmt: 1523244722
loeid: 38893312,332260007
plat: 2:16777216,16:8388608
format: 0x0
url: https://zhangkn.github.io/
ea: 0
flash: 0
pra: 5
wgl: 1
dt: 1523253356540
bpp: 102
bdt: 457
fdt: 113
idt: 429
shv: r20180402
cbv: r20170110
saldr: aa
correlator: 3615180665817
frm: 20
ga_vid: 841685145.1519869648
ga_sid: 1523253357
ga_hid: 1084747826
ga_fc: 0
pv: 2
iag: 3
icsg: 2
nhd: 1
dssz: 3
mdo: 0
mso: 0
u_tz: 480
u_his: 3
u_java: 0
u_h: 579
u_w: 400
u_ah: 579
u_aw: 400
u_cd: 24
u_nplug: 0
u_nmime: 0
adx: 0
ady: 0
biw: 400
bih: 579
abxe: 1
scr_x: 0
scr_y: 0
eid: 21061122,38893302,332260003,33895412,20040066
oid: 3
rx: 0
eae: 2
fc: 912
brdim: 0,0,0,0,400,0,400,579,400,579
vis: 1
rsz: ||nr|
abl: CS
ppjl: u
fu: 8216
bc: 1
osw_key: 2591979424
ifi: 0
dtd: 454



Request URL: https://pagead2.googlesyndication.com/pagead/js/r20180402/r20170110/osd.js

Request URL: https://googleads.g.doubleclick.net/pagead/adview?ai=CH9XzcADLWoOzH4SC9gXPnryoAqXVnbEFjdzCup8Cpp6tjWsQCCD4q7JeKAxgnQHIAQGoAwHIAwKqBJIBT9DN2Yedx80PobFHMDtErcNg07nN1Qtrif86jCTLosx5kMu0OAEpbLz9k2sb7NSKJG8UrlD8DzFNzurnPHmonrILsuIXH72Copd3pB86El7-nrXWCN1KTS9sLAsCUx1ShmTTtE5e-D8rlcmBAoV0M7LKeGYnzDOpBx9FrLEzx6T3lEMAT5RiOZf0vGnjP6ArXznABKXAy4o0iAW9u41skgUECBoYBKAGRcAGC9gGAoAH5ffiNagHjs4bqAfVyRuoB6a-G9gHAfIHBBC37QegCOwQsAgC0ggHCIABEAEYAoAKAYIUExoRemhhbmdrbi5naXRodWIuaW8&sigh=cc7yss7Zmjs

ADyxukuHgiWLQ9dCOgzwZxqeATa_LjRGPnJjXGFnNvYvhZ2gxmslqGzJuVS5z1kLjoGK8qbegN

222438da12780072

```

>* [googleadservices](https://www.google.com/adsense/new/u/0/pub-5776998009494842/home)


```
<!-- cse.google.com  General -->

Request URL: https://www.googleadservices.com/pagead/aclk?sa=L&ai=DChcSEwiwjL2HoazaAhUUJCsKHX6tAxAYABAAGgJzZg&ohost=cse.google.com&cid=CAASEeRosGemPQC-ljCbnIoMFwGK&sig=AOD64_2bBbZborTwGxjPnUrPFbXk1N04-Q&adurl=&q=&nb=0&res_url=https%3A%2F%2Fcse.google.com%2Fcse%3Fcx%3Dpartner-pub-5776998009494842%3A2886811629%26q%3Diosre%26oq%3Diosre%26gs_l%3Dpartner-generic.3...0.0.3.15830.0.0.0.0.0.0.0.0..0.0.gsnos%252Cn%253D13...0.12j144j2..1ac..25.partner-generic..0.0.0.&rurl=https%3A%2F%2Fzhangkn.github.io%2Fabout%2F&nm=23&nx=63&ny=5&is=1237x124&clkt=104&bg=!VlWlVU1Ew4KtQuEtCycCAAAAMVIAAAAPmQEzcoBm63sQPypfwo0pvD4vOCmThmMwaBP4b2lmUnzOEH-mtPc2NBl-4qr87bD1Dz8TIY2jjxkOjjhQX7E1dwZiVs3Jr6ZDI2iRmQc-NYXnPK1Sr0B1ZnXoyoRU0b-Q7HymmHd1VFLKYxIQ9m685xhjmhasd7Iuy4WG7Fx77Z-JJuzp41V7C4oP7mBOzgY9iVXhHwlRMVe-CjoJ9fh5fdxDYGGU3KsTHnyvx0AYbi78ibcLh0fOFb5FD3LEga3iJVDorOVNkYvCjp90_qn8IVEkBNe98mlnYCjUKQz8753KR1NScawghDX7r5D0aaad7av6c4lsqOTXfKyeIMuI42s2s_D2bdUT9KIUMfd_BU4rIj9CMrE3QGSGG18I9ffBq1UlXOzVeWANOV_4BXA5MsKD2TljRA


Request Method: GET
Status Code: 302 
Remote Address: 203.208.40.109:443
Referrer Policy: origin

<!-- response headers -->

alt-svc: hq="googleads.g.doubleclick.net:443"; ma=2592000; quic=51303432; quic=51303431; quic=51303339; quic=51303335,quic="googleads.g.doubleclick.net:443"; ma=2592000; v="42,41,39,35",hq=":443"; ma=2592000; quic=51303432; quic=51303431; quic=51303339; quic=51303335,quic=":443"; ma=2592000; v="42,41,39,35"
cache-control: no-cache, must-revalidate
content-length: 0
content-type: text/html; charset=UTF-8
date: Mon, 09 Apr 2018 03:33:22 GMT
expires: Fri, 01 Jan 1990 00:00:00 GMT
location: http://qoo10.sg/ad?keyword=ios&jaehuid=2000180847
p3p: policyref="http://www.googleadservices.com/pagead/p3p.xml", CP="NOI DEV PSA PSD IVA IVD OTP OUR OTR IND OTC"
pragma: no-cache
server: adclick_server
set-cookie: Conversion=CukBQzRzTVEtOTdLV3ZDaUY1VElyQUgtMm82QUFhREFtOHBQMnBpT25wWUdnSktaQ0FnQUVBRWdwS3poWDJDX0JhQUIzZUNtNWdQSUFRR3BBcjktTXRCVWZLay15QU5icWdRMFQ5RDhQWEtOOXZISHFKUVpPUXRpZ01EQ3Rwd2VfQWh2UndCb1Z5XzhVQ2o1SURwcHlNMWxRcGpFUUpyM3cyMExVb1VZcjhBRWd1cmx4WDJnQmxHQUI4cjl1Q3FRQndHb0I2YS1HOWdIQWJFSmQ4Ql8tdmVDb2M2NUNYZkFmX3IzZ3FITy1Ba0ISEwiIrtOKoazaAhXVAyoKHY4qAlUYASDNmp-oz-Ke2f8BSAGQAdqYjp6WBpgBAKABAKgBALABAA; expires=Sun, 08-Jul-2018 03:33:22 GMT; path=/pagead/conversion/1019850845/
status: 302
x-content-type-options: nosniff
x-xss-protection: 1; mode=block

<!-- query string parameters -->

sa: L
ai: DChcSEwiwjL2HoazaAhUUJCsKHX6tAxAYABAAGgJzZg
ohost: cse.google.com
cid: CAASEeRosGemPQC-ljCbnIoMFwGK
sig: AOD64_2bBbZborTwGxjPnUrPFbXk1N04-Q
adurl: 
q: 
nb: 0
res_url: https://cse.google.com/cse?cx=partner-pub-5776998009494842:2886811629&q=iosre&oq=iosre&gs_l=partner-generic.3...0.0.3.15830.0.0.0.0.0.0.0.0..0.0.gsnos%2Cn%3D13...0.12j144j2..1ac..25.partner-generic..0.0.0.
rurl: https://zhangkn.github.io/about/
nm: 23
nx: 63
ny: 5
is: 1237x124
clkt: 104
bg: !VlWlVU1Ew4KtQuEtCycCAAAAMVIAAAAPmQEzcoBm63sQPypfwo0pvD4vOCmThmMwaBP4b2lmUnzOEH-mtPc2NBl-4qr87bD1Dz8TIY2jjxkOjjhQX7E1dwZiVs3Jr6ZDI2iRmQc-NYXnPK1Sr0B1ZnXoyoRU0b-Q7HymmHd1VFLKYxIQ9m685xhjmhasd7Iuy4WG7Fx77Z-JJuzp41V7C4oP7mBOzgY9iVXhHwlRMVe-CjoJ9fh5fdxDYGGU3KsTHnyvx0AYbi78ibcLh0fOFb5FD3LEga3iJVDorOVNkYvCjp90_qn8IVEkBNe98mlnYCjUKQz8753KR1NScawghDX7r5D0aaad7av6c4lsqOTXfKyeIMuI42s2s_D2bdUT9KIUMfd_BU4rIj9CMrE3QGSGG18I9ffBq1UlXOzVeWANOV_4BXA5MsKD2TljRA

```
