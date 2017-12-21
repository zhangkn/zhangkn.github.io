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




