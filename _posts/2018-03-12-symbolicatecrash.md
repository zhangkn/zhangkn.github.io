---
layout: post
title: symbolicatecrash
date: 2018-03-12
tag: iOSre
site: https://zhangkn.github.io
---


### 前言


>* 查找symbolicatecrash 工具的目录

```
在Xcode目录下查找/Applications/Xcode.app/Contents
find . -name  'symbolicatecrash' -ls
/Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash

<!-- 便于以后的使用，最好采用ln ，这样避免拷贝，尤其是后台的工程师尤其注意这点-->
devzkndeMacBook-Pro:LatestBuild devzkn$ ln -s /Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash ~/bin

devzkndeMacBook-Pro:LatestBuild devzkn$ ls -lrt ~/bin
lrwxr-xr-x  1 devzkn  staff     111 Mar 12 17:28 symbolicatecrash -> /Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash
```

### 正文


>* symbolicatecrash
```
Error: "DEVELOPER_DIR" is not defined at /Users/devzkn/bin/symbolicatecrash line 69.
```
>* devzkndeMacBook-Pro:Contents devzkn$ export DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer
```
<!-- 想要永久生效，就配置到配置文件 -->
devzkndeMacBook-Pro:~ devzkn$ open -e  .bash_profile
export DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer
source ~/.bash_profile
```

>* 符号化
```
devzkndeMacBook-Pro:LatestBuild devzkn$ /Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash --dsym=/Users/devzkn/work/.git//LatestBuild/.dylib.dSYM /Users/devzkn/Desktop/SpringBoard-2018-03-12-162524.ips  --output=/Users/devzkn/Desktop/kn.crash
```

>* 效果

```
Exception Type:  EXC_BAD_ACCESS (SIGSEGV)
Exception Subtype: KERN_INVALID_ADDRESS at 0x0000000000000010
Triggered by Thread:  16

Thread 16 name:  com..
Thread 16 Crashed:
0   .dylib              	0x0000000109b639b8 0x109b54000 + 63928
1   .dylib              	0x0000000109b639a0 0x109b54000 + 63904
2   .dylib              	0x0000000109b63330 0x109b54000 + 62256
3   .dylib              	0x0000000109b631f0 0x109b54000 + 61936
4   .dylib              	0x0000000109b8084c 0x109b54000 + 182348
5   .dylib              	0x0000000109b812b4 0x109b54000 + 185012
6                       	0x0000000188225048 0x18811b000 + 1089608

<!-- 符号化之后的结果 -->
Thread 16 Crashed:
0   .dylib              	0x0000000109b639b8 +[ASWindowKNTool setupSwitchIpABUYUN1:] (ASWindowKNTool.m:289)
1   .dylib              	0x0000000109b639a0 +[ASWindowKNTool setupSwitchIpABUYUN1:] (ASWindowKNTool.m:287)
2   .dylib              	0x0000000109b63330 +[ASWindowKNTool setupSwitchIp:] (ASWindowKNTool.m:88)
3   .dylib              	0x0000000109b631f0 +[ASWindowKNTool setupCloseOpenWifi] (ASWindowKNTool.m:62)
4   .dylib              	0x0000000109b8084c -[ ] (.m:501)
5   .dylib              	0x0000000109b812b4 -[ ] (.m:644)
6                       	0x0000000188225048 __NSThreadPerformPerform + 340


```

### CrashReporter


>* find . -name  'SpringBoard*' -ls
```
12598152   92 -rw-rw-rw-   1 mobile   mobile      94165 Mar 12 16:25 ./private/var/mobile/Library/Logs/CrashReporter/SpringBoard-2018-03-12-162524.ips
```

>* /private/var/mobile/Library/Logs/CrashReporter
```
-rw-rw-rw- 1 mobile mobile 373035 Mar 12 16:09 panic-2018-03-12-160904.ips
-rw-rw-rw- 1 mobile mobile  93633 Mar 12 16:17 SpringBoard-2018-03-12-161753.ips
-rw-rw-rw- 1 mobile mobile  94099 Mar 12 16:18 SpringBoard-2018-03-12-161853.ips
-rw-rw-rw- 1 mobile mobile  93633 Mar 12 16:20 SpringBoard-2018-03-12-162025.ips
-rw-rw-rw- 1 mobile mobile  94015 Mar 12 16:21 SpringBoard-2018-03-12-162151.ips
-rw-rw-rw- 1 mobile mobile  94165 Mar 12 16:25 SpringBoard-2018-03-12-162524.ips
```


### see also 
- [How iCloud.com works?](https://github.com/prabhu/iCloud)
- [Lets study iTunes for fun and learning](https://github.com/prabhu/iTunes)
```
devzkndeMacBook-Pro:code devzkn$ scp -r  usb2222:/var/mobile/Library/Caches/sharedCaches/com.apple.iTunesStore.NSURLCache  ~/code/com.apple.iTunesStore.NSURLCache
NSFilePath=/var/mobile/Library/Caches/com.apple.storeservices/SSAppImageDatabaseCacheEntry
```
- [https://github.com/zhangkn/KNcom.apple.iTunesStore.NSURLCache.git](https://github.com/zhangkn/KNcom.apple.iTunesStore.NSURLCache.git)
- [deb](https://baike.baidu.com/item/deb)

```
一般有 5 个文件：
1、control，用了记录软件标识，版本号，平台，依赖信息等数据；

2、preinst，在解包data.tar.gz 前运行的脚本；postinst，在解包数据后运行的脚本；

3、prerm，卸载时，在删除文件之前运行的脚本；

4、postrm，在删除文件之后运行的脚本；在 Cydia 系统中，Cydia 的作者 Saurik 另外添加了一个脚本，extrainst_，作用与 postinst 类似。
```
