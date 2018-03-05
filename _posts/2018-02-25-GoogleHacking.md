---
layout: post
title: GoogleHacking
date: 2018-02-25
tag: tool
site: https://zhangkn.github.io
---

### 前言

Google不仅能帮我们找到所需要的文章，电影，本子[8]和各种gv，还能让我们在渗透检测过程中省时省力


### 正文

>* intext:ReverseEngineering 
```
搜索冒号后接的一个关键字
https://www.reddit.com/r/ReverseEngineering/
```
>* allintext:ReverseEngineering

```
能接多个关键字，能与其他操作符混合使用
```
>* intitle:ReverseEngineering intext:ios
```
intitle搜索网页标题中是否有所输入字符
```

>* allintitle:zhangkn.github.io
```
和ntitle类似，能接多个关键字，但是不能与其他操作符混合使用，可单独使用
```


>* [cache:https://zhangkn.github.io intitle:ReverseEngineering intext:ios](http://webcache.googleusercontent.com/search?q=cache:https://zhangkn.github.io++intitle:ReverseEngineering+intext:ios&num=1&safe=strict&biw=1280&bih=656&strip=1&vwsrc=0)
```
This is Google's cache of https://zhangkn.github.io/. It is a snapshot of the page as it appeared on 20 Feb 2018 15:37:02 GMT.
输入URL，搜索特定页面的缓存快照，即使目标页面发生变动甚至不存在了，依然可以看到它的副本
```

>* define:ReverseEngineering
```
搜索输入关键词或关键词组的定义来源链接;该操作符不能与其他操作符及关键字混用。
```

>* filetype:md intext:Reverse Engineering intitle:Reverse Engineering
```
搜索指定类型的文件;可以与其他操作符混合使用;
ext:与filetype等价
```

>* info:https://zhangkn.github.io
```
搜索输入URL的摘要信息和其他相关信息，该操作符不能与其他操作符及关键字混用
```

>* site:https://zhangkn.github.io
```
将搜索范围缩小到特定的网站，域或子域
```
>* inurl:vi  site:https:zhangkn.github.io
```
1、搜索输入字符是否存在于URL中.可以联合site 使用
https://zhangkn.github.io/2018/02/vi/
2、allinurl:类似inurl:,但是可指定多个字符，不能与其他操作符混合使用，可单独使用
allinurl:vi 02 site:https:zhangkn.github.io
```

>* link:https:zhangkn.github.io
```
搜索链接到所输入URL的页面
```

>* related:https:zhangkn.github.io
```
冒号后接一个URL，搜索与该URL相关的页面
```

>* inanchor:reverseEngineering intext:ios
```
搜索一个HTML标记中的一个链接的文本表现形式:
```

>* stocks:高阳

```
搜索关于指定公司的股票市场信息

```

>* insubject:reverseEngineering intext:ios

```
搜索Google组的标题行

```
### 例子

>* 端口扫描 inurl:8080 -intext:8080
```
inurl:8080 -intext:8080
inurl:nqt -intext:8080 filetype:php intext:"network query tool"  --http://portal.trgsites.de/network/nqt.php
```

>*  SQL数据库挖掘
```
filetype:sql + "IDENTIFIED BY" -cvs
filetype:sql + "IDENTIFIED BY" ("Grant * on *" | "create user")
filetype:mp4 inurl:xxx intext:xxx: Google不仅能帮我们找到所需要的文章，电影，本子[8]和各种gv，还能让我们在渗透检测过程中省时省力
```


### see also
- [如何用好 Google 搜索引擎解决你的问题](https://xiaozhuanlan.com/topic/5798613204)

```
一、基础搜索语法: +、-(逻辑“非”的操作)、OR、""(表示是完全匹配搜索) 
备注：目前 Google filetype 指令支持以下文档和图片类型：
Adobe Acrobat PDF (.pdf)
Adobe Postscript (.ps)
Autodesk DWF (.dwf)
Google地球 KML (.kml)
Google地球 KMZ (.kmz)
Lotus 1-2-3（wk1、wk2、wk3、wk4、wk5、wki、wks、wku）
Lotus WordPro (.lwp)
MacWrite (.mw)
Microsoft Excel (.xls)
Microsoft Powerpoint (.ppt)
Microsoft Word (.doc)
Microsoft Works（wks、wps、wdb）
Microsoft Write (.wri)
Rich Text Format富文本格式 (.rtf)
Shockwave Flash (.swf)
纯文本（ans、txt）
图片类型：

filetype:jpg
filetype:gif
filetype:png
filetype:bmp

```
- [bfinject](https://github.com/BishopFox/bfinject/blob/master/README.md#credits)
- [https://minapp.com/miniapp/](https://minapp.com/miniapp/)
- [breaking-into-ios-11](https://blog.elcomsoft.com/2018/02/breaking-into-ios-11/)
- [IOSurfaceRootUserClient Port UAF](http://blog.pangu.io/iosurfacerootuserclient-port-uaf/)
- [async_wake_ios](https://bugs.chromium.org/p/project-zero/issues/detail?id=1417#c3)
- [https://coolstar.org/electra/](https://coolstar.org/electra/)
- [https://twitter.com/saurik/status/952725123601084416](https://twitter.com/saurik/status/952725123601084416)
- [https://blog.tylinux.com/2018/02/07/install-tweak-with-electra-on-ios11/](https://blog.tylinux.com/2018/02/07/install-tweak-with-electra-on-ios11/)
- [知名 Tweak 开发者 CoolStar 基于 Comex 开发的 CydiaSubstrate 的开源替代: Substitute](https://github.com/comex/substitute)
- [手动完成一个 Tweak 的安装（因为没有 Cydia）,结合 https://github.com/BishopFox/bfinject 使用----期待Electra的最终版本](https://blog.tylinux.com/2018/02/07/install-tweak-with-electra-on-ios11/)

```
知名 Tweak 开发者 CoolStar 基于 Comex 开发的 CydiaSubstrate 的开源替代: Substitute，开发了 Electra 越狱工具。支持 iOS11.0 - iOS 11.1.2 的全部 iOS 设备。
```
- [《iOS逆向工程》- 越狱](https://blog.tylinux.com/2017/07/24/reverse-engineering-001/)
```
jailbreak在Android上叫Root，在iOS上叫越狱，在Symbian上叫免签，在Web入侵时叫提权; 
iOS 10开始，非完美越狱也越来越难，虽然在 Google Project Zero 的“帮助”下，Luca Todesco 开发了 Yalu102 ，但是支持设备有限，越狱稳定性不高。
```

- [reverse-neteasemusic-001](https://blog.tylinux.com/2017/07/18/reverse-neteasemusic-001/)
- [在macOS下高效使用命令行](https://blog.tylinux.com/2016/08/30/live-under-console-on-macOS/)

```
macOS的内核衍生自FreeBSD，属于正统Unix.
使用Linux式命令: Linux下的标准命令包含在由GNU提供的coreutils包中，在Mac下可以通过Homebrew安装coreutils包来获取支持(brew install coreutils)
<!-- 把coreutils的命令设为默认，取代macOS提供的命令。在你的shell配置文件：bash是~/.bashrc，zsh是~/.zshrc中追加如下两条配置： -->
# 把coreutils的bin目录放在PATH前边，这样就优先执行coreutils的命令
PATH="/usr/local/opt/coreutils/libexec/gnubin:$PATH"
# 优先调用coreutls的man信息
MANPATH="/usr/local/opt/coreutils/libexec/gnuman:$MANPATH"
<!-- coreutils的ls命令默认没有颜色，同样在配置文件中加入下边这行： -->
alias ls="ls --color=tty"
<!-- 修改Homebrew源 :国内良心组织/个人提供国内镜像。比如中科大LUG（Linux User Group），使用如下命令修改：-->
# 详见https://lug.ustc.edu.cn/wiki/mirrors/help/brew.git
# 替换homebrew默认源
cd /usr/local
git remote set-url origin git://mirrors.ustc.edu.cn/brew.git
# 详见https://lug.ustc.edu.cn/wiki/mirrors/help/homebrew-bottles
# 替换homebrew bottles默认源
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bashrc

```

- [让天下没有难下的源http://ban.ninja/](http://ban.ninja/)

```
设置环境变量 HOMEBREW_BOTTLE_DOMAIN 即可使用本镜像源加速下载 Homebrew 资源。

bash
在 ~/.bashrc 中加入

export HOMEBREW_BOTTLE_DOMAIN=http://7xkcej.dl1.z0.glb.clouddn.com
fish
在 ~/.config/fish/config.fish 中加入

set -x HOMEBREW_BOTTLE_DOMAIN http://7xkcej.dl1.z0.glb.clouddn.com
```

- [让终端程序使用代理](https://blog.tylinux.com/2016/08/30/live-under-console-on-macOS/)

```
<!-- 1、环境变量:对于支持从环境变量中读取代理配置的程序，比如wget,git等，可以通过设置http_proxy或者https_proxy环境变量使代理生效。 -->
export http_proxy=http://ip:port
wget http://www.baidu.com
<!--2、 proxychains: ，比如shadowsocks、GoAgent使用的socks5协议 可以使用proxychains命令来使代理生效。-->
<!-- 首先通过Homebrew安装proxychains-ng包： -->
brew install proxychains-ng
<!-- 之后需要对proxychain进行配置，proxychains的配置文件路径是/usr/local/etc/proxychains.conf。打开配置文件，注释掉最后一行的默认配置，在之后按照协议 IP 端口 用户名(可选) 密码(可选)的格式添加自己的代理服务器配置。示例如下： -->
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
#socks4     127.0.0.1 9050
socks5 127.0.0.1 1080
<!-- proxychains本身支持代理链，顾名思义就是通过代理A连接代理B再连接代理C…连接目标服务器，如需使用代理链，只需要按次序将代理服务器信息填写到配置文件中既可。 -->

<!-- 3、有些程序自身支持代理，比如curl和git等。 -->
<!-- curl支持通过配置文件设置代理的方法，打开curl配置文件（~/.curlrc） -->
proxy="protocal://ip:port"
# protocal可以是http、https、socks4/5
<!-- git也支持通过将代理信息写入配置文件的方式设置代理： -->
# git支持http、https、socks代理
git config --global http.proxy "protocal://ip:port"
git config --global https.proxy "protocal://ip:port"
<!-- 或者直接打开git配置文件（~/.gitconfig），写入如下配置：

 -->
 [http]
proxy="protocal://ip:port"
[https]
proxy="protocal://ip:port"


```

- [闷骚的悟空](https://zongquan.wang/archives/)
- [Luke's Homepage](http://luklab.com/)
- [reverse-engineering](https://github.com/wtsxDev/reverse-engineering)
- [iOS-Reverse-Engineering-presentation](https://github.com/aozhimin/iOS-Reverse-Engineering-presentation)
- [http://www.freetechbooks.com](http://www.freetechbooks.com)
- [Google Hacking————你真的会用Google吗？](https://zhuanlan.zhihu.com/p/22161675)
- [https://blog.quarkslab.com/category/reverseengineering.html](https://blog.quarkslab.com/category/reverseengineering.html)
- [charles 手机抓https包: Configure your device to use Charles as its HTTP proxy on 192.168.2.139:8888, then browse to chls.pro/ssl to download and install the certificate.](https://mp.weixin.qq.com/s/c0RIfRhEPmVzR0JHvorgFw)
```
charlesV4.2 的辅助功能：
Compose:可直接自定义对应的请求，并执行该请求。这个对我们抓包用处很大。我们可以从中得到该接口的必填参数、验证我们生成的Sign等等的。
Repeat：很简单就是执行一次重复请求操作
```
- [webtools](https://123.w3cschool.cn/webtools)


