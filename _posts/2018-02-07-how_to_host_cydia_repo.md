---
layout: post
title: how_to_host_cydia_repo
date: 2018-02-07
tag: iOSre
site: https://zhangkn.github.io
---

### 前言


将 Tweak 部署到大量设备上和更新的解决方案是搭建私有Cydia源 ；而非通常的make package install 、dpkg -i


>* 目录结构：deb 的源本质上就是需要特定结构的目录
```
cydia--
           |--debs--*.deb
           |--Packages  :dpkg-scanpackages debs /dev/null > Packages ;Packages 文件中包含源中每个包文件的信息，包括文件路径、大小、依赖、架构及校验信息
           |--Packages.bz2 ：由Packages文件压缩而来, 命令行: bzip2 Packages；
           |--Packages.gz
           |--Release  ：是一个普通的文本文件，用于描述当前源的信息；这些信息会在 Cydia 的源列表及 Tweak 搜索列表中显示
           |--Release.gpg
```


### [Cydia源服务器搭建.](https://github.com/zhangkn/KNBin/blob/master/kncydia)


>* 搭建软件源，必须保证至少有Release【可选】和Packages两个文件,[具体请看辅助脚本](https://github.com/zhangkn/KNBin/blob/master/kncydia)
```
dpkg-scanpackages: info: Wrote 1 entries to output Packages file.
Serving HTTP on 0.0.0.0 port 8088 ...
192.168.2.156 - - [06/Feb/2018 18:50:04] "HEAD /Packages.bz2 HTTP/1.1" 200 -
192.168.2.156 - - [06/Feb/2018 18:50:04] code 404, message File not found
192.168.2.156 - - [06/Feb/2018 18:50:04] "HEAD /Packages.gz HTTP/1.1" 404 -
192.168.2.156 - - [06/Feb/2018 18:50:09] code 404, message File not found
192.168.2.156 - - [06/Feb/2018 18:50:09] "GET /./InRelease HTTP/1.1" 404 -
192.168.2.156 - - [06/Feb/2018 18:50:09] code 404, message File not found
192.168.2.156 - - [06/Feb/2018 18:50:09] "GET /./Release HTTP/1.1" 404 -
192.168.2.156 - - [06/Feb/2018 18:50:09] "GET /./Packages.bz2 HTTP/1.1" 200 -
192.168.2.156 - - [06/Feb/2018 18:52:20] code 404, message File not found
192.168.2.156 - - [06/Feb/2018 18:52:20] "GET /./CydiaIcon.png HTTP/1.1" 404 -
192.168.2.156 - - [06/Feb/2018 18:52:24] code 404, message File not found
192.168.2.156 - - [06/Feb/2018 18:52:24] "GET /./CydiaIcon.png HTTP/1.1" 404 -
192.168.2.156 - - [06/Feb/2018 18:52:25] code 404, message File not found
192.168.2.156 - - [06/Feb/2018 18:52:25] "GET /./InRelease HTTP/1.1" 404 -
192.168.2.156 - - [06/Feb/2018 18:52:25] code 404, message File not found
192.168.2.156 - - [06/Feb/2018 18:52:25] "GET /./Release HTTP/1.1" 404 -
192.168.2.156 - - [06/Feb/2018 18:52:25] "GET /./Packages.bz2 HTTP/1.1" 200 -
192.168.2.156 - - [06/Feb/2018 18:52:38] code 404, message File not found
192.168.2.156 - - [06/Feb/2018 18:52:38] "GET /CydiaIcon.png HTTP/1.1" 404 -
```

>* 假设URL地址是 192.168.2.189/cydia, 对于本地的cydia目录来说, 结构如下
```
Cydia/
   --Release
   --Packages
   --Packages.gz
   --Packages.bz2
   --CydiaIcon.png
   --debs/
     --xxxx1.deb
     --xxxx2.deb
```



>* Packages
```
deb包索引文件, 保存了各个deb包的control文件的信息,以及各个deb包的文件信息.
```

>* Packages.bz2
```
由Packages文件压缩而来, 命令行: bzip2 Packages
```

>* Release 
```
Cydia源配置文件,客户端通过下载此文件来读取cydia源信息;
Release文件几乎不用改, 只要准备好deb文件, 然后用dpkg-scanpackage命令生成Packages就可以了
```


### see also


>* [How to Host a Cydia™ Repository](http://www.saurik.com/id/7)

```
Creating a Repository
dpkg-scanpackages -m . /dev/null >Packages
 bzip2 Packages

```


>* [how-to-develop-jailbreak-apps-for-ios](http://blog.kernelpanic.im/2014/01/25/how-to-develop-jailbreak-apps-for-ios)

```
Cydia的source/repo基本上是Debian的APT repo, 只需要提供:

Release, repo描述文件
Packages|Pacakges.gz/bz2, repo的package清单
*.deb, 实际package文件

可以通过rsync同步到服务器上, 跑个文件下载的web服务就可以了---python -m SimpleHTTPServer 8088
```

- [hewigovens.github.com](https://github.com/hewigovens/hewigovens.github.com/wiki)
- [create-private-cydia-repo](https://blog.tylinux.com/2017/08/22/create-private-cydia-repo/)
```
control  文件 的Depends 项中添加 Tweak 的依赖，以逗号隔开。要注意：这里填写的是包名，比如 Open 这个工具，需要填写的是 com.conradkramer.open，可以先用Cydia安装，然后 dpkg -l 查看。
```
- [《iOS逆向工程》- 砸壳](https://blog.tylinux.com/2017/07/24/reverse-engineering-002/)
```
iOS的加壳操作则是由苹果进行的。这个壳的主要目的不是防止被逆向分析，而是一种DRM(数字版权管理)手段，它与iTunes Store中的其他资源一样，使用FairPlay(Wikipedia)进行加密，只能在特定账户的特定设备上运行。
```


