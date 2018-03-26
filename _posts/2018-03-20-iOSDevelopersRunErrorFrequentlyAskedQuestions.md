---
layout: post
title: iOSDevelopersRunErrorFrequentlyAskedQuestions
date: 2018-03-20
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

>* target specifies product type 'com.apple.product-type.tool', but there's no such product type for the 'iphoneos'

```
<!-- http://iosre.com/t/com-apple-product-type-tool/7060/4 -->

<!-- sudo ./configure-xcode-for-ios-development -->

use constant COMMAND_LINE_PRODUCT_TYPE => "com.apple.product-type.tool";


devzkndeMacBook-Pro:Scripts-Com.apple.product-type.tool解决方法 devzkn$ chmod +x ./configure-xcode-for-ios-
development

devzkndeMacBook-Pro:Scripts-Com.apple.product-type.tool解决方法 devzkn$  ./configure-xcode-for-ios-development
configure-xcode-for-ios-development must be run as root.
devzkndeMacBook-Pro:Scripts-Com.apple.product-type.tool解决方法 devzkn$ sudo  ./configure-xcode-for-ios-development

Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.12.sdk/usr/include/libxslt to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/libxslt.
Successfully updated '/Applications/Xcode.app/Contents/PlugIns/IDEiOSSupportCore.ideplugin/Contents/Resources/Embedded-Simulator.xcspec'.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.12.sdk/usr/include/libxslt to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/libxslt.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/crt_externs.h to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/crt_externs.h.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/MacErrors.h to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/MacErrors.h.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/mach/mach_types.defs to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/mach/mach_types.defs.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/mach/machine/machine_types.defs to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/mach/machine/machine_types.defs.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/mach/std_types.defs to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/mach/std_types.defs.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/objc/objc-class.h to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/objc/objc-class.h.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/objc/objc-runtime.h to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/objc/objc-runtime.h.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/objc/Protocol.h to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/objc/Protocol.h.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/readline/history.h to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/readline/history.h.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/readline/readline.h to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/readline/readline.h.
Successfully copied /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.3.sdk/usr/include/sqlite3_private.h to /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.3.sdk/usr/include/sqlite3_private.h.


```

>* framework not found IOKit



```
devzkndeMacBook-Pro:IOKit.framework devzkn$ sudo cp -r /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/Frameworks/IOKit.framework/Headers /Applications/Xcode9.1.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/Frameworks/IOKit.framework

```


### 正文



>* 'system' is unavailable: not available on iOS

```

/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS11.1.sdk/usr/include/stdlib.h:195:6: note: 'system' has been explicitly marked unavailable here
int      system(const char *) __DARWIN_ALIAS_C(system);
         ^
1 error generated.



 'system' is unavailable: not available on iOS


http://community.openfl.org/t/solved-system-not-available-on-ios-with-xcode-9-0/9683/2


<!-- d代替方案，从越狱源码找utils.h -->

  int rv;
    pid_t pd;
    
//    rv = posix_spawn(&pd, "/usr/bin/killall", NULL, NULL, (char **)&(const char*[]){ "killall", "-9", "SpringBoard", NULL }, NULL);
    
    char* argv[] = {(char *)cmd, NULL};
    
    rv = posix_spawn(&pd, "/usr/bin/killall", NULL, NULL,argv, NULL);
    
    waitpid(pd, NULL, 0);


<!-- run -->

    int run(const char *cmd) {
    char *myenviron[] = {
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/bin/X11:/usr/games",
        "PS1=\\h:\\w \\u\\$ ",
        NULL
    };
    
    pid_t pid;
    char *rawCmd = fixedCmd(cmd);
    char *argv[] = {"sh", "-c", (char*)rawCmd, NULL};
    int status;
    status = posix_spawn(&pid, "/bin/sh", NULL, NULL, argv, (char **)&myenviron);
    if (status == 0) {
        if (waitpid(pid, &status, 0) == -1) {
            perror("waitpid");
        }
    } else {
        printf("posix_spawn: %s\n", strerror(status));
    }
    free(rawCmd);
    return status;
}

```

>* 'launch.h' file not found


