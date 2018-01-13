---
layout: post
title: iOSOpenDevInstallFailed
date: 2018-01-10
tag: iOSre
site: https://zhangkn.github.io
---


###  iOSOpenDevInstallFailed（iOSOpenDev-1.6-2）


>* iOSOpenDevInstallFailed info （使用快捷键Command + L 查看）
```
Jan 10 11:33:54 devzkndeMacBook-Pro Installer[21643]: install:didFailWithError:Error Domain=PKInstallErrorDomain Code=112 "An error occurred while running scripts from the package “iOSOpenDev-1.6-2.pkg”." UserInfo={NSFilePath=./postinstall, NSURL=file://localhost/Users/devzkn/Downloads/iOSOpenDev-1.6-2.pkg#iodsetup.pkg, PKInstallPackageIdentifier=com.iosopendev.iosopendev162.iod-setup.pkg, NSLocalizedDescription=An error occurred while running scripts from the package “iOSOpenDev-1.6-2.pkg”.}
```

>* [解决安装问题](https://github.com/zhangkn/iosOpenDev)
```
# copy  iPhoneOS 开头的文件
devzkndeMacBook-Pro:zhangkn.github.io devzkn$ cd /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Specifications 
# 拷贝 iPhone Simulator ProductTypes 开头的文件
devzkndeMacBook-Pro:Xcode devzkn$ /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/Library/Xcode/Specifications 
# sudo  mkdir usr 创建bin 目录
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/usr/bin
```

### [错误2 :PrivateFrameworks apple已经将私有framework全部移走](https://github.com/zhangkn/knPrivateFrameworks)

>*  mkdir PrivateFrameworks
```
Jan 10 11:55:18 devzkndeMacBook-Pro installd[496]: ./postinstall: PrivateFramework directory not found: /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS11.1.sdk/System/Library/PrivateFrameworks
devzkndeMacBook-Pro:Library devzkn$ sudo  mkdir PrivateFramework
```

>*   Failed to download 错误3 
```
404:https://codeload.github.com/kokoabim/iOSOpenDev-Xcode-Templates/tar.gz/master%20to%20/var/folders/zz/zyxvpxvq6csfxvn_n0000000000000/T/iod-setup.cDoMQ34f/file.tar.gz
```

###  今天下载Xcode7.2 专门用于调用PrivateFramework和安装iOSOpenDev

```
devzkndeMacBook-Pro: devzkn$ xcodebuild -version
Xcode 7.2
Build version 7C68
```

>*  File not found iPhoneOSPackageTypes.xcspec
```
devzkndeMacBook-Pro:PrivateFrameworks devzkn$ mkdir mkdir /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Specifications/
devzkndeMacBook-Pro:PrivateFrameworks devzkn$ cp -R  /Applications/Xcode9.1.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Specifications/*  /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Specifications/
```

>* File not found: iPhone Simulator PackageTypes.xcspec
```
devzkndeMacBook-Pro:PrivateFrameworks devzkn$ cp -R  /Applications/Xcode9.1.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/Library/Xcode/Specifications/*  /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/Library/Xcode/Specifications/
```
>*  usr -> ../../../usr

```
devzkndeMacBook-Pro:PrivateFrameworks devzkn$ cp -R /Applications/Xcode9.1.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/usr /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/
```

### Q&A

>*  如果Xcode总是莫名其妙闪退 就删除xcodeproj/xcuserdata

>* Mac Xcode的首选项,偏好设置(Userdefault)文件地址 ~/Library/Preferences/com.apple.dt.Xcode.plist

>* [Alcatraz](https://github.com/alcatraz/Alcatraz)



### 参考

>* 官方参考
```
Follow — https://twitter.com/kokoabim
Download — http://iOSOpenDev.com
Wiki — https://github.com/kokoabim/iOSOpenDev/wiki

• The `iosod` command-line tool was placed in /opt/iOSOpenDev/bin. This is used by Xcode during Build Phases of projects created from iOSOpenDev templates. It also provides other various functions. To see all of its usages: Open Terminal and type `iosod --help`.

• The `iod-setup` command-line tool was placed in /opt/iOSOpenDev/bin. It provides ability to set up different Xcode and iOS SDK versions. To see all of its usages: Open Terminal and type `iod-setup`.

• The `iod-uninstall` command-line tool was placed in /opt/iOSOpenDev/bin. It provides ability to uninstall iOSOpenDev. To see all of its usages: Open Terminal and type `iod-uninstall`.
```

- [iOSOpenDev Installer Failures](https://github.com/kokoabim/iOSOpenDev/wiki/Troubleshoot)
- [iOSOpenDev](https://github.com/iFindTA/iOSOpenDev)
- [在xcode7下安装iOSOpendev，并使用iOSOpendev模板编译iOS9钩子](http://iosre.com/t/xcode7-iosopendev-iosopendev-ios9/1963)
- [Xcode 9 iOSOpenDev 安装及编译](http://bbs.iosre.com/t/xcode-9-iosopendev/10019/1)
- [https://developer.apple.com/download/more/ 下载Xcode7.2](https://developer.apple.com/download/more/) 
- [Inter-process communication](http://iphonedevwiki.net/index.php/Updating_extensions_for_iOS_7)

