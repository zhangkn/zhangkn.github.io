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

### 参考
- [run-a-daemon-as-root-on-ios](http://iosre.com/t/run-a-daemon-as-root-on-ios/212)