```

因为 /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/xpc

包含了，可以找launch.h 文件 devzkndeMacBook-Pro:include devzkn$ /Users/devzkn/Downloads/kevinsoftware/ios-Reverse_Engineering/mac-headers-master/usr/include/launchd.h 

devzkndeMacBook-Pro:include devzkn$ /Users/devzkn/Downloads/kevinsoftware/ios-Reverse_Engineering/mac-headers-master/usr/include/launchd.h /Users/devzkn/code/re/electra/basebinaries/jailbreakd/launch.h 

<!-- 最好的解决方案是使用没包含launch.h的文件 -->




```

>*  'xpc/xpc.h' file not found

```
<!-- 解决方案 -->

devzkndeMacBook-Pro:include devzkn$ sudo ln -s /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/xpc /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/include

/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/include


devzkndeMacBook-Pro:include devzkn$ cd /Applications/Xcode8.3.3.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/include
devzkndeMacBook-Pro:include devzkn$ ls -lrt xpc
total 232
-rwxr-xr-x@ 1 devzkn  admin  28756 Jan 16 17:06 connection.h
-rwxr-xr-x@ 1 devzkn  admin    672 Jan 16 17:06 endpoint.h
-rwxr-xr-x@ 1 devzkn  admin    730 Jan 16 17:06 debug.h
-rwxr-xr-x@ 1 devzkn  admin  73697 Jan 16 17:06 xpc.h
-rwxr-xr-x@ 1 devzkn  admin   2901 Jan 16 17:06 base.h


<!-- 删除上面建立的软连接 -->

rm -rf  /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/include

rm -fr xxxx 没有/ 这个是删除软链接

rm -fr xxxx/ 加了个/ 这个是删除文件夹

devzkndeMacBook-Pro:include devzkn$ sudo  rm -rf  /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/include/xpc

devzkndeMacBook-Pro:include devzkn$ sudo ln -s /Applications/Xcode8.3.3.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/include/xpc  /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/include/xpc


```


>* 'IOSurface/IOSurfaceRef.h' file not found

```
Frameworks  与PrivateFrameworks  冲突导致的没法找到

 /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/Frameworks/IOSurface.framework


#include <IOSurface/IOSurfaceRef.h>

导致的 
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/PrivateFrameworks/IOSurface.framework

<!-- 解决方法  -->

devzkndeMacBook-Pro:IOSurface.framework devzkn$ rm -rf /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/PrivateFrameworks/IOSurface.framework

devzkndeMacBook-Pro:IOSurface.framework devzkn$ ln -s /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/Frameworks/IOSurface.framework /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/PrivateFrameworks/IOSurface.framework

```


>* target specifies product type 'com.apple.product-type.tool', but there's no such product type for the 'iphoneos' platform

