---
layout: post
title: iphoneDaemonTool
date: 2017-12-24
tag: iOSre
site: https://zhangkn.github.io
---


### 前言
创建iPhone Daemon的例子


### [iphone/tool 的编写](https://github.com/zhangkn/knDaemonDemo)

>* iphone/tool 创建   
```
#$/opt/theos/bin/nic.pl 
NIC 2.0 - New Instance Creator
  [10.] iphone/tool
```
>* layout文件夹下创建LaunchDaemons配置文件

```
devzkndeMacBook-Pro:LaunchDaemons devzkn$ echo "<?xml version="1.0" encoding="UTF-8"?>
> <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
-bash: !DOCTYPE: event not found
> <plist version="1.0">
> <dict>
>         <key>KeepAlive</key>
>         <true/>
>         <key>Label</key>
>         <string>com.kn.knDaemonDemo</string>
>         <key>Program</key>
>         <string>/usr/bin/knDaemonDemo</string>
>         <key>RunAtLoad</key>
>         <true/>
> </dict>
> </plist>" > com.kn.knDaemonDemo.plist
```

```
devzkndeMacBook-Pro:LaunchDaemons devzkn$ cat *
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Program</key>
	<string>/usr/bin/knDaemonDemo</string>
	<key>RunAtLoad</key>
	<true/>
	<key>KeepAlive</key>
	<true/>
	<key>Label</key>
	<string>com.kn.knDaemonDemo</string>
</dict>
</plist>
```

在Theos建立的工程的根目录下建立一个 layout文件夹,这个文件夹就相当于设备的根目录了!在编译生成的deb包中,会自动放到对应的文件夹


>* 通过MakeFile 修改守护进程和它的配置文件由root：wheel来拥有

```
THEOS_DEVICE_IP=usb2222	#5C9

ARCHS = armv7 armv7s arm64
TARGET = iphone:latest:8.0

include $(THEOS)/makefiles/common.mk

TOOL_NAME = knDaemonDemo
knDaemonDemo_FILES = main.mm

include $(THEOS_MAKE_PATH)/tool.mk

after-install::
	install.exec "echo '' > /var/log/syslog"
	install.exec "chown root:wheel /usr/bin/$(TOOL_NAME)"
	install.exec "chown root:wheel  /Library/LaunchDaemons/com.kn.$(TOOL_NAME).plist"
	install.exec "reboot"
```
>* deploy
```
#!/bin/sh
cd `dirname $0` 
make clean
make package install
rm -f ./debs/*
exit 0
```
```
devzkndeMacBook-Pro:kndaemondemo devzkn$ deploy
==> Cleaning…
> Making all for tool knDaemonDemo…
==> Compiling main.mm (armv7)…
==> Linking tool knDaemonDemo (armv7)…
==> Compiling main.mm (armv7s)…
==> Linking tool knDaemonDemo (armv7s)…
==> Compiling main.mm (arm64)…
==> Linking tool knDaemonDemo (arm64)…
==> Merging tool knDaemonDemo…
==> Signing knDaemonDemo…
> Making stage for tool knDaemonDemo…
dpkg-deb: building package 'com.yourcompany.kndaemondemo' in './packages/com.yourcompany.kndaemondemo_0.0.1-2+debug_iphoneos-arm.deb'.
==> Installing…
Selecting previously unselected package com.yourcompany.kndaemondemo.
(Reading database ... 4263 files and directories currently installed.)
Preparing to unpack /tmp/_theos_install.deb ...
Unpacking com.yourcompany.kndaemondemo (0.0.1-2+debug) ...
Setting up com.yourcompany.kndaemondemo (0.0.1-2+debug) ...
install.exec "echo '' > /var/log/syslog"
install.exec "chown root:wheel /usr/bin/knDaemonDemo"
install.exec "chown root:wheel  /Library/LaunchDaemons/com.kn.knDaemonDemo.plist"
install.exec "reboot"
Connection to localhost closed by remote host.
```

>* 查看运行效果
```
iPhone:~ root# ps -e |grep knDaemonDemo
  160 ??         0:10.72 /usr/bin/knDaemonDemo
  510 ttys000    0:00.00 grep knDaemonDemo
```

