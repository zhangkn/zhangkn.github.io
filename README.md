# kn site

[kn](https://zhangkn.github.io) 

# Github Pages 整体思路
本地编写符合Jekyll规范的网站源码，然后上传到github，由github生成并托管整个网站；享受git的版本管理功能；用自己喜欢的编辑器写文章
缺点：不适合大型网站，因为没有用到数据库，每运行一次都必须遍历全部的文本文件，网站越大，生成时间越长
适合暂时还没买服务器，数据库的人

### 使用条件

Jekyll 支持 Mac 、Windows、ubuntu 、Linux 操作系统                     
Jekyll 需要依赖：Ruby、bundler

#### 安装Jekyll
- [Jekyll中文官方文档](http://jekyll.bootcss.com/) 一个静态站点生成器，它会根据网页源码生成静态文件

- [Jekyll使用Liquid模板语言](https://github.com/shopify/liquid/wiki/liquid-for-designers）
 * {{ page.title }}表示文章标题，
 * {{ content }}表示文章内容 
 * { page.date }}则是嵌入文件名的日期（也可以在文件头重新定义date变量），"| date_to_string"表示将page.date变量转化成人类可读的格式
 * {% for post in site.posts %}，表示对所有帖子进行一个遍历。
 *这里要注意的是，Liquid模板语言规定，输出内容使用两层大括号，单纯的命令使用一层大括号。


- 每篇文章的头部，必须有一个[yaml文件头](https://github.com/jekyll/jekyll/wiki/YAML-Front-Matter) 用来设置一些元数据
 每一行设置一种元数据;
 title :该文章的标题  如果不设置这个值，默认使用嵌入文件名的标题


# 添加动态功能必须使用外部服务
- 访问量统计分析
- comment

# 访问量统计分析
- [analytics](https://analytics.google.com/)
- [TrackingCode](https://analytics.google.com/analytics/web/#management/Settings/a111030597w165618623p166148021/%3Fm.page%3DTrackingCode/)
- [Google AdSense申请](https://www.google.com/adsense/?sourceid=aso&subid=WW-ET-ADSBY2)
AdSense 通过让 Google 在您的网站上投放广告，帮助您从中获利。将两个帐户关联之后，您就可以查看投资的回报情况了。
复制粘贴代码进行关联AdSense；放置在 <head> 和 </head> 标记之间；//head.html

- [baidu](https://tongji.baidu.com)
选择的是www.github.io的子域名
- [ 两行代码  搞定计数](http://busuanzi.ibruce.info/)

- [A meta tag that Google can recognize](https://support.google.com/webmasters/answer/79812?hl=zh-Hant)
- [Search Console](https://developers.google.com/search/docs/guides/enhance-site#add-a-sitelinks-searchbox-for-your-site) 用Search Console来配置Google如何显示有关您网站的信息
- [webmastersTools](https://www.google.com/webmasters/tools/ad-experience-unverified?hl=zh-CN&authuser=0)
- [Google AdWords](https://adwords.google.com/home/?subid=ww-ww-et-g-aw-a-gsc_in_prod_1!o2) 在 Google 上宣传您的商家信息并通过 AdWords 吸引新的客户
-[自定义搜索CSE](https://cse.google.com/cse/create/new?hl=zh-CN&cselang=zh-CN&utm_source=wmx)
搜索引擎的名称:Zhangkn.github.io  成功创建自定义搜索引擎 [将该代码添加到您的网站](https://cse.google.com/cse/create/congrats?cx=013483152311799686538%3A__rsnolmpao&hl=zh-CN)
-[allcse](https://cse.google.com/cse/all) 管理cse
- [AdSense 搜索广告 (AFS)](https://support.google.com/adsense/answer/160530?hl=zh-Hans)

- [baiducse](http://zn.baidu.com/cse/site/siteadd)

html标签验证  网站首页html代码的<head>标签与</head>标签之间

# comment
- [disqus](https://disqus.com)
- [universalcode](https://iosre.disqus.com/admin/install/platforms/universalcode/)

- [How to display comment count] 
 Place the following code before your site's closing </body> tag:
 <script id="dsq-count-scr" src="//iosre.disqus.com/count.js" async></script>

# xiaozhuanlan

- [iosre](https://xiaozhuanlan.com/iosre)

# footer.html

- 修改底部的样式

# feed

-[feed]:feed:https://zhangkn.github.io/feed.xml

# stylesheet

- [fontawesome](http://fontawesome.io/icon/github/)
# GitHub buttons

- [GitHub buttons](https://ghbtns.com/#watch)

# 目录结构
- _config.yml   // 全局配置文件所在，位置与名字不可改动
- _posts/       // 文章编写存放之处       注意，文件名必须为"年-月-日-文章标题.后缀名"的格式 如果采用markdown格式，后缀名为md
-|        |--　2012-08-25-hello-world.html
- index.html    // 首页入口文件
- _layouts
-|　　　|--　default.html


# cse  新增站内搜索

```
<script type="text/javascript">

$(document).ready(function(){

 done = false;
 $('form.search').on('submit', function (event) {
    if (false === done) {
      event.preventDefault();
      var orgVal = $(this).find('#search').val();
      $(this).find('#search').val('site:{{ site.url }} ' + orgVal);
      done = true;
      $(this).submit();
    }
  });

});

</script>
```

- [site:https://zhangkn.github.io/   iosre](https://www.google.com.sg/search?dcr=0&ei=QdE4Wov3EYeFvQTNzY24Cg&q=site%3Ahttps%3A%2F%2Fzhangkn.github.io%2F+++iosre&oq=site%3Ahttps%3A%2F%2Fzhangkn.github.io%2F+++iosre&gs_l=psy-ab.3...44442.46722.0.47230.9.9.0.0.0.0.588.1838.3-2j0j2.4.0....0...1.1j4.64.psy-ab..6.0.0....0.1uwBt5n_ubs)

# 其他参考文章
- [http://simiki.org/zh-docs/](http://simiki.org/zh-docs/)
- [Jekyll创始人的示例库](https://github.com/mojombo/tpw)
- [使用BitBucket和FTPloy私有Jekyll源码](http://chenjun.com/)
- [http://chenjun.com/模版](https://github.com/Zhu8/zhu8.github.io)
- [source of others’ blogs](https://github.com/jekyll/jekyll/wiki/Sites)
- [Ghost's default theme (Casper) on Jekyll](https://github.com/rosario/kasper)
- [手把手教你在 Github 上建立 Ghost 博客](http://www.jianshu.com/p/5ce120eb888c)
- [hello-world-vno](http://vno.onevcat.com/2016/02/hello-world-vno/)
- [OneV-s-Den](https://github.com/onevcat/OneV-s-Den)
- [markdown的基本使用](https://blog.zz173.com/detail/1)
- [How to Jailbreak iOS 10-10.2 with Yalu](https://www.cybrary.it/0p3n/jailbreak-ios-10-10-2-yalu/)
- [extra_recipe](https://github.com/xerub/extra_recipe)
- [setting-up-ios-hacking-env](https://togetherwehack.com/2017/09/19/setting-up-ios-hacking-env/)
- [https://gohugo.io/](https://gohugo.io/) https://github.com/gohugoio/hugo/blob/master/CONTRIBUTING.md
- [使用hugo搭建个人博客站点](https://blog.coderzh.com/2015/08/29/hugo/)
- [hugo-pacman-theme](http://coderzh.github.io/hugo-pacman-theme/)
- [StaticGen](https://www.staticgen.com/)
- [dbyll](https://github.com/dbtek/dbyll)
- [tmallfe.github.io](https://github.com/tmallfe/tmallfe.github.io)
- [Huxpro/huxpro.github.io](https://github.com/Huxpro/huxpro.github.io)
- [share-buttons-jekyll](https://superdevresources.com/share-buttons-jekyll/)
- [Simple Sharing Buttons Generator](https://simplesharingbuttons.com/)
- [weibo connect](http://open.weibo.com/connect)
- [http://share.baidu.com/code](http://share.baidu.com/code)
- [baiduShare](https://github.com/hrwhisper/baiduShare)
- [push](http://ziyuan.baidu.com/college/courseinfo?id=267&page=2#h2_article_title18)
- [confluence 文档保存](http://cwiki.apachecn.org/display/Index/Index)
- [microdata-markup-with-mainentityofpage-for-google-article-rich-snippet/](https://stackoverflow.com/questions/36117596/microdata-markup-with-mainentityofpage-for-google-article-rich-snippet/36117597#36117597)
- [adding-schema-org-metadata-to-Jekyll/](http://hungchicheng.me/2017/06/02/adding-schema-org-metadata-to-Jekyll/)
- [http://schema.org/ 结构化数据的模式](http://schema.org/)
- [Schema.org 中文站](https://schema.org.cn/docs/getstarted.html)
- [microdata例子](https://github.com/lidong1665/lidong1665.github.com/blob/master/page/2/index.html)
- [WPHeader](http://schema.org/WPHeader)
- [GitHub 风格的 Markdown 语法](https://mp.weixin.qq.com/s?__biz=MzIyMjE0ODQ0OQ==&mid=2651552757&idx=1&sn=79f77622e411e3d099208bee55f6358a)

- [getting-started-with-writing-and-formatting-on-github](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/)
- [GitHub 上的书写方式](https://mp.weixin.qq.com/s?__biz=MzIyMjE0ODQ0OQ==&mid=2651552756&idx=1&sn=e79d9bf525d4e9c0d2a755088e711993&scene=21#wechat_redirect)
- [https://libraries.io/](https://libraries.io/)
- [gitbook](http://www.chengweiyang.cn/gitbook/basic-usage/README.html)
- [gitbook 发布到 GitHub Pages](http://www.chengweiyang.cn/gitbook/github-pages/README.html)
- [chengweiv5.github.io](https://github.com/chengweiv5/chengweiv5.github.io)
- [使用多种方法进行验证可让您的拥有权得到更全面的保护](https://www.google.com/webmasters/verification/verification?hl=en&siteUrl=https://zhangkn.github.io/)
- [链接提交](http://ziyuan.baidu.com/linksubmit/url?sitename=https%3A%2F%2Fzhangkn.github.io%2Ftags%2F)
- [tagmanager.google.com/ 代码跟踪管理器](https://tagmanager.google.com/#/admin/accounts/create)
- [xmlsitemapgenerator 站点地图](https://xmlsitemapgenerator.org/free/waiting.aspx?job=8b793801-6086-4d18-976a-5e8c004c2bd7)
# 辅助 post.sh  
```
#!/bin/sh
# 根据标题创建post   规则：文件名必须为"年-月-日-文章标题.后缀名"的格式 如果采用markdown格式，后缀名为md
#  post frida
# 获取当前系统的时间
cd /Users/devzkn/githubPages/zhangkn.github.io/_posts
data="`date +%F`"
postName="$data"-"$1".md
content="---\nlayout: post\ntitle: "$1"\ndate: "$data"\ntag: iOSre\n---\n"
echo ${content} > ${postName}
open -b com.sublimetext.3 ${postName}
exit 0
```

- [Gitment：使用 GitHub Issues 搭建评论系统 https://github.com/imsun/gitment](https://imsun.net/posts/gitment-introduction/)
```
1、使用最新的界面与特性:
<div id="container"></div>
<link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
<script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
<script>
var gitment = new Gitment({
  id: '页面 ID', // 可选。默认为 location.href
  owner: '你的 GitHub ID',
  repo: '存储评论的 repo',
  oauth: {
    client_id: '你的 client ID',
    client_secret: '你的 client secret',
  },
})
gitment.render('container')
</script>

2、页面发布后，你需要访问页面并使用你的 GitHub 账号登录（请确保你的账号是第二步所填 repo 的 owner），点击初始化按钮。

之后其他用户即可在该页面发表评论。

```
- [Register a new  github OAuth application 用于登陆GitHub，获取GitHub用户权限](https://github.com/settings/applications/new)
- [duoshuo](https://github.com/duoshuo)
- [gitment  Demo示例页面](https://imsun.github.io/gitment/)
- [Source code of (https://blog.tylinux.com) 使用gitment的例子](https://github.com/tylinux/Blog)
- [conceptclear.github.io github pages 使用gitment的例子](https://github.com/conceptclear/conceptclear.github.io)
```
<div id="container"></div>
<link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
<script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
<script>
var gitment = new Gitment({
  id: 'location.href', // 可选。默认为 location.href
  owner: 'conceptclear',
  repo: 'githubpages-comments',
  oauth: {
    client_id: '',
    client_secret: '',
  },
})
gitment.render('container')
</script>
```

### ad

- [analytics](https://analytics.google.com/analytics/web/?hl=zh#report/visitors-overview/a111030597w165618623p166148021/)

- [tagmanager](https://tagmanager.google.com/?hl=en#/home)


- [admob](https://www.google.cn/admob/)

```
https://firebase.google.com/docs/admob/?hl=zh-cn

https://apps.admob.com/v2/home?_ga=2.208906258.1809489479.1522035233-542669175.1522035233

```

- [广告:Google 在广告中使用 Cookie 的方式](https://www.google.com/intl/zh-CN/policies/technologies/ads/)

```

<!-- Google 在广告中使用 Cookie 的方式: googleads.g.doubleclick.net -->

1)目的: 避免反复向您展示同样的广告，检测并阻止欺诈性点击，以及展示可能具有更高相关度的广告（例如根据您访问过的网站来展示广告）

2) https://www.google.com/intl/zh-CN/policies/privacy/key-terms/#toc-terms-server-logs 包含您的网络请求、IP地址、浏览器类型、浏览器语言、您所发请求的日期和时间以及可唯一标识您的浏览器的一个或多个Cookie
---删除部分IP地址（9个月后）和Cookie的信息（18个月后）

3)我们的广告 Cookie:  AdSense、AdWords、Google Analytics（分析）以及一系列 DoubleClick 品牌服务。当您访问/看到使用其中某个产品的网页/广告时（无论是在 Google 服务中还是在其他网站和应用中），系统都可能会向您的浏览器发送各种Cookie。

---这些 Cookie 可能会通过几个不同的网域进行设置，其中包括 google.com、doubleclick.net、googlesyndication.com、googleadservices.com 或我们合作伙伴的网站所在的网域。我们的部分广告产品可让我们的合作伙伴将其他服务与我们的服务（如广告评估和报表服务）结合使用，而这些服务可将其自身的 Cookie 发送至您的浏览器。此类 Cookie 会通过各项相关服务所在的网域进行设置。


4) Google 使用的 Cookie 类型---https://www.google.com/intl/zh-CN/policies/technologies/types/

5) 如何使用这些 Cookie: https://www.google.com/intl/zh-CN/policies/technologies/cookies/


6) 在网络浏览器中管理 Cookie: https://www.google.com/intl/zh-CN/policies/technologies/managing/

<!-- 广告中使用的其他技术 -->


1) 移动应用的广告标识符:为了能在无法使用 Cookie 技术的服务（例如移动应用）中投放广告，我们可能会使用功能与 Cookie 类似的技术。


将投放在移动应用中的广告所用的标识符与同一设备上的广告 Cookie 相关联，以协调移动应用和移动浏览器中展示的广告。例如，如果您在应用内看到的广告可在移动浏览器中打开一个网页，Google 就可能会进行上述关联





2) 位置信息



<!-- Google 如何使用 Cookie -->

Cookie 是由用户访问的网站向浏览器发送的一小段文本。它帮助浏览器记录有关访问活动的信息，例如首选语言和其他一些设置。

1 )记录您的安全搜索偏好设置、向您展示与您更相关的广告、统计某个网页的访问者数量、帮助您注册我们的服务、保护您的数据，或记住您的广告设置。

<!-- Google 使用的 Cookie 类型:使用不同类型的Cookie来运作Google网站以及与广告相关的产品 -->

1) 广告: 在非 Google 网站上使用的主要广告 Cookie 之一称为“IDE”，它存储在浏览器中的 doubleclick.net 网域下。另一个主要广告 Cookie 存储在 google.com 中，称为“ANID”。我们亦会使用其他 Cookie，例如“DSID”、“FLC”、“AID”、“TAID”和“exchange_uid”。

>* 在您访问的网站所在的网域中设置名为“__gads”或“__gac”的 Cookie。

>* Google 还会使用转化 Cookie，它的主要用途是协助广告主确定用户对广告的点击最终转化为购买行为的次数。通过这些 Cookie，Google 和广告主能够确定您是否点击了广告并在随后访问了广告主的网站

>* 一种专用的 Cookie，称为“Conversion”。这种 Cookie 通常会设置在 googleadservices.com 网域或 google.com 网域（该页面的底部会列出我们用于设置广告 Cookie 的网域）中。我们的一些其他 Cookie 也可用于评估转化事件，例如 DoubleClick Cookie 和 Google Analytics（分析）Cookie。


>* 使用名为“AID”、“DSID”和“TAID”的 Cookie，以便关联您在各种设备上的活动（前提是您之前已在其他设备上登录自己的 Google 帐号）。这样做是为了协调您在各种设备上看到的广告并评估转化事件。这些 Cookie 可能会设置在 google.com/ads、google.com/ads/measurement 或 googleadservices.com 网域中


2) 会话状态 : 这些 Cookie 也可以用于匿名衡量 PPC（每次点击费用）和联属网络营销广告的效果

使用名为“recently_watched_video_id_list” 的 Cookie，这样 YouTube 就可以记录通过某个特定浏览器在近期观看的视频。


3) Google Analytics（分析） : 该工具会使用一系列 Cookie 收集信息并生成网站使用情况统计信息报表

Google Analytics（分析）使用的主要 Cookie 是“__ga”Cookie。


4) 进程: 使用的名为“lbcs”的 Cookie 可让 Google 文档在一个浏览器中打开多个文档。阻止该 Cookie 会导致 Google 文档不能正常使用。


5) 使用安全 Cookie :对用户进行身份验证，防止登录凭据遭到冒用并且防止未经授权的第三方使用用户数据。

使用名为“SID”和“HSID”的 Cookie，其中包含了关于用户 Google 帐户 ID 和最近登录时间的数字签名和加密记录。通过结合使用这两个 Cookie，我们可以阻止多种类型的攻击，例如试图窃取您在网页上填写的表单内容的行为。


6)偏好设置:  称为“NID”的偏好设置 Cookie。浏览器会将该 Cookie 随同请求一起发送至 Google 的网站。NID Cookie 包含一个独一无二的 ID，Google 会利用该 ID 记住您的偏好设置和其他信息，例如您的首选语言（如简体中文）、您希望每页显示的搜索结果条数（如 10 条或 20 条），以及您是否希望启用 Google 的安全搜索过滤器。



<!-- 使用各种网域来设置我们的广告产品中使用的 Cookie -->

admob.com

adsensecustomsearchads.com

adwords.com

doubleclick.net

google.com

googleadservices.com

googleapis.com

googlesyndication.com

googletagmanager.com

googletagservices.com

googletraveladservices.com

googleusercontent.com

google-analytics.com

gstatic.com

urchin.com

youtube.com

ytimg.com



<!-- 在浏览器中管理 Cookie -->


https://adssettings.google.com/?hl=cn


https://support.google.com/chrome/answer/95464?hl=cn

<!-- Google 如何使用模式识别 -->

计算机经过训练可识别组成面部数码影像的常见形状和色彩模式。这个过程称为人脸检测，这项技术可帮助 Google 保护您在街景视图等服务中的隐私权


<!-- Google 使用的位置数据类型 -->

1) 隐式位置信息:   对特定地点手动输入的搜索查询就是一种隐式位置信息

2) 互联网流量信息（例如 IP 地址） : 通常是按照根据国家/地区的流量进行分配的，因此它可以用来确定您的设备所在的国家/地区，以及执行提供搜索时应使用的语言和语言区域等特定操作。此类信息作为互联网流量的常规部分进行发送

3) Google 地图移动版: 精确位置的 GPS 信号、设备传感器、Wi-Fi 接入点和基站 ID 等信息。


<!-- Google 产品隐私权政策指南 -->


https://www.google.com/intl/zh-CN/policies/technologies/product-privacy/


<!-- https://support.google.com/analytics/answer/1011829?hl=zh-Hans -->

引荐来自 googleads.g.doubleclick.net 或 tpc.googlesyndication.com



```

- [googleads.g.doubleclick.net 就算强制经国外VPS代理解析，返回结果依然是国内IP #266](https://github.com/chengr28/Pcap_DNSProxy/issues/266)

```

<!-- CNAME规则就可以对应多个域名。 -->
[CNAME Hosts]
216.58.199.2|216.58.199.6 .*\.doubleclick.net
216.58.199.2|216.58.199.6 .*\.google.com
216.58.199.2|216.58.199.6 .*\.googlevideo.com


```


- [awesome-antifraud](https://github.com/yikebocai/awesome-antifraud/blob/master/README.md)

```

Sift Science: https://siftscience.com/   恶意广告点击的反欺诈工作  机器学习来优化网络欺诈监测和排查模型,不断地补充并修正它的整个欺诈监测模型

<!-- http://xinbo.me/2015/04/tongdun-rd-skills/ -->

http://wowubuntu.com/markdown/

https://maxiang.io/


https://www.zybuluo.com/mdeditor


```

- [groups.google.com/forum/#!网上论坛](https://groups.google.com/forum/#!forum/iosre)

```
https://groups.google.com/forum/#!creategroup

群组电子邮件地址: https://groups.google.com/d/forum/iosre


>* ios 逆向技术：iosre、aso;

>* 利用class-dump、hopper、lua、theos、MonkeyDev、Snoop-it、Keychain-Dumper、运行时常用的API、Cycript、AFLEXLoader进行软件逆向静态分析、动态调试、代码跟踪。

>* https://zhangkn.github.io/tags/

```

- [怎么做到长期写一个价值博客](http://mindhacks.cn/2009/02/15/why-you-should-start-blogging-now/)

```
http://mindhacks.cn/2009/02/09/writing-is-better-thinking/
http://mindhacks.cn/2009/03/09/first-principles-of-programming/

<!-- first-principles-of-programming: 弄清问题永远是问题解决过程中的第一步和最重要的一步。 -->

代码只是工具，不是手段。

http://en.wikipedia.org/wiki/Garbage_in,_garbage_out


```

- [为企业获取 Gmail、文档、云端硬盘和日历服务。](https://gsuite.google.com/index.html?utm_campaign=apps-help-center-promotion&utm_medium=et&utm_source=apps-help-center&utm_content=zh-Hans)

```
https://gsuite.google.com/signup/basic/welcome

iosre@zhangkn.github.io

https://groups.google.com/


```


### 百度广告，即百度联盟广告

>* [reg](http://union.baidu.com/product/prod-cpro.html)


### discourse

>* [iosre](view-source:http://iosre.com/)


```

<meta name="generator" content="Discourse 1.9.4 - https://github.com/discourse/discourse version e24d25ce01b735fb80acff5f313a2f801dba0c66">

<meta name="generator" content="Discourse 2.0.0.beta5 - https://github.com/discourse/discourse version 68ae009f98afd1284121d8195b2851195b72fa56">


```