```



devzkndeMacBook-Pro:~ devzkn$ sudo xcode-select -switch /Applications/Xcode9.1.app


devzkndeMacBook-Pro:Scripts-Com.apple.product-type.tool解决方法 devzkn$ sudo  ./configure-xcode-for-ios-development
Successfully updated '/Applications/Xcode9.1.app/Contents/PlugIns/IDEiOSSupportCore.ideplugin/Contents/Resources/Embedded-Device.xcspec'.
Successfully copied /Applications/Xcode9.1.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.13.sdk/usr/include/libxslt to /Applications/Xcode9.1.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS11.1.sdk/usr/include/libxslt.
Successfully updated '/Applications/Xcode9.1.app/Contents/PlugIns/IDEiOSSupportCore.ideplugin/Contents/Resources/Embedded-Simulator.xcspec'.
Successfully copied /Applications/Xcode9.1.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.13.sdk/usr/include/libxslt to /Applications/Xcode9.1.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator11.1.sdk/usr/include/libxslt.
ditto: can't get real path for source '/Applications/Xcode9.1.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator11.1.sdk/usr/include/crt_externs.h'
Could not copy /Applications/Xcode9.1.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator11.1.sdk/usr/include/crt_externs.h to /Applications/Xcode9.1.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS11.1.sdk/usr/include/crt_externs.h: No such file or directory at ./configure-xcode-for-ios-development line 100.

devzkndeMacBook-Pro:Scripts-Com.apple.product-type.tool解决方法 devzkn$ ls -lrt /Applications/Xcode9.1.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.13.sdk/usr/include/libxslt
total 32
-rw-r--r--  1 root  wheel   8297 Jul 16  2017 xsltutils.h
-rw-r--r--  1 root  wheel   1396 Jul 16  2017 xsltlocale.h
-rw-r--r--  1 root  wheel   3426 Jul 16  2017 xsltexports.h
-rw-r--r--  1 root  wheel   3662 Jul 16  2017 xsltconfig.h
-rw-r--r--  1 root  wheel  57010 Jul 16  2017 xsltInternals.h
-rw-r--r--  1 root  wheel   1964 Jul 16  2017 xslt.h
-rw-r--r--  1 root  wheel   2593 Jul 16  2017 variables.h
-rw-r--r--  1 root  wheel   6328 Jul 16  2017 transform.h
-rw-r--r--  1 root  wheel   2268 Jul 16  2017 templates.h
-rw-r--r--  1 root  wheel   2652 Jul 16  2017 security.h
-rw-r--r--  1 root  wheel    892 Jul 16  2017 preproc.h
-rw-r--r--  1 root  wheel   1998 Jul 16  2017 pattern.h
-rw-r--r--  1 root  wheel   2019 Jul 16  2017 numbersInternals.h
-rw-r--r--  1 root  wheel   1666 Jul 16  2017 namespaces.h
-rw-r--r--  1 root  wheel   1155 Jul 16  2017 keys.h
-rw-r--r--  1 root  wheel   1840 Jul 16  2017 imports.h
-rw-r--r--  1 root  wheel   2006 Jul 16  2017 functions.h
-rw-r--r--  1 root  wheel   1834 Jul 16  2017 extra.h
-rw-r--r--  1 root  wheel   6903 Jul 16  2017 extensions.h
-rw-r--r--  1 root  wheel   2704 Jul 16  2017 documents.h
-rw-r--r--  1 root  wheel    930 Jul 16  2017 attributes.h

1) devzkndeMacBook-Pro:Scripts-Com.apple.product-type.tool解决方法 devzkn$ sudo xcode-select -switch /Applications/Xcode.app


2) devzkndeMacBook-Pro:Scripts-Com.apple.product-type.tool解决方法 devzkn$ sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer

3) sudo xcodebuild -license

By typing 'agree' you are agreeing to the terms of the software license agreements. Type 'print' to print them or anything else to cancel, [agree, print, cancel] agree

You can view the license agreements in Xcode's About Box, or at /Applications/Xcode.app/Contents/Resources/English.lproj/License.rtf

4）再执行 devzkndeMacBook-Pro:Scripts-Com.apple.product-type.tool解决方法 devzkn$ sudo  ./configure-xcode-for-ios-development



xcode9.1 对应的SDK crt_externs.h  已经没有这个文件了，可以尝试从Xcode8 拷贝

```


