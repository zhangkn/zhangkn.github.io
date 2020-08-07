#【[ 新博客地址]】(kunnan.blog.csdn.net)


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



>* [重要文件]

```
devzkndeMacBook-Pro:zhangkn.github.io devzkn$ git checkout --  css/main.css
devzkndeMacBook-Pro:zhangkn.github.io devzkn$ git checkout --  _layouts/post.html
devzkndeMacBook-Pro:zhangkn.github.io devzkn$ git checkout --   _includes/footer.html
devzkndeMacBook-Pro:zhangkn.github.io devzkn$ git diff  _config.yml

1） 目前此博客版本，缺少catalog，我打算采用新的版本，可能暂时不更新。


```

>* [创建博客文章的辅助脚本：](https://github.com/zhangkn/KNBin/blob/master/post)

>* [devzkndeMacBook-Pro:~ devzkn$ cat .bash_profile]

```
使用别名快速的开启本地调试模式
alias sio='bundle exec jekyll server -s ~/githubPages/zhangkn.github.io'


Configuration file: /Users/devzkn/githubPages/zhangkn.github.io/_config.yml
            Source: /Users/devzkn/githubPages/zhangkn.github.io
       Destination: /Users/devzkn/githubPages/zhangkn.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
                    done in 32.002 seconds.
 Auto-regeneration: enabled for '/Users/devzkn/githubPages/zhangkn.github.io'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
      Regenerating: 1 file(s) changed at 2018-04-19 14:49:31 
```

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

### jekyll

>* [permalinks](https://www.jekyll.com.cn/docs/permalinks/)

```
permalink: /blog/:year/:month/:day/:title  /blog/2009/04/29/slap-chop/index.html

permalink: /:year/:month/:title/           https://zhangkn.github.io/2018/04/launchApplicationWithIdentifier/

```

>* [pages](https://www.jekyll.com.cn/docs/pages/)

```

<!-- 站点上任何 HTML 文件，包括主页，都可以使用布局和 include 中的内容一般共用的内容，如页面的 header 和 footer. 将合适的部分抽出放到布局中。 -->

<!DOCTYPE html>
<html xmlns:wb="https://open.weibo.com/wb"><!-- 微博分享 在HTML标签中增加XML命名空间 -->


  {% include head.html %}

  <body>
<!-- 命名文件夹：在站点的根目录下为每一个页面创建一个文件夹，并把 index.html 文件放在每 个文件夹里。 -->

 /Users/devzkn/githubPages/zhangkn.github.io/_site/2018/04/AutoUnlock/index.html


<!-- 一个站点下通常会有：主页 (homepage), 关于 (about), 和一个联系 (contact) 页。根目录下的文件结构和对应生成的 URL 会是下面的样子： -->
<!--  方式一：命名 HTML 文件 -->

.
|-- _config.yml
|-- _includes/
|-- _layouts/
|-- _posts/
|-- _site/
|-- about.html    # => http://example.com/about.html --- 本站采用about.md  /Users/devzkn/githubPages/zhangkn.github.io/_site/about/index.html 
|-- index.html    # => http://example.com/           ----目前本站有
└── contact.html  # => http://example.com/contact.html ---- 暂时没有，因为联系方式在底部

<!-- 方式二：命名一个文件夹并包含一个 index.html 文件 ：  本站http://zhangkn.github.io/采用这种方式-->

你只需要为每个顶级页面创建一个文件夹，并包含一个 inex.html 文件。 这样，每个 URL 就将以文件夹的名字作为结尾，网站服务器会将对应的 index.html 展示给用户。下面是一个示例来展示这种结构的样子：

.
├── _config.yml
├── _includes/
├── _layouts/
├── _posts/
├── _site/
├── about/
|   └── index.html  # => http://example.com/about/
├── contact/
|   └── index.html  # => http://example.com/contact/
└── index.html      # => http://example.com/


```


>* [posts](https://www.jekyll.com.cn/docs/posts/)

```
简单的管理你电脑中的一个文件夹下的文本文件就 可以写文章并方便的在线上发布。与繁琐的配置和维护数据库和基于网站的内容管理系统（CMS）相比， 这是一个非常受欢迎的改变。

<!-- 文章文件夹 -->
所有的文章都在_posts文件夹中。 这些文件可以用Markdown 编写， 也可以用Textile 格式编写。只要文件中有 YAML头信息，它们就会从源格式转化成 HTML 页面，从而成为 你的静态网站的一部分。



```


>* [frontmatter: 头信息](https://www.jekyll.com.cn/docs/frontmatter/)

```
<!-- 任何只要包含 YAML 头信息的文件在 Jekyll 中都能被当做一个特殊的文件来处理。头信息必须在文件的开始部分，并且需要按照 YAML 的格式写在两行三虚线之间。下面是一个 -->

---
layout: post
title: NSFileHandle
date: 2018-04-17
tag: ios #可以通过 YAML list来指定，或者用空格隔开
site: https://zhangkn.github.io
---

两行的三虚线之间，你可以设置一些预定义的变量

创建一个你自己定义的变量。这样在接下来的文件和任意模板中或者在包含这些页面或博客的模板中都可以通过使用 Liquid 标签来访问这些变量


<!-- 预定义的全局变量 -->

1) layout

如果设置的话，会指定使用该模板文件。指定模板文件时候不需要扩展名。模板文件需要放在 _layouts 目录下。


2) permalink---相当于重写

如果你需要让你的博客中的URL地址不同于默认值 /year/month/day/title.html 这样，那么当你设置这个变量后，就会使用最终的URL地址。



3） published----设置是否公开

当站点生成的时候，如果你不需要展示一个具体的博文，可以设置这个变量为 false。

4） category

categories

除过将博客文章放在某个文件夹下面外，你还可以根据文章的类别来给他们设置一个或者多个分类属性。这样当你的博客生成的时候这些文章就可以根据这些分类来阅读。在一个文章中多个类别可以通过 YAML list来指定，或者用空格隔开。

5） tags---- 本站就是采用这种方式

类似分类，一篇文章也可以给它增加一个或者多个标签。同样多个标签之间可以通过 YAML 列表或者空格隔开。


<!-- 自定义变量 -->

    <title>{{ page.title }}</title>

<!-- 在文章中预定义的变量 -->

date

这里的日期会覆盖文章名字中的日期。这样就可以用来确定文章分类的正确。

```

>* [structure](https://www.jekyll.com.cn/docs/structure/)

```

Jekyll 的核心其实是一个文本转换引擎。它的概念其实就是： 你用你最喜欢的标记语言来写文章，可以是 Markdown，也可以是 Textile,或者就是简单的 HTML, 然后 Jekyll 就会帮你套入一个或一系列的布局中。在整个过程中你可以设置URL路径, 你的文本在布局中的显示样式等等

<!-- 一个基本的 Jekyll 网站的目录结构一般是像这样的： -->

.
├── _config.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.textile
|   └── on-simplicity-in-technology.markdown
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
|   └── 2009-04-26-barcamp-boston-4-roundup.textile
├── _data
|   └── members.yml
├── _site
└── index.html

目前我比较关心是Jekyll否可以在_posts 目录进行创建子目录


<!-- 来看看这些都有什么用： -->

_config.yml

保存配置数据。很多配置选项都会直接从命令行中进行设置，但是如果你把那些配置写在这儿，你就不用非要去记住那些命令了。

_drafts  --- 目前尚未使用

drafts 是未发布的文章。这些文件的格式中都没有 title.MARKUP 数据。学习如何使用 drafts.

_includes --- 

你可以加载这些包含部分到你的布局或者文章中以方便重用。可以用这个标签  {% include file.ext %} 来把文件 _includes/file.ext 包含进来。

            {% include footer.html %}


_layouts

layouts 是包裹在文章外部的模板。布局可以在 YAML 头信息中根据不同文章进行选择。 。标签 {{ content }} 可以将content插入页面中。

<section class="post">
    {{ content }}
  </section>



_posts

这里放的就是你的文章了。文件格式很重要，必须要符合: YEAR-MONTH-DAY-title.MARKUP。 The permalinks 可以在文章中自己定制，但是数据和标记语言都是根据文件名来确定的。----- 是否可以创建子目录？？？

<!-- 博客文章放在_posts目录中，可以使用子目录。 -->
http://holbrook.github.io/2013/05/27/jekyll_mysite.html

http://holbrook.github.io/2017/11/10/ebook_with_sphinx.html




_data

Well-formatted site data should be placed here. The jekyll engine will autoload all yaml files (ends with .yml or .yaml) in this directory. If there's a file members.yml under the directory, then you can access contents of the file through site.data.members.

_site

一旦 Jekyll 完成转换，就会将生成的页面放在这里（默认）。最好将这个目录放进你的 .gitignore 文件中。----这点很重要

devzkndeMacBook-Pro:zhangkn.github.io devzkn$ cat .gitignore
_site
.sass-cache
.jekyll-metadata



index.html and other HTML, Markdown, Textile files

如果这些文件中包含 YAML 头信息 部分，Jekyll 就会自动将它们进行转换。当然，其他的如 .html， .markdown，  .md，或者 .textile 等在你的站点根目录下或者不是以上提到的目录中的文件也会被转换。


Other Files/Folders

其他一些未被提及的目录和文件如  css 还有 images 文件夹， favicon.ico 等文件都将被完全拷贝到生成的 site 中。 

ps: http://tom.preston-werner.com/2008/11/03/how-to-meet-your-next-cofounder.html


```

>* [分页功能Pagination](https://www.jekyll.com.cn/docs/pagination/)


```
# Pagination 分页功能
plugins: [jekyll-paginate,jekyll-sitemap,jekyll-feed]
paginate: 20
paginate_path: "page/:num/"


<!-- paginate: 5  每页需要几行, 即每个页面展示的文字数量，目前我定位20 篇-->

<!-- paginate_path: "blog/page:num"    需要带有分页页面的配置-->

blog/index.html将会读取这个设置，把他传给每个分页页面，然后从第 2 页开始输出到 blog/page:num ， :num 是页码。

如果有 12 篇文章并且做如下配置 paginate: 5 ， Jekyll会将前 5 篇文章写入 blog/index.html ，把接下来的 5 篇文章写入 blog/page2/index.html，

最后 2 篇写入 blog/page3/index.html。

<!-- 具体的效果请看这个 -->
第一页：https://zhangkn.github.io/#blog  

第二页： https://zhangkn.github.io/page/2/#blog



<!-- 与 paginator 相同的属性 -->

page 当前页码

per_page 每页文章数量

posts 当前页的文章列表

total_posts 总文章数

total_pages 总页数

previous_page 上一页页码 或 nil

previous_page_path 上一页路径 或 nil

next_page 下一页页码 或 nil

next_page_path  下一页路径 或 nil



<!-- 生成带分页功能的文章 -->


<!-- 分页链接 -->
<hr class="post-list__divider " />

<nav class="pagination" role="navigation">

  <!-- 百度搜索 
    <script type="text/javascript">(function(){document.write(unescape('%3Cdiv id="bdcs"%3E%3C/div%3E'));var bdcs = document.createElement('script');bdcs.type = 'text/javascript';bdcs.async = true;bdcs.src = 'http://znsv.baidu.com/customer_search/api/js?sid=5841224908133801750' + '&plate_url=' + encodeURIComponent(window.location.href) + '&t=' + Math.ceil(new Date()/3600000);var s = document.getElementsByTagName('script')[0];s.parentNode.insertBefore(bdcs, s);})();</script>-->

  
    {% if paginator.previous_page %}
        <a class="newer-posts pagination__newer btn btn-small btn-tertiary" href="{{ paginator.previous_page_path }}#blog">&larr; 最近</a>
    {% endif %}
    <span class="pagination__page-number">{{ paginator.page }} / {{ paginator.total_pages }}</span>
    {% if paginator.next_page %}
        <a class="older-posts pagination__older btn btn-small btn-tertiary" href="{{ paginator.next_page_path }}#blog">更早 &rarr;</a>
    {% endif %}

    

</nav>


<!-- 遍历分页后的文章 -->


<ol class="post-list">
    {% for post in paginator.posts %}
    <li>
      <h2 class="post-list__post-title post-title"><a href="{{ post.url }}" title="访问 {{ post.title }}">{{ post.title }}</a></h2>
      <p class="excerpt">{{ post.content | strip_html | strip_newlines | truncate: 250 }}&hellip;</p>
      <div class="post-list__meta">        
        <time datetime="{{post.date | date: date_to_xmlschema}}" class="post-list__meta--date date">
          <img src="/images/calendar.png" width="20px" /> 
          {{ post.date | date: "%F"}}</time> 
        <div class = "tag-img-icon">
          <img src="/images/tag-icon.svg" width="20px" /> 
        </div>
        <a href="/tags">
          <div class = "post-list-icon-mate">
            <span class="post-list__meta--tags-right">{{ post.tags }}</span>
          </div>
          <div class = "post-list-small-mate">
          <a class="btn-border-small" href={{ post.url }}>阅读全文 » </a>
          </div>
      </div>
      <hr class="post-list__divider" />
    </li>
    {% endfor %}
  </ol>

<!-- 遍历分页后的文章 官方的代码-->

<!-- 遍历分页后的文章 -->
{% for post in paginator.posts %}
  <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
  <p class="author">
    <span class="date">{{ post.date }}</span>
  </p>
  <div class="content">
    {{ post.content }}
  </div>
{% endfor %}

```


>* [plugins](https://www.jekyll.com.cn/docs/plugins/)


```
https://www.jekyll.com.cn/docs/plugins/

GitHub Pages是由Jekyll提供技术支持的，考 虑到安全因素，所有的 Pages 通过 --safe 选项禁用了插件功能，因此如果你的网 站部署在 Github Pages ，那么你的插件不会工作。


不过仍然有办法发布到 GitHub Pages，你只需在本地做一些转换，并把生成好的文件上传到 Github 替代 Jekyll 就可以了。


<!-- 在网站根下目录建立 _plugins 文件夹，插件放在这里即可。 Jekyll 运行之前，会加载此目录下所有以 *.rb 结尾的文件。 -->

目前没采用

<!-- 在 _config.yml 文件中，添加一个以 gems 作为 key 的数组，数组中存放插件的 gem 名称。例如： -->

 gems: [jekyll-test-plugin, jekyll-jsonify, jekyll-assets]
 # This will require each of these gems automatically.



<!-- _plugins and gems 可以同时使用。 -->

```

>* [github-pages](https://www.jekyll.com.cn/docs/github-pages/)

```
https://www.jekyll.com.cn/docs/github-pages/


Github Pages 是面向用户、组织和项目开放的公共静态页面搭建托管服 务，站点可以被免费托管在 Github 上，你可以选择使用 Github Pages 默 认提供的域名 github.io 或者自定义域名来发布站点。Github Pages 支持 自动利用 Jekyll 生成站点，也同样支持纯 HTML 文档，将你的 Jekyll 站 点托管在 Github Pages 上是一个不错的选择。

Github Pages 依靠 Github 上项目的某些特定分支来工作。Github Pages 分为两种基本类型：用户/组织的站点和项目的站点。搭建这两种类型站 点的方法除了一小些细节之外基本一致。


<!-- 一、用户和组织的站点 -->

<!-- 建立主页:GitHub用户通过创建特殊名称的Git版本库或在Git库中建立特别的分支实现对主页的维护: https://zhangkn.github.io/2018/03/GitHub/ -->

仓库中master分支里的文件将会被用来生成 Github Pages 站点，所以请 确保你的文件储存在该分支上。





<!-- 二、项目的站点-->

项目的站点文件存放在项目本身仓库的 gh-pages 分支中。该分支下的文件将会被 Jekyll 处理，生成的站点会被 部署到你的用户站点的子目录上，例如 username.github.io/project



项目主页:项目主页也通过二级域名进行访问 http://gotgithub.github.io/项目名称—https://zhangkn.github.io/AlipayWalletTweakF

在Git库中建立特别的gh-pages分支实现对主页的维护。
<!-- 使用命令行创建干净的gh-pages分支：http://www.worldhello.net/gotgithub/03-project-hosting/050-homepage.html -->

```

# see also

>* [建立主页:GitHub用户通过创建特殊名称的Git版本库或在Git库中建立特别的分支实现对主页的维护](https://zhangkn.github.io/2018/03/GitHub/)

>* [《机器学习》](http://holbrook.github.io/2017/03/04/machine-learning_index.html)

>* [https://huangxuan.me/](https://huangxuan.me/)

```

个人比较喜欢它的tag 以及，catalog


https://github.com/Huxpro/huxpro.github.io

http://qiubaiying.top/2017/02/06/%E5%BF%AB%E9%80%9F%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/

<!-- 整个网站结构 -->

├── _config.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.textile
|   └── on-simplicity-in-technology.markdown
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
|   └── 2009-04-26-barcamp-boston-4-roundup.textile
├── _data
|   └── members.yml
├── _site
├── img
└── index.html


_config.yml 全局配置文件
_posts  放置博客文章的文件夹
img 存放图片的文件夹


<!-- 基础设置 -->

然后编辑_config.yml的内容

# Site settings
title: You Blog               #你博客的标题
SEOTitle: 你的博客 | You Blog      #显示在浏览器上搜索的时候显示的标题
header-img: img/post-bg-rwd.jpg   #显示在首页的背景图片
email: You@gmail.com  
description: "You Blog"        #网站介绍
keyword: "BY, BY Blog, 柏荧的博客, qiubaiying, 邱柏荧, iOS, Apple, iPhone" #关键词
url: "https://qiubaiying.github.io"          # 这个就是填写你的博客地址
baseurl: ""      # 这个我们不用填写


# Sidebar settings
sidebar: true                           # 是否开启侧边栏.
sidebar-about-description: "说点装逼的话。。。"
sidebar-avatar:/img/avatar-by.JPG      # 你的个人头像 这里你可以改成我在img文件夹中的两张备用照片 img/avatar-m 或 avatar-g


<!-- 社交账号:社交账号的用户名 -->

# SNS settings
RSS: false
weibo_username:     username
zhihu_username:     username
github_username:    username
facebook_username:  username
jianshu_username: jianshu_id


jianshu_id 在你打开你的简书主页后的地址如：http://www.jianshu.com/u/e71990ada2fd中，后面这一串数字：e71990ada2fd

<!-- 评论系统 -->

博客中使用的是 Disqus 评论系统，在 官网 注册帐号后，按下面的步骤简单的配置即可：



https://disqus.com/home/settings/profile/

这个 Username 就是我们 _config.yml 中 disqus_username



# Disqus settings（https://disqus.com/）
disqus_username: qiubaiying


<!-- Gitalk  -->
http://qiubaiying.top/2017/12/19/%E4%B8%BA%E5%8D%9A%E5%AE%A2%E6%B7%BB%E5%8A%A0-Gitalk-%E8%AF%84%E8%AE%BA%E6%8F%92%E4%BB%B6/

集成了 Baidu Analytics 和 Google Analytics，到各个网站注册拿到track_id替换




<!-- 网站统计 -->


# Analytics settings
# Baidu Analytics
ba_track_id: 83e259f69b37d02a4633a2b7d960139c

# Google Analytics
ga_track_id: 'UA-90855596-1'            # Format: UA-xxxxxx-xx
ga_domain: auto


<!-- 好友 -->


<!-- 写文章 -->

<!-- 格式 -->


YAML 就是我们配置 _config文件用的语言。http://www.ruanyifeng.com/blog/2016/07/yaml.html?f=tt

MarkDown 是一种轻量级的「标记语言 :http://sspai.com/25137/ 


<!-- 首页标签 -->


# Featured Tags
featured-tags: true                     # 是否使用首页标签
featured-condition-size: 1              # 相同标签数量大于这个数，才会出现在首页


<!-- 自定义域名 -->

购买一个自己的域名。

在阿里云购买的域名: https://wanwang.aliyun.com/domain/?spm=5176.8006371.1007.dnetcndomain.q1ys4x


GitHub Pages 无法处理中文域名



<!-- 解析域名 -->

注册好域名后，需要将域名解析到你的博客上

管理控制台 → 域名与网站（万网） → 域名
选择你注册好的域名，点击解析

1) 分别添加两个A 记录类型,

一个主机记录为 www,代表可以解析 www.qiubaiying.top的域名


另一个为 @, 代表 qiubaiying.top



记录值就是我们博客的IP地址，是 GitHub Pagas 在美国的服务器的地址 151.101.100.133


devzkndeMacBook-Pro:github devzkn$ ping zhangkn.github.io
PING sni.github.map.fastly.net (151.101.229.147): 56 data bytes


然后 GitHub Pages 再通过 CNAME记录 跳转到你的主页上


2)修改CNAME

<!-- 利用GithHub Desktop管理GitHub仓库 -->



<!-- 修改个人介绍需要修改根目录下的 about.html 文件 -->



<!-- 在本地调试博客 -->
sio

<!-- 修改主页的座右铭 -->

index.html 文件

<!-- 如何在博客文章中上插入图片 -->

MarkDown 中添加图片的形式是 :![](图片的URL)

1) 所以，要在 MacDown 中插入图片，这张图片就需要上传到图床（网上），然后在引 用这张图片的URL。

  这样速度会快一些

2） 将图片上传到图床： iPic

直接拖动图片到 P 图标上，或者选中图片按快捷键 ⌘+U，

<!-- 推荐几个好用软件 -->

https://en.toolinbox.net/iPic/

https://macdown.uranusjr.com/

https://github.com/MacDownApp/macdown/releases/download/v0.7.1/MacDown.app.zip


图片压缩工具：https://imageoptim.com/




```

# MacDown

![MacDown logo](http://macdown.uranusjr.com/static/images/logo-160.png)

Hello there! I’m **MacDown**, the open source Markdown editor for OS X.

Let me introduce myself.

## Markdown and I

**Markdown** is a plain text formatting syntax created by John Gruber, aiming to provide a easy-to-read and feasible markup. The original Markdown syntax specification can be found [here](http://daringfireball.net/projects/markdown/syntax).

You can configure various application (that's me!) behaviors in the [**General** preference pane](#general-pane).


### Line Breaks
To force a line break, put two spaces and a newline (return) at the end of the line.

* This two-line bullet 
won't break

* This two-line bullet  
will break

Here is the code:

```
* This two-line bullet 
won't break

* This two-line bullet  
will break
```

### Strong and Emphasize

**Strong**: `**Strong**` or `__Strong__` (Command-B)  
*Emphasize*: `*Emphasize*` or `_Emphasize_`[^emphasize] (Command-I)

### Headers (like this one!)

  Header 1
  ========

  Header 2
  --------

or

  # Header 1
  ## Header 2
  ### Header 3
  #### Header 4
  ##### Header 5
  ###### Header 6



### Links and Email
#### Inline
Just put angle brackets around an email and it becomes clickable: <uranusjr@gmail.com>  
`<uranusjr@gmail.com>`  

Same thing with urls: <http://macdown.uranusjr.com>  
` <http://macdown.uranusjr.com>`  

Perhaps you want to some link text like this: [Macdown Website](http://macdown.uranusjr.com "Title")  
`[Macdown Website](http://macdown.uranusjr.com "Title")` (The title is optional)  


### Lists

* Lists must be preceded by a blank line (or block element)
* Unordered lists start each item with a `*`
- `-` works too
  * Indent a level to make a nested list
    1. Ordered lists are supported.
    2. Start each item (number-period-space) like `1. `
    42. It doesn't matter what number you use, I will render them sequentially
    1. So you might want to start each line with `1.` and let me sort it out


### Block Quote

> Angle brackets `>` are used for block quotes.  
Technically not every line needs to start with a `>` as long as
there are no empty lines between paragraphs.  
> Looks kinda ugly though.
> > Block quotes can be nested.  
> > > Multiple Levels
>
> Most markdown syntaxes work inside block quotes.
>
> * Lists
> * [Links][arbitrary_id]
> * Etc.

### Inline Code
`Inline code` is indicated by surrounding it with backticks:  
`` `Inline code` ``


#### Table

This is a table:

First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell

You can align cell contents with syntax like this:

| Left Aligned  | Center Aligned  | Right Aligned |
|:------------- |:---------------:| -------------:|
| col 3 is      | some wordy text |         $1600 |
| col 2 is      | centered        |           $12 |
| zebra stripes | are neat        |            $1 |

The left- and right-most pipes (`|`) are only aesthetic, and can be omitted. The spaces don’t matter, either. Alignment depends solely on `:` marks.


### TeX-like Math Syntax
I can also render TeX-like math syntaxes, if you allow me to.[^math] I can do inline math like this: \\( 1 + 1 \\) or this (in MathML): <math><mn>1</mn><mo>+</mo><mn>1</mn></math>, and block math:

\\[
    A^T_S = B
\\]

or (in MathML)

<math display="block">
    <msubsup><mi>A</mi> <mi>S</mi> <mi>T</mi></msubsup>
    <mo>=</mo>
    <mi>B</mi>
</math>


### Task List Syntax
1. [x] I can render checkbox list syntax
  * [x] I support nesting
  * [x] I support ordered *and* unordered lists
2. [ ] I don't support clicking checkboxes directly in the html window


### Jekyll front-matter
If you like, I can display Jekyll front-matter in a nice table. Just make sure you put the front-matter at the very beginning of the file, and fence it with `---`. For example:

```
---
title: "Macdown is my friend"
date: 2014-06-06 20:00:00
---
```

