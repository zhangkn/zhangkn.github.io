# [新博客地址](kunnan.blog.csdn.net)


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

- [
