---
layout: post
title: dumpdecrypted
date: 2017-12-16
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

本文重点推荐使用[frida-ios-dump-master](https://github.com/zhangkn/frida-ios-dump)，而非dumpdecrypted.dylib。


### frida-ios-dump-master 

使用frida-ios-dump-master 只需先用frida-ps查看applications Name ，之后执行dump.py 即可在dump.py 目录下生成砸壳之后的ipa包。

Frida环境的搭建可以看下[这篇文章](https://zhangkn.github.io/2017/12/frida/#gsc.tab=0)

### dumpdecrypted.dylib 
>*dumpdecrypted的原理：

```
把自己通过DYLD_INSERT_LIBRARIES这个环境变量注入到已经通过系统加载器解密的 mach-o文件(因此要求程序是运行状态)，再把解密后的内存数据 dump出来--并没有破解 appstore的加密算法
```

砸壳的步骤：

>*1、找到app二进制文件对应的目录； 
```
ps -e|grep /var/mobile/Container*
```
>*2、找到app document对应的目录； 
```
cycript -p 
```
```
cy# [[NSFileManager defaultManager] URLsForDirectory:NSDocumentDirectory inDomains:NSUserDomainMask][0]
```
>*3、将砸壳工具dumpdecrypt.dylib拷贝到ducument目录下； //目的是为了获取写的权限 
```
devzkndeMacBook-Pro:dumpdecrypted-master devzkn$ scp ./dumpdecrypted.dylib root@192.168.2.212://var/mobile/Containers/Data/Application/91E7D6CF-A3D3-435B-849D-31BB53ED185B/Documents
```

>*4、砸壳；利用环境变量 DYLD_INSERT_LIBRARY 来添加动态库dumpdecrypted.dylib
```
DYLD_INSERT_LIBRARIES=dumpdecrypted.dylib /var/mobile/Containers/Bundle/Application/01ECB9D1-858D-4BC6-90CE-922942460859/KNWeChat.app/KNWeChat
```
第一个path为dylib，目标path 为app二进制文件对应的目录


>* iPhone:~ root# find / -name "*.*" | xargs grep "DYLD_INSERT_LIBRARIES" > ~/text.text
```
iPhone:~ root# cat  text.text
Binary file /Developer/Library/PrivateFrameworks/DTDDISupport.framework/libViewDebuggerSupport.dylib matches
Binary file /Developer/usr/lib/libBacktraceRecording.dylib matches
Binary file /Library/Frameworks/CydiaSubstrate.framework/Libraries/SubstrateLauncher.dylib matches
Binary file /Library/Frameworks/CydiaSubstrate.framework/Libraries/SubstrateLoader.dylib matches
Binary file /System/Library/Caches/com.apple.xpcd/xpcd_cache.dylib matches
/System/Library/LaunchDaemons/com.apple.searchd.plist:		<key>no_DYLD_INSERT_LIBRARIES</key>
```

### 可以dump混编的

>* [可以dump混编的](https://github.com/zhangkn/KNBin/blob/master/swiftOCclass-dump)
```
devzkndeMBP:bin devzkn$ swiftOCclass-dump  --arch arm64 /Users/devzkn/decrypted/AppStoreV10.2/Payload/AppStore.app/AppStore -H -o  /Users/devzkn/decrypted/AppStoreV10.2/head
```


### see also
>* [KNas10.2Head](https://github.com/zhangkn/KNas10.2Head/tree/master/as10.2/head)
>* [NSFileManager](http://iosre.com/t/ios-igrimace/448)
- [防止tweak依附，App有高招；破解App保护，tweak留一手](http://bbs.iosre.com/t/tweak-app-app-tweak/438)
```
三种情况下，DYLD_环境变量会被dyld无视，分别是：
1. 可执行文件被setuid或setgid了；
2. 可执行文件含有__RESTRICT/__restrict这个section；
3. 可执行文件被签了某个entitlements。
```
- [MachOView](https://github.com/gdbinit/MachOView)
- [anti-DYLD_INSERT_LIBRARIES](http://geohot.com/e7writeup.html)
- [Sam Marshall总结的逆向工程学习资源](http://bbs.iosre.com/t/sam-marshall/92)
- [Reverse Engineering Resources](https://pewpewthespells.com/re.html)
- [Samantha Demi](https://pewpewthespells.com/)
- [Blocking Code Injection on iOS and OS X](https://pewpewthespells.com/blog/blocking_code_injection_on_ios_and_os_x.html)
- [pewpewthespells](https://pewpewthespells.com/ramble.html)

- [Getting runtime information about blocks](https://github.com/zhangkn/CTObjectiveCRuntimeAdditions)
- [Block-ABI-Apple.html](http://clang.llvm.org/docs/Block-ABI-Apple.html)
- [UIDebuggingInformationOverlay](http://ryanipete.com/blog/ios/swift/objective-c/uidebugginginformationoverlay/)