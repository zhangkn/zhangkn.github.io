---
layout: post
title: iOSobfuscation
date: 2018-04-12
tag: anti
site: https://zhangkn.github.io
---


### 前言


### 字符串替换


- [confuse.sh原版](https://gist.github.com/zhangkn/dc2f5e5a883cc506d20fc6285775c313)

- [confuse.sh原版：自动提取以.m或.h结尾的文件的方法名称](https://github.com/iOSobfuscation/HSKConfuse/blob/master/HSKConfuse/Resource/confuse.sh)

```
屏蔽系统的方法名，暂行的策略是:将自己定义的方法名全部添加一个前缀

https://github.com/iOSobfuscation/HSKConfuse/blob/master/HSKConfuse/Resource/confuse.sh

例如采用 ：只匹配以“hsk_”字符串前缀的方法名，而不是匹配字符‘h’ 、‘s’、‘k’、‘_’； 以后自己的代码要养成这个习惯
```

- [gem install codeobscure，基于confuse.sh 进行优化](https://github.com/iOSobfuscation/codeobscure)

```

<!-- sudo gem install codeobscure -->
You don't have write permissions for the /Library/Ruby/Gems/2.3.0 directory.
目前还不错的方案。----项目太复杂了，目前暂时混淆类名。

```

- [关键字提取之后进行md5加密zmconfuse.sh:不是用宏定义的方式，而是直接替换](https://github.com/iOSobfuscation/ZMConfuse/blob/master/zmconfuse.sh)--- 灵活性不太好

>* [基于class-dump的扩展:用class-dump扫描出编译后的类名、方法名、属性名并做替换；iOS-Class-Guard works alongside LLVM Obfuscator](https://github.com/Polidea/ios-class-guard)

```
简单基础的混淆方法:https://github.com/Polidea/ios-class-guard/blob/master/README.md

看来只适合app,不适合dylib
```

>* [LLVM Obfuscator 原版GitHub地址](https://github.com/HikariObfuscator/Hikari)

```
https://github.com/iOSHacking/Hikari 
```


### 正文

### codeobscure

>*  sudo  gem install codeobscure

```

Fetching: colorize-0.8.1.gem (100%)
Successfully installed colorize-0.8.1
Fetching: random-word-2.1.0.gem (100%)
Successfully installed random-word-2.1.0
Fetching: codeobscure-0.1.5.3.gem (100%)
Successfully installed codeobscure-0.1.5.3
Parsing documentation for colorize-0.8.1
Installing ri documentation for colorize-0.8.1
Parsing documentation for random-word-2.1.0
Installing ri documentation for random-word-2.1.0
Parsing documentation for codeobscure-0.1.5.3
Installing ri documentation for codeobscure-0.1.5.3
Done installing documentation for colorize, random-word, codeobscure after 0 seconds
3 gems installed

 which codeobscure
/usr/local/bin/codeobscure


```


>*  codeobscure --help


```

Usage: obscure code for object-c project
    -o, --obscure XcodeprojPath      obscure code
    -l, --load path1,path2,path3     load filt symbols from path  ---相当于白名单
    -r, --reset                      reset loaded symbols
    -f, --fetch type1,type2,type3    fetch and replace type,default type is [c,p,f].c for class,p for property,f for function
    -i, --ignore XcodeprojPath       create a ignore file, you can write your filt symbols in it.eg:name,age ...
    -t, --type replaceType           obscure type = [r,w,c] ,r: random w: random words c: custom replace rule


```


>* Example

```

sudo codeobscure -o /Users/mac/Downloads/Examples/Messenger.xcodeproj  -l /Users/mac/Downloads/Examples/Pods,/Users/mac/Downloads/Examples/Download


    ../codeObfuscation.h


"过滤的[NSClassFromString]数据：SBWiFiManager"

请在 Prefix.pch中#import "codeObfuscation.h"

配置完成!

请直接运行项目，如果项目中出现类似: `+[LoremIpsum PyTJvHwWNmeaaVzp:]: unrecognized selector sent to class`。在codeObfuscation.h中查询PyTJvHwWNmeaaVzp并删除它!


```

### ios-class-guard


>*  install ios-class-guard 

```
devzkndeMBP:_site devzkn$ brew install --HEAD ios-class-guard

==> Cloning https://github.com/Polidea/ios-class-guard.git
Cloning into '/Users/devzkn/Library/Caches/Homebrew/ios-class-guard--git'...
remote: Counting objects: 631, done.
remote: Compressing objects: 100% (497/497), done.
remote: Total 631 (delta 111), reused 386 (delta 54), pack-reused 0
Receiving objects: 100% (631/631), 409.73 KiB | 15.00 KiB/s, done.
Resolving deltas: 100% (111/111), done.
==> Checking out branch master
==> xcodebuild -workspace ios-class-guard.xcworkspace -scheme ios-class-guard -configuration Release SYMROOT=build PREFIX=/usr/local/Cellar/ios-class-guard/HEAD-4199b01 ONLY_ACTIVE_ARCH=YES
🍺  /usr/local/Cellar/ios-class-guard/HEAD-4199b01: 5 files, 894.6KB, built in 49 seconds

<!-- devzkndeMBP:_site devzkn$ which ios-class-guard -->
/usr/local/bin/ios-class-guard


<!-- devzkndeMBP:_site devzkn$ ios  + 按下tab 键，可以自动补全显示-->
ios-class-dump   ios-class-guard  ios-hooker.py    ioscan           iosnoop          iosod            iostat           

```


>* Usage

```
devzkndeMBP:_site devzkn$ ios-class-guard
ios-class-guard 0.8 (64 bit) [based on class-dump 3.5]
Usage: ios-class-guard [options] <mach-o-file>

  where options are:
        -F <class>        specify class filter for symbols obfuscator (also protocol))
        -i <symbol>       ignore obfuscation of specific symbol)
        --arch <arch>     choose a specific architecture from a universal binary (ppc, ppc64, i386, x86_64, armv6, armv7, armv7s, arm64)
        --list-arches     list the arches in the file, then exit
        --sdk-ios         specify iOS SDK version (will look for /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS<version>.sdk
                          or /Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS<version>.sdk)
        --sdk-mac         specify Mac OS X version (will look for /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX<version>.sdk
                          or /Developer/SDKs/MacOSX<version>.sdk)
        --sdk-root        specify the full SDK root path (or use --sdk-ios/--sdk-mac for a shortcut)
        -X <directory>    base directory for XIB, storyboards (will be searched recursively)
        -P <path>         path to project.pbxproj of Pods project (located inside Pods.xcodeproj)
        -O <path>         path to file where obfuscated symbols are written
        -m <path>         path to symbol file map (default value symbols.json)
        -c <path>         path to symbolicated crash dump
        --dsym <path>     path to dSym file to translate
        --dsym-out <path> path to dSym file to translate

```

>* have to add Pre compiled header file manually before obfuscation.

```
<!-- File -> New -> File -> iOS -> Other -> PCH File -->

MyProject-Prefix.pch

<!-- At the target's Build Settings, in Apple LLVM - Language section, set Prefix Header to your PCH file name -->

<!-- At the target's Build Settings, in Apple LLVM - Language section, set Precompile Prefix Header to YES. -->



```


>* How to use it?(integrate iOS Class Guard in a project)

```
<!-- A few steps are required to integrate iOS Class Guard in a project. -->

<!-- 1) Download obfuscate_project in to your project root path. -->

curl -o obfuscate_project https://raw.githubusercontent.com/Polidea/ios-class-guard/master/contrib/obfuscate_project && chmod +x obfuscate_project

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3036  100  3036    0     0   3851      0 --:--:-- --:--:-- --:--:--  3847



<!-- 2) Update the project file, scheme and configuration name. -->

https://github.com/zhangkn/KNBin/blob/5a6298c4dbc10da2c4a589f09575ad5c3a09a11b/obfuscate_project


更新 obfuscate_project 内的project file、scheme 和 configuration name


# Build project to fetch symbols
[[ -n "$WORKSPACE" ]] && XCODEBUILD_OPTS="$XCODEBUILD_OPTS -workspace $WORKSPACE"
[[ -n "$PROJECT" ]] && XCODEBUILD_OPTS="$XCODEBUILD_OPTS -project $PROJECT"
[[ -n "$SCHEME" ]] && XCODEBUILD_OPTS="$XCODEBUILD_OPTS -scheme $SCHEME"
[[ -n "$CONFIGURATION" ]] && XCODEBUILD_OPTS="$XCODEBUILD_OPTS -configuration $CONFIGURATION"

----- # 设置-scheme  Debug、Release
----- 内的project file： # WORKSPACE=YourWorkspace.xcworkspace PROJECT=YourProject.xcodeproj



<!-- 3) Do bash obfuscate_project every time when you want to obfuscate your project. -->

 It should be done every release. Store the json file containing symbol mapping so you can get the original symbol names in case of a crash.

4) Build, test and archive your project using Xcode or other tools.

The presented way is the simplest one. You can also add additional target that will automatically regenerate the symbols map during compilation.




```

>* [obfuscate_project](https://github.com/zhangkn/KNBin/blob/5a6298c4dbc10da2c4a589f09575ad5c3a09a11b/obfuscate_project)

```
# General build options 最好抽取出来
# WORKSPACE=YourWorkspace.xcworkspace
PROJECT=YourProject.xcodeproj
SCHEME=YourScheme    ## 设置工程名称
CONFIGURATION=Release  ## 设置-scheme  Debug、Release
SDK=7.1


<!-- 0)  xcodebuild -->
echo_and_run xcodebuild $XCODEBUILD_OPTS \
    clean build \
    -derivedDataPath build
    OBJROOT=build/ \
    SYMROOT=build/

<!-- 1) ios-class-guard -->
    echo_and_run ios-class-guard \
        $CLASS_GUARD_OPTS_SDK \
        $CLASS_GUARD_OPTS \
        -O "$SYMBOLS_FILE" \
        "$app/$TARGET"

SYMBOLS_FILE="$PWD/symbols.h"

CLASS_GUARD_OPTS="-i IgnoredSymbol -F !ExcludedClass"

# In case of using Xcode >= 6 and SDK >= 8
CLASS_GUARD_OPTS_SDK="--sdk-root /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator$SDK.sdk"



<!-- #ls -lrt /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/ -->

lrwxr-xr-x  1 root  wheel   19 Nov 27 17:00 iPhoneSimulator11.1.sdk -> iPhoneSimulator.sdk


```

>* ./obfuscate_project

```

WARNING: This will wipe all your not commited changes in your repository
Press Ctrl-C to Cancel or Enter to proceed.


git reset --hard

git clean -fdx

Removing xxx.xcodeproj/xcuserdata/
Removing xxx/switchip/
Removing obfuscate_project


xcodebuild -project xx.xcodeproj -scheme xx -configuration Release -sdk iphonesimulator11.1 clean build -derivedDataPath build
User defaults from command line:
    IDEDerivedDataPathOverride = /Users/devzkn/work/.git/xx/build

Build settings from command line:
    SDKROOT = iphonesimulator11.1

=== CLEAN TARGET xx OF PROJECT xx WITH CONFIGURATION Release ===

Check dependencies

Clean.Remove clean build/Build/Intermediates.noindex/PrecompiledHeaders/xx-Prefix-fbixccmptffkxzamlvgvfhnhahyq/xx-Prefix.pch.pch
    builtin-rm -rf /Users/devzkn/work/.git/xx/build/Build/Intermediates.noindex/PrecompiledHeaders/xx-Prefix-fbixccmptffkxzamlvgvfhnhahyq/-Prefix.pch.pch

Clean.Remove clean build/Build/Products/Release-iphonesimulator
builtin-rm -rf /build/Build/Products/Release-iphonesimulator

** CLEAN SUCCEEDED **

Write auxiliary files

write-file Script-.sh
chmod 0755  Script-.sh

write-file 
/bin/mkdir -p
PhaseScriptExecution Run\
 /bin/sh -c 
 Preparing to run Xcode Build Phase for Logos Processor...
 Logos Processor:
 Xcode Build Phase for Logos Processor complete.
ProcessPCH
 normal i386 objective-c com.apple.compilers.llvm.clang.1_0.compiler
 ProcessPCH++ 
 CompileC tion.o  tion.m normal i386 objective-c com.apple.compilers.llvm.clang.1_0.compiler




** BUILD SUCCEEDED **

find . -name *-Prefix.pch -exec sed -i .bak 1i\
#import "/Users/devzkn//./symbols.h"
 {} ;


Congratulations! Obfuscation completed. You can now build, test and archive Your project using Xcode, Xctool or Xcodebuid...



<!-- 其中的#SDK=7.1 和#PROJECT=YourProject.xcodeproj 可以注射掉 -->




<!-- # Obfuscate project  进支持app？？？ -->
appsNumber=0;
while read app
do
    if ((appsNumber > 0))
    then
        echo ""
        echo ""
        echo "You cannot use this tool when there is more than one .app file in products. Otherwise, only the first one will be used for obfuscation."
        echo ""
        echo ""
        exit 1
    fi
    ((appsNumber+=1))

    TARGET=$(basename "$app" .app)
    echo "Obfuscating $TARGET in $app..."
    echo_and_run ios-class-guard \
        $CLASS_GUARD_OPTS_SDK \
        $CLASS_GUARD_OPTS \
        -O "$SYMBOLS_FILE" \
        "$app/$TARGET"
done < <(find build/ -name '*.app')

echo ""
echo ""
echo "Congratulations! Obfuscation completed. You can now build, test and archive Your project using Xcode, Xctool or Xcodebuid..."
echo ""
echo ""


```

### see also


- [iOS开发系列--代码混淆](https://www.jianshu.com/p/c9073d1e06b9)

```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk/System/Library/Frameworks



https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2FTMWu%2FTMConfuse

<!-- https://github.com/iOSobfuscation/TMConfuse -->

<!-- 确定以上几项后，找到仓库根目录的Confuse.py文件，使用以下命令行模板运行： -->
<!-- 使用方式 -->
1) clone本仓库； devzkndeMBP:re devzkn$ git clone git@github.com:iOSobfuscation/TMConfuse.git
drwxr-xr-x   10 devzkn  staff   320 Apr 14 14:32 CodeConfuse
-rw-r--r--    1 devzkn  staff  6565 Apr 14 14:32 README.md
drwxr-xr-x  103 devzkn  staff  3296 Apr 14 14:32 System_Frameworks_iOS
drwxr-xr-x    5 devzkn  staff   160 Apr 14 14:32 TMConfuse.xcodeproj
drwxr-xr-x   11 devzkn  staff   352 Apr 14 14:32 TMConfuse



2) python3

3) 你首先需要确定以下几项：

- 当前项目编译环境的SDK库头文件目录: /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk/System/Library/Frameworks

- 你需要混淆的代码的目录；

- 你不需要混淆的代码的目录；

- 你需要提取关键字做排除混淆的目录；（例如Pod仓库、第三方头文件）

- Swift代码目录；（理论上不会扫描替换，可以用于排除桥接文件）

- 输出文件目录；脚本运行后会产生多个log文件，以及最终需要使用到的混淆头文件；




4) 仓库根目录的Confuse.py文件，使用以下命令行模板运行：

python3 Confuse.py \
-i   必须  你需要混淆的代码的目录，可以是多个目录，以`,`分隔 \
-s  可选  当前项目编译环境的SDK库头文件目录，可以是多个目录，以`,`分隔 \
-e 你不需要混淆的代码的目录，Swift代码目录，可以是多个目录，以`,`分隔 \
-c 你需要提取关键字做排除混淆的目录，可以是多个目录，以`,`分隔 \
-k 可选，用于存放需要过滤的key(增加内容)
-o 必须  输出文件目录:输出关键字、日志以及最后生成的混淆头文件的目录


5) 运行后会在你指定的输出目录下产生一份Confuse.h文件， 



devzkndeMBP:CodeConfuse devzkn$ cat  start.sh
#!/usr/bin/env bash
python3 /Users/wuaming/Desktop/TMConfuse/CodeConfuse/Confuse.py \
-i /Users/wuaming/Desktop/TMConfuse/TMConfuse \
-s /Users/wuaming/Desktop/TMConfuse/System_Frameworks_iOS \
-k /Users/wuaming/Desktop/TMConfuse/CodeConfuse/ignoreKey.txt \
-o /Users/wuaming/Desktop/TMConfuse/CodeConfuse


<!-- ls -lrt ../System_Frameworks_iOS  -->

devzkndeMBP:CodeConfuse devzkn$ ls -lrt ../System_Frameworks_iOS
total 8
drwxr-xr-x  5 devzkn  staff  160 Apr 14 14:32 ARKit.framework
drwxr-xr-x  6 devzkn  staff  192 Apr 14 14:32 AVFoundation.framework
drwxr-xr-x  5 devzkn  staff  160 Apr 14 14:32 AVKit.framework
drwxr-xr-x  6 devzkn  staff  192 Apr 14 14:32 Accelerate.framework
drwxr-xr-x  5 devzkn  staff  160 Apr 14 14:32 Accounts.framework
drwxr-xr-x  5 devzkn  staff  160 Apr 14 14:32 AdSupport.framework
drwxr-xr-x  4 devzkn  staff  128 Apr 14 14:32 AddressBook.framework
drwxr-xr-x  5 devzkn  staff  160 Apr 14 14:32 AddressBookUI.framework
drwxr-xr-x  5 devzkn  staff  160 Apr 14 14:32 AssetsLibrary.framework

devzkndeMBP:CodeConfuse devzkn$ ls -lrt  /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk/System/Library/Frameworks
但是这个目的通常需要root 权限才能修改修改

/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/PrivateFrameworks


ls -lrt /Users/devzkn/code/re/TMConfuse/System_Frameworks_iOS


<!-- /Users/devzkn/code/re/TMConfuse/CodeConfuse/start.sh -->


Start scanning System identifiers...
Complete scan System identifiers...

Start writing System identifiers into Dict File, File fullpath is /Users/devzkn/work/.git/kniosre/kniosre/system_identifiers.txt
Complete write System identifiers into Dict File

Start scanning assign need deal files' identifiers...
Start excluding system identifiers...
Start excluding ignore keys...
Start writing need deal files' identifiers into Dict File, File fullpath is /Users/devzkn/work/.git/kniosre/kniosre/user_identifiers.txt
Complete write need deal files' identifiers into Dict File

Start creating confuse file, file fullpath is /Users/devzkn/work/.git/kniosre/kniosre/Confuse.h
Complete create confuse file

You can browse run logs in file /Users/devzkn/work/.git/kniosre/kniosre/confuse_log.log


<!-- 新增的文件 -->
    Confuse.h
    confuse_log.log
    system_identifiers.txt
    user_identifiers.txt



<!-- devzkndeMacBook-Pro:Developer devzkn$ du -sh * |sort -hr -->

devzkndeMacBook-Pro:/ devzkn$ du -sh * |sort -hr
du: Library/Application Support/Apple/ParentalControls/Users: Permission denied
du: Library/Application Support/Apple/AssetCache/Data: Permission denied

/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/PrivateFrameworks



```


- [iOS Class Guard github译文及使用经验总结](http://elijahdou.github.io/blog/2015/07/08/ios-class-guard.html)


```
https://github.com/iOSobfuscation/ios-class-guard
https://github.com/Polidea/ios-class-guard/blob/master/README.md


<!-- 工作原理 -->

只对应用程序的编译版本起作用（工具的脚本文件会首先编译项目源码，得到应用文件，之后使用class-dump处理应用文件）

1) 每次执行混淆操作，都会生成一个唯一的symbol map。之后这个map会格式化成一个C的宏定义 头文件，并包含到 .pch文件中。

 在编译期间内，所有定义在头文件内的symbol都会用对应的生成的不同的符号替换并编译。

2) iOS-Class-Guard也提供了对cocoapod库的混淆。这个工具会 根据用户提供的pods路径 自动遍历所有列出的target 并 查找 .xcconfig文件和要修改的预编译头文件路径。然后添加预先生成的头文件到库 .pch头文件，并更新target的.xcconfig文件中的头文件的search path参数。

3) 注意 iOS-Class-Guard不混淆system symbol，所有如果在自定义类中的某些属性和方法与system symbol有相同的名字，则不会被混淆。

<!-- 集成iOS-Class-Guard到项目中需要以下几步： -->




```


- [安全、逆向](https://github.com/ShannonChenCHN/iOSLevelingUp/issues/28)

```
https://github.com/noteblogpost/iOSLevelingUp

```



- [A simple category to hide sensitive strings from appearing in your binary](https://github.com/UrbanApps/UAObfuscatedString)

```


<!-- 可对 Xcode 项目工程所有的 objective-c 文件内包含的明文进行加密混淆，提高逆向分析难度。 -->

混淆（或者加密）硬编码的明文字符串: 加密会在编译开始时自动处理 https://github.com/danleechina/mixplaintext

<!-- 比如 ptrace 反调试等（不过据说已经可以很容易被绕过） -->

// see http://iphonedevwiki.net/index.php/Crack_prevention for detail
static force_inline void disable_gdb() {
#ifndef DEBUG
    typedef int (*ptrace_ptr_t)(int _request, pid_t _pid, caddr_t _addr, int _data);
#ifndef PT_DENY_ATTACH
#define PT_DENY_ATTACH 31
#endif
    // this trick can be worked around,
    // see http://stackoverflow.com/questions/7034321/implementing-the-pt-deny-attach-anti-piracy-code
    void* handle = dlopen(0, RTLD_GLOBAL | RTLD_NOW);
    ptrace_ptr_t ptrace_ptr = dlsym(handle, [@"".p.t.r.a.c.e UTF8String]);
    ptrace_ptr(PT_DENY_ATTACH, 0, 0, 0);
    dlclose(handle);
#endif
}


https://github.com/ShannonChenCHN/iOSLevelingUp/issues/28

```



- [Simple Objective-C obfuscator for Mach-O executables](https://github.com/Polidea/ios-class-guard)

```

<!-- brew install ios-class-guard -->
 the utility is installed in /usr/local/bin.

1) brew install --HEAD ios-class-guard  ---To install bleeding edge version:



<!-- How to use it? -->

1)Download obfuscate_project in to your project root path.

curl -o obfuscate_project https://raw.githubusercontent.com/Polidea/ios-class-guard/master/contrib/obfuscate_project && chmod +x obfuscate_project

2)Update the project file, scheme and configuration name.

3) Do bash obfuscate_project

4) Build, test and archive your project using Xcode or other tools.

 You can also add additional target that will automatically regenerate the symbols map during compilation.


<!-- Pre compiled header file -->

 iOS-Class-Guard will try to add generated symbols header (symbols.h) to your project's *.pch file


<!-- To add *.pch file to your project follow the steps below: -->

1) Create PCH file in your project's root directory. In Xcode go to File -> New -> File -> iOS -> Other -> PCH File. To ensure backward compatibility iOS-Class-Guard will be looking for a file matching the *-Prefix.pch mask, as an example MyProject-Prefix.pch

2) At the target's Build Settings, in Apple LLVM - Language section, set Prefix Header to your PCH file name.

3) At the target's Build Settings, in Apple LLVM - Language section, set Precompile Prefix Header to YES.



<!-- Example -->

git clone https://github.com/Polidea/ios-class-guard-example ios-class-guard-example
cd ios-class-guard-example
make compile


Here is class-dump for non-obfuscated sources: https://github.com/Polidea/ios-class-guard-example/tree/master/SWTableViewCell-no-obfuscated.xcarchive/Headers

What it will look like when you use iOS Class Guard: https://github.com/Polidea/ios-class-guard-example/tree/master/SWTableViewCell-obfuscated.xcarchive/Headers


<!-- Command Line Options -->

Usage: ios-class-guard [options] <mach-o-file>

  where options are:

        -F <class>     specify class filter for symbols obfuscator (also protocol))  ：   -F '!APH*' -F '!MC*'  


        -i <symbol>    ignore obfuscation of specific symbol)： -i 'deflate' -i 'curl_*' This will not obfuscate symbols named deflate and symbols that start with curl_*.


        --arch <arch>  choose a specific architecture from a universal binary (ppc, ppc64, i386, x86_64, armv6, armv7, armv7s, arm64)
        --list-arches  list the arches in the file, then exit
        --sdk-ios      specify iOS SDK version (will look for /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS<version>.sdk
                       or /Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS<version>.sdk)
        --sdk-mac      specify Mac OS X version (will look for /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX<version>.sdk
                       or /Developer/SDKs/MacOSX<version>.sdk)
        --sdk-root     specify the full SDK root path (or use --sdk-ios/--sdk-mac for a shortcut)

        -X <directory> base directory for XIB, storyboards (will be searched recursively)：-X SWTableView/Xib


        -P <path>      path to project.pbxproj of Pods project (located inside Pods.xcodeproj) ： -P Pods/Pods.xcodeproj/project.pbxproj


        -O <path>      path to file where obfuscated symbols are written  -----重要参数： -O SWTableView/symbols.h

        -m <path>      path to symbol file map (default value symbols.json)： -m release/symbols_1.0.0.json


        -c <path>      path to symbolicated crash dump ： -c crashdump -m symbols_1.0.0.json






<!-- Reversing obfuscation in dSYMs -->


Build Phases/Run script example


if [ -f "$PROJECT_DIR/symbols.json" ]; then
/usr/local/bin/ios-class-guard -m $PROJECT_DIR/symbols.json --dsym $DWARF_DSYM_FOLDER_PATH/$DWARF_DSYM_FILE_NAME --dsym-out $DWARF_DSYM_FOLDER_PATH/$DWARF_DSYM_FILE_NAME
fi

# Another invocations eg.: ./Crashlytics.framework/run <Crashlytics secret #1> <Crashlytics secret #2>


Manual usage example


ios-class-guard -m symbols.json --dsym MyProject_obfuscated.app.dSYM --dsym-out MyProject_unobfuscated.app.dSYM



iOS-Class-Guard works alongside LLVM Obfuscator: https://github.com/obfuscator-llvm/obfuscator. However, this has not been tested.






<!-- https://github.com/iOSobfuscation/ios-class-guard -->

 a command-line utility for obfuscating Objective-C class, protocol, property and method names. It was made as an extension for class-dump.

 make your application harder to read by an attacker

 blog: http://www.polidea.com/#!heartbeat/blog/Protecting_iOS_Applications

<!-- Do I need It? -->

This utility makes code analyzing and runtime inspection more difficult, which can be referred to as a simple/basic method of obfuscation.


<!-- How does it work? -->

 It reads the Objective-C portion of Mach-O object files. It parses all classes, properties, methods and i-vars defined in that file adding all symbols to the list. 

This file is then included in .pch file

1) iOS Class Guard also provides support for obfuscating CocoaPods libraries.







```

- [ZMConfuse](https://github.com/kongcup/ZMConfuse)

```
一个命令行脚本。用于对使用Objective-C为开发语言的应用进行代码混淆。混淆的内容包括文件名、类名、协议名、属性名、函数名。

https://github.com/kongcup/ZMConfuse/blob/master/zmconfuse.sh



```

- [Binary Format](https://gloxec.github.io/2017/05/03/Binary%20Format/)


- [iOS应用安全之代码混淆实现篇](https://blog.csdn.net/zm53373581/article/details/49120895)

```
https://github.com/kongcup/ZMConfuse

<!-- 该脚本大致使用的工具如下：vi、grep、sed、find、awk、cut、sort、uniq、cat、md5等。 -->

<!--一、 对confuse.sh 的提取代码进行解释 -->

<!-- 属性名-->

grep -r -h -I  ^@property $ROOTFOLDER  $EXCLUDE_DIR --include '*.[mh]' | sed "s/(.*)/ /g"  | sed "s/<.*>//g" |sed "s/[,*;]/ /g" | sed "s/IBOutlet/ /g" |awk '{split($0,s," ");print s[3];}'|sed "/^$/d" | sort |uniq  


<!-- 类名 -->

grep -h -r -I  "^@interface" $ROOTFOLDER  $EXCLUDE_DIR  --include '*.[mh]' | sed "s/[:(]/ /" |awk '{split($0,s," ");print s[2];}'|sort|uniq  


<!-- 函数名 -->

grep -h -r -I  "^[-+]" $ROOTFOLDER $EXCLUDE_DIR --include '*.[mh]' |sed "s/[+-]//g"|sed "s/[();,: *\^\/\{]/ /g"|sed "s/[ ]*</</"|awk '{split($0,b," ");print b[2];}'| sort|uniq |sed "/^$/d"|sed "/^init/d"  

<!-- 去除空行 -->

sed "/^$/d"  


<!-- 前缀规则：忽略 -->

将函数中的以init开头的函数去除： sed "/^init/d"  




```


- [iOS代码加固](http://fighting300.com/2017/04/01/iOS-code-obfuscate/)

```

<!-- iOS代码加密的几种方式 -->
<!-- 一、字符串处理： -->
<!-- 1.字符串加密:对需要加密的字符串加密，并保存加密后的数据，再在使用字符串的地方插入解密算法 -->

简单的加密算法可以把NSString转为byte或者NSData的方式，还可以把字符串放到后端来返回.

<!-- 2.符号混淆 : 将类名、方法名、变量名替换为无意义符号，提高应用安全性-->

1) 提取方法名称：https://github.com/iOSobfuscation/HSKConfuse/blob/master/HSKConfuse/Resource/confuse.sh---grep sed awk

#过滤规则包括 ：hsk_ 即方法名前缀
grep -h -r -I  "^[-+]" $CONFUSE_FILE  --include '*.[mh]' |sed "s/[+-]//g"|sed "s/[();,: *\^\/\{]/ /g"|sed "s/[ ]*</</"| sed "/^[ ]*IBAction/d"|awk '{split($0,b," "); print b[2]; }'| sort|uniq |sed "/^$/d"|sed -n "/^hsk_/p" >$STRING_SYMBOL_FILE


2） 脚本搜索遍历了代码目录下的需要混淆的关键字（函数名）： 过滤规则包括

grep -h -r -I  "^[-+]" $ROOTFOLDER $EXCLUDE_DIR --include '*.[mh]' |sed "s/[+-]//g"|sed "s/[();,: *\^\/\{]/ /g"|sed "s/[ ]*</</"| sed "s/<.*>//g" |awk '{split($0,b," ");print b[2];}'| sort|uniq |sed "/^$/d"|sed "/^init/d" >filter_fun.txt





<!-- 3.C重写, 最好采用结构体的方式存储方法;把函数名隐藏在结构体里,以函数指针成员的形式存储 -->

<!-- C重写 的例子： https://zhangkn.github.io/2017/12/antiDebugger/ -->
<!--1） KNUtil.h -->

@interface KNUtil : NSObject
/**
 把函数名隐藏在结构体里，以函数指针成员的形式存储。
 编译后，只留了下地址，去掉了名字和参数表，提高了逆向成本和攻击门槛.
 */
typedef struct _util {
    void (*cign)(char *kns[],unsigned int kncount, const char *knkey, unsigned char *knput);
}CNtKNil_t ;
#define SharedUtilStruct ([KNUtil sharedUtil])//提供给外围的接口
+ (CNtKNil_t *)sharedUtil;


<!--2）KNUtil.m -->

#include <stdio.h>
#include <string.h>
#include <stdlib.h>
//留给读者自己实践

<!-- 3）外围调用 -->

SharedUtilStruct->cign(key ,count,knkey, knput);



<!-- 二.代码逻辑（主谓宾定状补）混淆 -->


https://github.com/obfuscator-llvm/obfuscator

<!-- https://github.com/HikariObfuscator/Hikari   ：字符串加密 、指令替换、间接跳转、函数封装、 反class-dump -->

https://github.com/HikariObfuscator/Hikari/releases

https://naville.gitbooks.io/hikaricn/content/Intergrating-with-Xcode.html

https://tvoskit.cn/index.php/ios/19.html

inurl:Hikari site:https://github.com

inurl:Hikari  intext:LLVM  site:https://github.com    2018.01.18   

https://github.com/iOSHacking/Hikari

https://github.com/OrzRys/Hikari

https://github.com/jingjinghack/Hikari


https://naville.gitbooks.io/hikaricn/content/
https://legacy.gitbook.com/book/naville/hikaricn/details


<!--  三.加固SDK -->


```


- [antiDebugger](https://zhangkn.github.io/2017/12/antiDebugger/)

```

<!-- 只留下了地址，去掉了名字和参数表，让他们无从下手(copy from 念茜) -->

typedef struct _util {  
  BOOL (*doTrade)(void);  
  BOOL (*makeDeal)(void);  
void (*transferMoney)(NSString *password);  
 }FTradeUtil_t ;  

#define FTradeUtil ([_FTradeUtil sharedUtil])  

@interface _FTradeUtil : NSObject  

+ (FTradeUtil_t *)sharedUtil;  

@end


```



- [iOS安全攻防（二十三）：Objective-C代码混淆](https://blog.csdn.net/yiyaaixuexi/article/details/29201699)

```

<!-- 混淆的常规思路 -->

1）花代码花指令，即随意往程序中加入迷惑人的代码指令
2）易读字符替换


<!-- Objective-C的方法名混淆 -->
在Build Phrase 中设定在编译之前进行方法名的字符串替换----- 在开发时一直保留清晰可读的程序代码，方便自己


<!-- 混淆的方法 -->

1）方法名混淆其实就是字符串替换，有2个方法:一个是#define，一个是利用tops？？？？？。

2） #define的方法有一个好处，就是可以把混淆结果合并在一个.h中，在工程Prefix.pch的最前面#import这个.h。不导入也可以编译、导入则实现混淆。


<!-- 我的混淆工具 -->

https://gist.github.com/zhangkn/dc2f5e5a883cc506d20fc6285775c313


<!-- 操作步骤 -->

1） mv confuse.sh your_proj_path/

2） 打开Xcode，修改XXX-Prefix.ch ，添加混淆头文件:


#ifdef __OBJC__  
    #import <UIKit/UIKit.h>  
    #import <Foundation/Foundation.h>  
    //添加混淆作用的头文件（这个文件名是脚本confuse.sh中定义的）  
    #import "codeObfuscation.h"  //由confuse.sh 生成的替换文件
#endif  



3）在工程Build Phase中添加执行脚本操作，执行confuse.sh脚本，


4）创建函数名列表func.list，写入待混淆的函数名，----- 手写太麻烦，最好采用自动查找自定义的方法名来进行替换，而无需手动配置 func.list 文件
http://www.jianshu.com/p/0d42e5c6361c  ---- 改进confuse.sh脚本（grep sed sort uniq ）

```

- [iOS代码混淆----自动](https://www.jianshu.com/p/0d42e5c6361c)


```


func.list函数列表抽取，和宏定义是脚本自动完成，不需要手动抽函数和手动宏定义

<!-- 自动混淆，方法名字不能一个一个的添加到func.list中，所以方法名只能从.m和.h文件中抽取 -->

1) 屏蔽系统的方法名，暂行的策略是:将自己定义的方法名全部添加一个前缀

<!-- 改进的脚本逻辑 -->

1） 程序每次预处理，都就会执行confuse.sh,从.m和.h文件中按照"一定的规则"抽取需要混淆的函数名 ，全部写到func.list中

2）  再从func.list中逐行提取函数名进行宏定义,宏定义使用随机字符串,然后写到codeObfuscation.h文件中 --- 保持原来的处理流程



https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fhousenkui%2FHSKConfuse

ps另送一份：iOS 脚本打包 傻瓜版,无需改变配置 github地址:https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fhousenkui%2FautoComplie



```


- [ios自动打包生成ipa到桌面 傻瓜版本](https://github.com/housenkui/autoComplie)

```
放到项目根目录下，然后直接执行 shell脚本

<!--1、 echo  "开始编译workspace...." -->


xcodebuild  -workspace $projectName.xcworkspace -scheme $projectName  -configuration $buildConfig clean build SYMROOT=$buildAppToDir

<!-- xcodebuild -scheme xxxx项目名称 -quiet -configuration $options -->

<!--2、 echo  "开始编译target...." -->

xcodebuild  -target  $projectName  -configuration $buildConfig clean build SYMROOT=$buildAppToDir

<!-- 3、echo "开始打包$projectName.app成$projectName.ipa....." -->

xcrun -sdk iphoneos -v PackageApplication $appDir/$projectName.app  -o $outPath/$projectName.ipa




```


- [ Objective-C 代码混淆:遍历项目，过滤不能进行宏定义的名称](https://github.com/iOSobfuscation/codeobscure)

```

Usage: obscure code for object-c project
-o, --obscure XcodeprojPath      obscure code
-l, --load path1,path2,path3     load filt symbols from path
-r, --reset                      reset loaded symbols
-f, --fetch type1,type2,type3    fetch and replace type,default type is [c,p,f].c for class,p for property,f for function
-i, --ignore XcodeprojPath       create a ignore file, you can write your filt symbols in it.eg:name,age ...



<!-- v0.1.5 添加NSClassFromString以及setValue:forKeyPath:等字段过滤，进一步优化运行出现不识别方法崩溃的情况。 -->

添加替换方式[-t]，包括：随机字符 ， 随机单词，自定义替换(目前自定义替换暂未实现，将在0.1.6中实现)

  生成结果示例：	
  r ：#define getHeight ZbgTCtOTDmEazebk
  w ：#define getHeight nodulatedBasutoland   --- 随机单词直接加 -t w 就可以了,不需要你手动输入随机单词的

```


- [CodeConfuseTool](https://github.com/macosunity/CodeConfuseTool)

```
<!-- C++/Objective C 项目代码混淆工具，主要功能是混淆类名和函数名，使用C++和Qt开发 -->

1） 其中qt 要占用20G的磁盘大小，且采用的是原理是：class、property、function 混淆其实就是字符串替换，使用#define； 于是就放弃了。

2） 打算采用使用念茜的脚本，简便些： https://blog.csdn.net/yiyaaixuexi/article/details/29201699

3） 因此目前只要解决一个问题：遍历获取方法名 于是找到了https://github.com/kaich/codeobscure  ---- 自动查找自定义的方法名来进行替换，而无需手动配置func.list 文件

```


- [ShannonChenCHN/iOSLevelingUp](https://github.com/ShannonChenCHN/iOSLevelingUp/issues/28)

```


<!-- 评论存储区域 -->

<!-- class dump -->

获取头文件信息：先获取 ipa 包中的可执行文件，然后执行终端命令 class-dump -H 要破解的可执行文件路径 -o 破解后的头文件存放路径

<!-- 代码混淆 -->

1） 基本原理：通过宏定义将函数名、方法名、类名、属性名等代码内容替换成无法被识别的加密字符串

2）要点：

如何配置宏定义，一个一个手写吗？ -----就是念茜的方式：  https://blog.csdn.net/yiyaaixuexi/article/details/29201699
 - 把要替换的内容写到一个文件里面，然后通过脚本读取该文件内容，再生成相应的宏定义文件，并在 pch 文件中导入该文件 --confuse.sh
 - 什么时候替换？怎么替换？ --build phrase


3）注意点：
    - 不要把系统的方法名和类名给替换掉了，最好给自定义的方法名加前缀  -- 很重要，养成习惯，便于以后的代码混淆
    - XIB中拖线的控件名


4) zhangkn/confuse.sh : https://gist.github.com/zhangkn/dc2f5e5a883cc506d20fc6285775c313


```


- [iOS App 的逆向工程: Hacking on Lyft](https://academy.realm.io/cn/posts/conrad-kramer-reverse-engineering-ios-apps-lyft/)

- [iOS学习之解压资源](https://github.com/steventroughtonsmith/cartool)

```


Export images from OS X / iOS .car CoreUI archives

<!-- $ cd <cartool文件所在目录> -->
$ ./cartool  /xxx/Assets.car /xxx/outputDirectory


```









