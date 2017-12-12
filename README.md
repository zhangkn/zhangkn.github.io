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

# 访问量统计分析
[analytics](https://analytics.google.com/)
[baidu](https://tongji.baidu.com)
# comment
[disqus](https://disqus.com)
# xiaozhuanlan
[iosre](https://xiaozhuanlan.com/iosre)
# footer.html
修改底部的样式
# feed
[feed](feed:https://zhangkn.github.io/feed.xml)

# stylesheet
[fontawesome](http://fontawesome.io/icon/github/)
# GitHub buttons
[GitHub buttons](https://ghbtns.com/#watch)

# 目录结构
- _config.yml   // 全局配置文件所在，位置与名字不可改动
- _posts/       // 文章编写存放之处       注意，文件名必须为"年-月-日-文章标题.后缀名"的格式 如果采用markdown格式，后缀名为md
|        |--　2012-08-25-hello-world.html
- index.html    // 首页入口文件
-_layouts
|　　　|--　default.html


# 其他参考文章
- [Jekyll创始人的示例库](https://github.com/mojombo/tpw)
- [使用BitBucket和FTPloy私有Jekyll源码](http://chenjun.com/)
- [http://chenjun.com/模版](https://github.com/Zhu8/zhu8.github.io)
- [source of others’ blogs](https://github.com/jekyll/jekyll/wiki/Sites)