### dylib、bundle或daemon，的位置

>* 基于CydiaSubstrate的dylib
```
iPhone:~ root# ls -l /Library/MobileSubstrate/DynamicLibraries/
total 3568
-rwxr-xr-x 1 mobile staff 1292720 Oct 31 15:01 AFlexLoader.dylib*
-rw-r--r-- 1 mobile staff      60 Oct 31 15:01 AFlexLoader.plist
```

>* Bundle
```
# AppStore App
/var/mobile/Containers/Bundle/Application/
# 系统自带的app
iPhone:~ root# ls -l /Applications
lrwxr-xr-x 1 root admin 32 Apr 23  2017 /Applications -> /var/stash/_.TvfJPY/Applications/
#Frameworks
iPhone:~ root# ls -l /System/Library/Frameworks
total 0
drwxr-xr-x 42 root wheel 1462 Oct 14  2014 AVFoundation.framework/
drwxr-xr-x 42 root wheel 1496 Oct 14  2014 AVKit.framework/
#PrivateFrameworks
iPhone:~ root# ls -l /System/Library/PrivateFrameworks
total 4
drwxr-xr-x  4 root wheel   306 Oct  7  2014 ABLE.framework/
drwxr-xr-x  4 root wheel   408 Oct  7  2014 ABLEModel.framework/
drwxr-xr-x  4 root wheel   170 Sep 13  2014 ACTFramework.framework/
```

>* daemon的配置文件
```
iPhone:~ root# ls -lrt /System/Library/LaunchDaemons/
total 32
-rwxr-xr-x 1 root wheel   328 Mar  6  2014 com.jensen.iRE.Startup.plist*
-rw-r--r-- 1 root wheel   281 Sep 13  2014 com.apple.absd.plist
# 通常用户自己的demo，就放在此目录，就比如本文的knDaemonDemo
iPhone:/Library root# ls -lrt /Library/LaunchDaemons
total 24
-rw-r--r-- 1 root wheel 847 Feb 15  2011 com.openssh.sshd.plist
-rw-r--r-- 1 root wheel 267 Feb  6  2017 com.rpetrich.rocketbootstrapd.plist
-rw-r--r-- 1 root wheel 446 Feb 16  2017 com.saurik.Cydia.Startup.plist
lrwxr-xr-x 1 root admin  61 Apr 23  2017 com.apple.mobile.installd.plist -> /System/Library/LaunchDaemons/com.apple.mobile.installd.plist
-rw-r--r-- 1 root wheel 779 Dec 14 00:54 re.frida.server.plist
-rw-r--r-- 1 root wheel 366 Dec 24 14:24 com.kn.knDaemonDemo.plist
# /Library/LaunchAgents
iPhone:/Library root# ls -lrt /Library/LaunchAgents
```


### knDaemonDemo/main.mm

>* 入口方法
```
int main(int argc, char **argv, char **envp) {
  return 0;
}
int main (int argc, const char * argv[]) {
  return 0;
}
```

### 参考
- [run-a-daemon-as-root-on-ios](http://iosre.com/t/run-a-daemon-as-root-on-ios/212)
- [Preferences_specifier_plist](http://iphonedevwiki.net/index.php/Preferences_specifier_plist)
- [Makefiles](http://www.gnu.org/software/make/manual/html_node/Makefiles.html)
- [Run a daemon (as root) on iOS](http://blog.linhere.com/archives/539.html)
- [jailbreakqa](http://www.jailbreakqa.com/)
- [Cydia Substrate](http://www.cydiasubstrate.com/)
- [iphonedevwiki](http://iphonedevwiki.net/index.php/Main_Page)
- [theiphonewiki](https://www.theiphonewiki.com/)
- [http://bbs.iosre.com/](http://bbs.iosre.com/)
- [security.ios-wiki.com](https://wizardforcel.gitbooks.io/ios-sec-wiki/content/index.html)
- [iossecurity](https://github.com/zhangkn/iossecurity)
- [Apple OpenSource](https://github.com/opensource-apple)

### see also
>* osx
![](/images/posts/{{page.title}}/osx.png)

>* iphone 
![](/images/posts/{{page.title}}/iphone.png)