>* [framework not found IOKit](https://github.com/iOSHacking/knIOKit.framework)

```
#import <IOKit/IOKitLib.h>

意味着/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/Frameworks/IOKit.framework/Headers 目录下找不到IOKitLib.h



<!-- /Applications/Xcode9.1.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/Frameworks/IOKit.framework -->

devzkndeMacBook-Pro:IOKit.framework devzkn$ ls -lrt
total 0
lrwxr-xr-x  1 root  wheel   20 Nov 27 17:00 IOKit.tbd -> Versions/A/IOKit.tbd
drwxr-xr-x  4 root  wheel  128 Nov 27 17:06 Versions

devzkndeMacBook-Pro:IOKit.framework devzkn$ ls -lrt /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/Frameworks/IOKit.framework/Headers
total 400
-rwxr-xr-x@ 1 devzkn  admin  101469 Jun 23  2016 OSKext.h
-rwxr-xr-x@ 1 devzkn  admin    7526 Jun 23  2016 IOReturn.h
-rwxr-xr-x@ 1 devzkn  admin    6749 Jun 23  2016 IOKitKeys.h
-rwxr-xr-x@ 1 devzkn  admin    6038 Feb  3  2017 IOTypes.h
-rwxr-xr-x@ 1 devzkn  admin   68790 Feb  3  2017 IOKitLib.h
-rwxr-xr-x@ 1 devzkn  admin    4252 Feb  3  2017 OSMessageNotification.h


<!-- devzkndeMacBook-Pro:IOKit.framework devzkn$ sudo cp -r /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/Frameworks/IOKit.framework/Headers /Applications/Xcode9.1.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/Frameworks/IOKit.framework -->


```

>* Preparing to run Xcode Build Phase...
Signing /Users/devzkn/Library/Developer/Xcode/ with codesign... iPhone Developer: ambiguous (matches "iPhone Developer:  and "iPhone Developer:  ()" in /Users/devzkn/Library/Keychains/login.keychain-db)
Failed.

```
searching for "iPhone" in Keychain and removing all expired certificates,只留下一个有效的即可
```


###  dpkg

>* dpkg-deb: error: control directory has bad permissions 700 (must be >=0755 and <=0775)

```
devzkndeMacBook-Pro:DEBIAN devzkn$ ls -lrt
total 40
-rwxr-xr-x  1 devzkn  staff   153 Mar 22 14:03 prerm
-rwxr-xr-x  1 devzkn  staff  5480 Mar 22 16:17 postinst
-rwxr-xr-x  1 devzkn  staff  3026 Mar 22 16:17 preinst
-rw-r--r--  1 devzkn  staff   287 Mar 22 16:17 control

<!-- 需要修改为 -->
 -rwxr-xr-x  1 devzkn  staff   302 Mar 19 10:26 control

<!-- 解决方法 -->
 devzkndeMacBook-Pro:DEBIAN devzkn$ chmod +x control

 # devzkndeMacBook-Pro: devzkn$ chmod -R 0755 Package

```







### see also 


- [IOS11 中使用rocketbootstrap遇到的问题](http://iosre.com/t/ios11-rocketbootstrap/11367)

```
kill的话，有没有签这个？
<key>platform-application</key>
<true/>
```


- [IOS高级开发～开机启动&无限后台运行&监听进程](https://blog.csdn.net/bao990423420/article/details/14123445)

- [逆向分析网络协议 iOS 篇](https://mp.weixin.qq.com/s?__biz=MzIwMTYzMzcwOQ==&mid=403204191&idx=1&sn=514664cc05597f8b76730cbf9f3a57f5&scene=23&srcid=0309yRmQiN6spspIEAk8t6kO#rd)

```
<!--1、 Class-dump   -A             show implementation addresses -->

devzkndeMacBook-Pro:KNMNV5.4.0 devzkn$  /Users/devzkn/Downloads/class-dump --arch armv7 KNMN.decrypted -H  -o ./header

Class-Dump XApp -H -A -S -o headers/

<!-- devzkndeMacBook-Pro:~ devzkn$ class-dump --arch armv7 tmp -H -A -S -o ./header -->


+ (id)ret32bitString;	// IMP=0x00013887
+ (id)setupHttpSign;	// IMP=0x0001395f

地址空间布局随机化 (ASLR) 可防止对内存损坏缺陷的利用。
<!-- image list -o -f -->

<!--2、 br s -a "0x00000000000f8000+0x000000010024b99c" -->

执行 c 或 continue 让 app 跑起来，触发断点的地址，就会发现断点停下来的。

<!-- po $arg1 -->

 OC 方法 $arg1 就是 self

<!-- 3、断点变 Log -->
breakpoint command add 1

> po $arg1
> c
> DONE

```

- [iOS 10-10.1.1重新越獄與誤點清除所有內容和設定解決方法](https://mrmad.com.tw/ios10-1-1-re-jailbreak-device-after-erase-all-content-and-settings)

```
<!-- 「設定」>「一般」>「重置」>「清除所有內容和設定」所引起 -->
造成Cydia的資料夾/var/lib 和 /var/log/apt整個被刪除   确实没有了

cp -R /var/mobile/Media/Books/lib /var

mkdir /var/log/apt

<!-- 没有cydia app -->
devzkndeMacBook-Pro:var devzkn$ scp -r /Users/devzkn/code/re/yalu102/yalu102/bootstrap/Applications/Cydia.app usb2222:/Applications/

<!-- 缺少cydia 目录 -->
devzkndeMacBook-Pro:~ devzkn$ scp -r  usb2222:/usr/libexec/cydia ~/code/re
devzkndeMacBook-Pro:~ devzkn$ scp -r  ~/code/re/cydia usb2222:/usr/libexec

<!-- /usr/libexec/cydo 修改为正确权限 -->
-rwsr-sr-x 1 root wheel  52416 Mar  9  2016 cydo

06555

-r-sr-sr-x 1 root wheel 133808 Mar 21 14:17 cydo

<!-- uikit tool 中包含uicahe 命令 进行icon的刷新 -->
```

