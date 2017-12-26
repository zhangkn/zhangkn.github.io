---
layout: post
title: the static analysis of iOS application 
date: 2017-01-03 
tag: iOSre
---
### 前言

　　使用iNalyzer进行静态分析并不适合头文件很多的app，因为头文件很多的话，就会导致大量的图表等分析文件，很消耗资源，这种情况可考虑使用hopper进行静态分析。
```
Running dot for graph 1175/42806
-rw-r--r--@ 1 devzkn  staff  47328989 Dec 10 13:35 static_2017_12_10-13_33_12.zip

```

   开篇就讲iNalyzer，是因为iNalyzer 能明显体现出静态分析的流程特点，以及它的安装包就包含了大量的基础工具：

   ```
   Class Dump Z
   Cycript
   Darwin CC Tools
   iNalyzer
   ldone
   UUID Generator
   otool.sh
   ```
### The goal of apps’ RE

>*  Inspect antire:在Mach-O的loadCommands 处通过阻止动态库注入保护。当然还有通过syscall 调用 ptrace来[anti调试](http://luoxianming.cn/2016/11/15/yueyutools3prevention/)
>*  Modify
>*  Verify usage of our (licensed) products
>*  Know your enemy


### Requirements（前期准备-基础的工具清单）

#### Device Tools 
>* 1、apt 0.6 transitional（用于自动从互联网的软件仓库中搜索、安装、升级、卸载软件,例如安装socat、git: apt-get install socat git ）
>* 2、dumpdecrypted.dylib or [clutch](https://github.com/KJCracks/Clutch)
>* 3、[cycript](http://www.cycript.org/) [frida](https://build.frida.re/frida/)
>* 4、class-dump-z （同系列工具：[class-dump](http://stevenygard.com/projects/class-dump/)、classdump-dyld、keychain_dumper、[swift-dump](https://github.com/zhangkn/swift-dump)）用来 dump 目标对象的 class 信息的工具
```
利用OC的runtime特性,将信息存储在Mach-O文件中的头文件信息取出来,并生成.h文件
```
>* 5、otool 、objdump(同系列工具：jtool ) -h -hv -vl  
>* 6、lipo、[debugserver](http://iphonedevwiki.net/index.php/Debugserver)、lldb、[toggle-pie](https://github.com/zhangkn/KNtoggle-pie)
>* 7、OpenSSH  2 (mac 同系列工具: [usbmuxd-1.0.8](https://cgit.sukimashita.com/usbmuxd.git/) ) ssh scp 都是建立在这基础之上
>* 8、 [AFlexLoader](https://github.com/swiftc-org/AFlexLoader)、[FLEX](https://github.com/Flipboard/FLEX)（mac 同系列工具： Reveal）
>* 9、Cydia Substrate (call/hook any method)
>* 10、[frida-server](https://build.frida.re/frida/)
>* 11、nm 旨在浏览mach-o可执行文件中的名称和符号  nm --help
>* 12、debugserver :Xcode附带的远程调试工具,它运行在iOS上,可以执行你在lldb(作为客户端)输入的指令同时返回执行结果到lldb上--即所谓的“远程调试”
```
在设备使用Xcode调试过app之后，debugserver才会被Xcode安装到iOS的“/Developer/usr/bin/”目录下。
```


#### Desktop tools

>* 1、Reveal、[interface-inspector](http://www.interface-inspector.com/)
>* 2、[MachOView](https://sourceforge.net/projects/machoview/)  is a visual Mach-O file browser ;MachOView就像otool和nm，但是有GUI，因此可以用户友好的方式浏览mach-o文件的结构,和编辑 Intel 和 ARM 二进制.
>* 3、[Hopper](https://www.hopperapp.com/) ;(同系列产品： [radare2](https://github.com/radare/radare2)、 [IDA_Pro_v7.0(MacOS)_and_Hex-Rays_Decompiler_(ARMx64,ARM,x64,x86).zip](https://down.52pojie.cn/Tools/Disassemblers/))
<br>hopper 支持伪代码
```
int __ZL24_logosLocalCtor_c81e728diPPcS0_(int arg0, int * * arg1, int * * arg2) {
    r0 = [@"251453530" longLongValue];
    r0 = r0 + 0x1;
    r0 = NSLog(@"Real group NO: %lld", r0);
    return r0;
}
```
![](/images/posts/iOS_Wifilist/assemblylanguage.png)

```
“程序员使用编译器、汇编器和链接器中的一个或几个创建可执行程序。为了回溯编程过程（或对程序进行逆向工程），我们使用各种工具来撤销汇编和编译过程。毫不奇怪，这些工具就叫做反汇编器和反编译器，名副其实。反汇编器撤销汇编过程，因此，我们可以得到汇编语言形式的输出结果（以机器语言作为输入）。反编译器则以汇编语言甚至是机器语言为输入，其输出结果为高级语言。”
```

>* 4、Tweak 工具：Theos（是一个越狱开发工具包，安装简单,Logos语法简单）、iOSOpenDev、MonkeyDev（支持CocoaPods) 可以用来开发iPhone tool 、iPhone tweak 
>* 5、codesign --help 关于Code Signature的全面信息
>* ollydbg 动态追踪工具,通过修改改汇编代码来生成机器码。


标准命令行工具
>* lldb(Low Level Debugger)是内置于Xcode中的动态调试工具.
>* otool是一个用于浏览mach-o可执行文件的控制台工具;
>* nm旨在浏览mach-o可执行文件中的名称和符号;
>* codesign可以提供关于Code Signature的全面信息
```
devzkndeMacBook-Pro:Payload devzkn$ codesign -d -v MKNoon
```
>* ldid 用来给deb包签名的工具,
```
usage: ldid -S[entitlements.xml] <binary>
   ldid -e MobileSafari
   ldid -S cat
   ldid -Stfp.xml gdb
```
>* [optool](https://github.com/zhangkn/KNBin/blob/master/optool) 依赖注入工具,可将dylib注入到二进制文件中
>* strings “实用工具专门用于提取文件中的字符串内容”
```
# -a     This option causes strings to look for strings in all sections of the object file (including the (__TEXT,__text) section.
devzkndeMacBook-Pro:knMokssnossV5.4.0 devzkn$ strings MJoKoKn.decrypted |grep password
configureUserNameEntry:passwordEntry:
passwordTextFieldFrame
passwordPlaceholder
passwordBackgroundView
#-t formatWrite each string preceded by its byte offset from the start of the file.  The format shall be dependent on the single character used as the format option-argument: 查看“所发现的每一个字符串的文件偏移量信息”
```
>*  ndisasm “流式反汇编器”
```
devzkndeMacBook-Pro:knMokssnossV5.4.0 devzkn$ ndisasm -u Mokkon.dec*
00000000  CAFEBA            retf 0xbafe
00000003  BE00000002        mov esi,0x2000000
00000008  0000              add [eax],al
0000000A  000C00            add [eax+eax],cl
0000000D  0000              add [eax],al
```

>* plutil


Jailbroken iOS device 

 


### iNalyzer 安装

>* 1、进入 cydia 添加源: http://appsec-labs.com/cydia/
>* 2、搜索 iNalyzer 并安装

### iNalyzer 的使用

#### 查看可解密的App

>* 1、iNalyzer.sh

```
iPhone:/Applications/iNalyzer.app root# /Applications/iNalyzer.app/iNalyzer.sh

Usage: /Applications/iNalyzer.app/iNalyzer.sh [list | clean | version | help]
Usage: /Applications/iNalyzer.app/iNalyzer.sh [info | ipa | sandbox | dynamic | nslog | cycript] <bundleGUID>
Usage: /Applications/iNalyzer.app/iNalyzer.sh [static] [auto | class-dump-z | classdump-dyld] <bundleGUID>

```

>* 2、 使用 list 查询bundleGUID,在使用ipa static 进行静态分析

```
iPhone:/Applications/iNalyzer.app root# /Applications/iNalyzer.app/iNalyzer.sh ipa 2B559443-6CEE-4731-AA3B-7E587BE67219    
appGuid=2B559443-6CEE-4731-AA3B-7E587BE67219
appDir=/var/mobile/Containers/Bundle/Application/2B559443-6CEE-4731-AA3B-7E587BE67219/KNknMokssnoss.app
appName=KNknMokssnoss
appBundleId=com.KNknMokssnoss.KNknMokssnoss
clutchAppId=3
mainExecutable=KNknMokssnoss
appExecutables=KNknMokssnoss
appExecutablesFullPath=/var/mobile/Containers/Bundle/Application/2B559443-6CEE-4731-AA3B-7E587BE67219/KNknMokssnoss.app/KNknMokssnoss
isEncrypted=1
appSandbox=/private/var/mobile/Containers/Data/Application/CC955B7F-E976-425F-ADB3-7F52D18B4EEF
Preparing folders for IPA...
Creating IPA file...
OUTPUTFILE:/var/root/Documents/iNalyzer/2B559443-6CEE-4731-AA3B-7E587BE67219/ipa/KNknMokssnoss.ipa

```
开始静态分析

```
iPhone:/Applications/iNalyzer.app root# /Applications/iNalyzer.app/iNalyzer.sh static 2B559443-6CEE-4731-AA3B-7E587BE67219

Preparing folders for static...
Preparing doxigen folders...
Binary is encyrpted. Decrypting..
clutchTmpFile=/var/tmp/clutch/3AD5851D-99B4-4BCB-A644-91B6B05EA4EB
Coping the decrypted binaries to the decryptedBinaries folder
===Binary analysis
Fetching binary info
Fetching entitlements
Looking for interesting symbols
Dumping strings
Dumping classes
Running class-dump with 'auto' mode
Trying classdump-dyld...
===Analyze interfaces...
===Decode mobile provisioning files...
Mobile provision file was not found
===Decode plist files...
Converted 1 files to XML format
OUTPUTFILE:/var/root/Documents/iNalyzer/2B559443-6CEE-4731-AA3B-7E587BE67219/static_2017_12_09-14_24_57.zip


```

>* 3、scp 到Mac

```
iPhone:/Applications/iNalyzer.app root# scp /var/root/Documents/iNalyzer/2B559443-6CEE-4731-AA3B-7E587BE67219/static_2017_12_09-14_24_57.zip devzkn@192.168.2.186://Users/devzkn/decrypted/knMokssnossV5.4.0

```
ps :iPhone SSH Mac 的前提是mac setremotelogin on

```
devzkndeMacBook-Pro:.ssh devzkn$ sudo systemsetup -setremotelogin off

devzkndeMacBook-Pro:.ssh devzkn$ sudo systemsetup -setremotelogin on
devzkndeMacBook-Pro:.ssh devzkn$ sudo systemsetup -getremotelogin
Remote Login: On
```
当然也可以使用界面操作
```
在 Mac 上，打开“共享”偏好设置（选取苹果菜单 >“系统偏好设置”，然后点按“共享”）。

1、选择“远程登录”。

2、选择“远程登录”时，还会启用安全 FTP（SFTP）服务。

3、指定哪些用户可以登录：
```

>* 4、执行 doxMe.sh 脚本
uses DoxyGen and GraphViz to display the information in a much more presentable format

在 Mac 端：先安装Doxygen 后安装Graphviz，否则在osx10.13.1 (17B48)会找不到oxygen
```
brew install graphviz
devzkndeMacBook-Pro:taoketool devzkn$ brew install doxygen

```
查看信息 
```
devzkndeMacBook-Pro:static_2017_12_09-14_24_57 devzkn$ ./doxMe.sh
Searching for include files...
Searching for example files...
Searching for images...
Searching for dot files...
Searching for msc files...
Searching for dia files...
Searching for files to exclude
Searching INPUT for files to process...
```
通过 index.html 我们可以直观的查看到Class Hierarchy 、Interface recovery等信息

```
Binary info
►Entitlements
▼Interfaces
 *.nib files
 View Controllers
►Plist files
►Strings - KNknMokssnoss
►Symbols analysis - Memory functions
```









### 小总结

iNalyzer 体现了 Image parsing

>* 1、Unpacking Fat (Universal) binaries
>* 2、Mach-O
>* 3、Symbols
>* 4、Function starts
>* 5、Objective-C runtime (__objc_*) Swift virtual tables

Statistics: vulnerabilities

```
NSLog 
Deprecated 
Reflection
Weak cipher
No SSL 
Weak SSL 
Pasteboard
```


Future work

```
– Deep analysis (dataflow, etc.)
– Less false positives
– Objective-C/Swift decompilation
```

### otool、 toggle-pie、lipo、usbmuxd-1.0.8使用例子


#### [去掉PIE](https://github.com/zhangkn/KNtoggle-pie)

如果你觉得 MH_PIE flag 对你的后续LLDB调试带来麻烦的话，可以先 disable ASLR 。

```
iPhone:~ root#  toggle-pie
Usage: toggle-pie <path_to_binary>
iPhone:~ root# ps -e |grep /var*
 1057 ttys004    0:00.01 grep /var
iPhone:~ root# otool -hv /var/mobile/Containers/Bundle/Application/2B559443-6CEE-4731-AA3B-7E587BE67219/
/var/mobile/Containers/Bundle/Application/2B559443-6CEE-4731-AA3B-7E587BE67219/.app/:
Mach header
      magic cputype cpusubtype  caps    filetype ncmds sizeofcmds      flags
   MH_MAGIC     ARM          9  0x00     EXECUTE    75       7500   NOUNDEFS DYLDLINK TWOLEVEL WEAK_DEFINES BINDS_TO_WEAK PIE
iPhone:~ root# toggle-pie /var/mobile/Containers/Bundle/Application/2B559443-6CEE-4731-AA3B-7E587BE67219/.app/
[STEP 1] Backing up the binary file...
[STEP 1] Binary file successfully backed up to /var/mobile/Containers/Bundle/Application/2B559443-6CEE-4731-AA3B-7E587BE67219/.app/.bak

[STEP 2] Flip the 32-bit PIE...
Original Mach-O header: cefaedfe0c00000009000000020000004b0000004c1d000085802100
Original Mach-O header flags: 85802100
Flipping the PIE...
New Mach-O header flags: 85800100
[STEP 2] Successfully flipped the 32-bit PIE.

[STEP 3] Flip the 64-bit PIE...
Original Mach-O header: cffaedfe0c00000100000000020000004b000000982000008580210000000000
Original Mach-O header flags: 85802100
Flipping the PIE...
New Mach-O header flags: 85800100
[STEP 3] Successfully flipped the 64-bit PIE.
iPhone:~ root# otool -hv /var/mobile/Containers/Bundle/Application/2B559443-6CEE-4731-AA3B-7E587BE67219/.app/
/var/mobile/Containers/Bundle/Application/2B559443-6CEE-4731-AA3B-7E587BE67219/.app/:
Mach header
      magic cputype cpusubtype  caps    filetype ncmds sizeofcmds      flags
   MH_MAGIC     ARM          9  0x00     EXECUTE    75       7500   NOUNDEFS DYLDLINK TWOLEVEL WEAK_DEFINES BINDS_TO_WEAK

```



#### otool

otool 是Xcode编译器LLVM的Toolchains，class-dump 都是基于‘otool -ov‘ 实现的。
[class_dump_z](https://code.google.com/archive/p/networkpx/wikis/class_dump_z.wiki)、[classdump-dyld](https://github.com/limneos/classdump-dyld) 这些工具以后再介绍。

>* 1、这里先简单说下：如何查看LC_ENCRYPTION_INFO

```
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
lrwxr-xr-x  1 root  wheel        10 Nov 27 17:04 otool -> llvm-otool
iPhone:~ root# otool -l /var/mobile/Containers/Bundle/Application/239A0B7E-AA8C-4E43-873D-16254934321A/T4iPhone.app/T4iPhone | grep -A 4 LC_ENCRYPTION_INFO

```

 >* 2、使用otool 可以快速查看flags，你还可以选择hopper进行查看

```
             __mh_execute_header:
00004000         struct __macho_header {                                        ; DATA XREF=sub_756b0+62, sub_78252+8, sub_81f9c+128, sub_85f82+66, sub_86bd0+42, sub_87976+724, sub_89532+80, sub_8da68+64, switch_table_8ff1a+12252, switch_table_8ff1a+12270, sub_b1794+216, …
                     0xfeedface,                          // mach magic number identifier

```


####  lipo

thin binary
```
iOS8-jailbreak:~ root# lipo -thin armv7 DamnVulnerableIOSApp -output DVIA32

```

####  scp 结合usbmuxd-1.0.8  的例子
```
./tcprelay.py -t 22:2222
$ scp -P2222 debugserver root@localhost:/tmp/

```
usbmuxd-1.0.8 还可以用于lldb debugserver,以及SSH

>* 1、应用场景1:通过USB连接 来使用SSH到iOS设备
把本地2222端口转发到iOS的22端口
```
alias relay22='python ~/Downloads/kevin－software/ios-Reverse_Engineering/usbmuxd-1.0.8\ 2/python-client/tcprelay.py  -t 22:2222'
alias sshusb='ssh root@localhost -p 2222'
```


>* 2、应用场景2:debugserver的开启与LLDB的连接
```
iPhone:/usr/bin root# debugserver *:12345 -a "KNWeChat"
```
把本地12345端口转发到iOS的12345端口
```
alias relay12345='python ~/Downloads/kevin－software/ios-Reverse_Engineering/usbmuxd-1.0.8\ 2/python-client/tcprelay.py  -t  12345:12345'
devzkndeMacBook-Pro:~ devzkn$ lldb
(lldb) process connect connect://localhost:12345
```



 

### Q&A


>* 1、cydia提示i wasn't able to locate file for the berkeleydb package

     
```
- 打开cydia。
- 点选下面的变更栏。
- 点选左上角的的刷新键。等待所有的packages加载完就可以了。(在此期间什么也别做)
```

<br>

参考资源：[Tampering and Reverse Engineering on iOS](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x06c-Reverse-Engineering-and-Tampering.md)
[iOS Anti-Reversing Defenses](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x06j-Testing-Resiliency-Against-Reverse-Engineering.md)
[DVIA](http://damnvulnerableiosapp.com/)
[Frida :Inject JavaScript to explore native apps](https://github.com/frida) [frida.re](https://www.frida.re/)
[Radare2](http://rada.re/r/)
[frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump)
[iOS instrumentation without jailbreak](https://www.nccgroup.trust/au/about-us/newsroom-and-events/blogs/2016/october/ios-instrumentation-without-jailbreak/)
[node-applesign](https://github.com/nowsecure/node-applesign)
[Kim Jong Cracks](https://github.com/KJCracks)
[ios-application-security-part-15-static-analysis-of-ios-applications-using-inalyzer](http://highaltitudehacks.com/2013/09/17/ios-application-security-part-15-static-analysis-of-ios-applications-using-inalyzer/)
- [introducing-the-ios-reverse-engineering-toolkit](https://www.veracode.com/blog/2014/03/introducing-the-ios-reverse-engineering-toolkit)
- [opensource](http://www.opensource.apple.com/)
- [optool](https://www.jianshu.com/p/8236249edd35)
- [Objective-C相关的iOS逆向理论基础](https://www.jianshu.com/p/5041795b7d23)
- [iOS_逆向_Theos](https://www.jianshu.com/p/d27cebabc282)

<br>

