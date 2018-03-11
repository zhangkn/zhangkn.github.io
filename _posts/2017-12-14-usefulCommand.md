---
layout: post
title: usefulCommand
date: 2017-12-14
tag: iOSre
---
### 前言

常用的命令和工具


### mkdir

>* -p
```
# mkdir -p或--parents 若所要建立目录的上层目录目前尚未建立，则会一并建立上层目录；
# devzkndeMacBook-Pro:Layout devzkn$ mkdir -p private/var/mobile/Media/TouchSprite/lua/
```

### python

>* [SimpleHTTPServer 可以使用在部署机器工作中，比如在cydia，中创建一个自己的私有源](https://github.com/zhangkn/KNBin/blob/master/kncydia)


>* [SimpleHTTPServer 开启一个本地服务器，访问方式：http://127.0.0.1:8088/IOS%EF%BC%8DKevin/](https://github.com/zhangkn/KNBin/blob/master/kncydiaServer)
```
devzkndeMBP:~ devzkn$  python -m SimpleHTTPServer 8088
Serving HTTP on 0.0.0.0 port 8088 ...
127.0.0.1 - - [01/Feb/2018 13:00:16] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [01/Feb/2018 13:00:16] code 404, message File not found
127.0.0.1 - - [01/Feb/2018 13:00:16] "GET /favicon.ico HTTP/1.1" 404 -
127.0.0.1 - - [01/Feb/2018 13:00:29] "GET /.bash_history HTTP/1.1" 200 -
127.0.0.1 - - [01/Feb/2018 13:00:33] "GET /IOS%EF%BC%8DKevin/ HTTP/1.1" 200 -
^CTraceback (most recent call last):
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/runpy.py", line 162, in _run_module_as_main
    "__main__", fname, loader, pkg_name)
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/runpy.py", line 72, in _run_code
    exec code in run_globals
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/SimpleHTTPServer.py", line 235, in <module>
    test()
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/SimpleHTTPServer.py", line 231, in test
    BaseHTTPServer.test(HandlerClass, ServerClass)
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/BaseHTTPServer.py", line 599, in test
    httpd.serve_forever()
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/SocketServer.py", line 236, in serve_forever
    poll_interval)
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/SocketServer.py", line 155, in _eintr_retry
    return func(*args)
KeyboardInterrupt
```

### ssh 远程控制

>* scp -r 同步目录
```
scp -r ~/cydia cydia://home/
```

>* 1、ssh-copy-id：千万不要把私钥泄漏！只是把公钥（*.pub 文件）复制给远程服务器，公钥就是要对外公开的。
```
# 1、IP+默认端口 这里拷贝的是公钥，，如果是指定私钥ssh-copy-id 会自己寻找公钥。
devzkndeMacBook-Pro:.ssh devzkn$ ssh-copy-id -i ~/.ssh/id_rsa_Theos125 root@192.168.2.144
# 本质上是将id_rsa.pub拷贝一份保存为authorized_keys iPhone:~/.ssh root# cat /var/root/.ssh/authorized_keys
# 2、指定端口
devzkndeMacBook-Pro:SQTaoke devzkn$ ssh-copy-id -i -p 2222  ~/.ssh/id_rsa_Theos125 root@localhost:2222
# 3、使用别名
devzkndeMacBook-Pro:bin devzkn$ ssh-copy-id -i ~/.ssh/id_rsa_Theos125 iphone150
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/devzkn/.ssh/id_rsa_Theos125.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@192.168.2.163's password: 
Number of key(s) added:        1
Now try logging into the machine, with:   "ssh 'iphone150'"
and check to make sure that only the key(s) you wanted were added.
#4、使用负责脚本
#!/bin/sh
# devzkndeMacBook-Pro:mplink devzkn$ sship 192.168.2.183
postName="root@"$1
ssh-copy-id -i ~/.ssh/id_rsa_Theos125 $postName
exit 0
```

>* 2、scp 到Mac

```
iPhone:/Applications/iNalyzer.app root# scp /var/root/Documents/iNalyzer/2B559443-6CEE-4731-AA3B-7E587BE67219/static_2017_12_09-14_24_57.zip devzkn@192.168.2.186://Users/devzkn/decrypted/knMokssnossV5.4.0
#sftp（ /usr/libexec/sftp-server  ）、scp （ /usr/bin/scp）
```
ps :iPhone SSH Mac 的前提是mac setremotelogin on

```
devzkndeMacBook-Pro:.ssh devzkn$ sudo systemsetup -setremotelogin off

devzkndeMacBook-Pro:.ssh devzkn$ sudo systemsetup -setremotelogin on
devzkndeMacBook-Pro:.ssh devzkn$ sudo systemsetup -getremotelogin
Remote Login: On
```

### 利用 ssh 的用户配置文件 config 管理 ssh 会话（How do I connect to ssh with a different public key）

```
# Private usb2222
Host usb2222
Port 2222 
HostName  localhost
User root
IdentityFile ~/.ssh/id_rsa_Theos125



# Private 192.168.2
Host iphone150
HostName  192.168.2.144
User root 
IdentityFile ~/.ssh/id_rsa_Theos125
```
>* scp 
```
scp com.my.al.deb usb2222:/var/root
```

### Xcode 编译相关的 run script

>* 将与tweak工程同名的plist文件和dylib文件放入/Library/MobileSubstrate/DynamicLibraries目录下
```
#/Library/MobileSubstrate/DynamicLibraries -> /var/stash/_.UmC91h/DynamicLibraries/
/opt/iOSOpenDev/bin/iosod --xcbp
echo "$TARGET_BUILD_DIR/$EXECUTABLE_NAME"
ldid -S "$TARGET_BUILD_DIR/$EXECUTABLE_NAME"
cp "$PROJECT_DIR/$TARGET_NAME/Package/Library/MobileSubstrate/DynamicLibraries/$TARGET_NAME".plist "$TARGET_BUILD_DIR"
scp -r "$TARGET_BUILD_DIR/"* ip:/var/stash/_.UmC91h/DynamicLibraries/
```
>*  [一次性编译多个dylib、framework、.a、tweakTool 的方法](https://github.com/zhangkn/KNCocoaTouchStaticLibrary)
```
# 1、 建立一个iOSapp 工程之后，将对应的dylib、framework、.a、tweakTool 工程目录导入到app工程
# 2、将需要一起编译的dylib、framework、.a、tweakTool 加入iOSapp的TargetDependencies列表
```

### nm
>* dump symbol table
```
devzkndeMacBook-Pro:MookkknV5.4.0 devzkn$ nm -gUj Molon.decrypted |head -n 20
nm -gUj MoKNon.decrypted |grep Observer
```

### hopper 的快捷键

```
1、按x查找引用关系
2、hopper按g跳转到指定地址:
```

### ln

>* 创建文件的软连接
```
devzkndeMacBook-Pro:bin devzkn$ sudo  ln -s /opt/iOSOpenDev/bin/ldid /opt/theos/bin/ldid
Password:
devzkndeMacBook-Pro:bin devzkn$ ls -l  /opt/theos/bin/ldid
lrwxr-xr-x  1 root  wheel  24 Jan 16 11:09 /opt/theos/bin/ldid -> /opt/iOSOpenDev/bin/ldid
```

### killall
```
killall -9 SpringBoard
```

### socat 的常用命令

>* UNIX-CONNECT
```
socat - UNIX-CONNECT:/var/run/lockdown/syslog.sock
```

### echo 
>* 清空文件内容
```
echo "https://zhangkn.github.io/archive/" > /var/log/syslog
```
>* 清理垃圾命令
```
apt-get autoclean 清理旧版本的软件缓存
apt-get clean 清理所有软件缓存
apt-get autoremove 删除系统不再使用的孤立软件
nautilus /boot 删除除了最新内核以外的其它文件
gedit /boot/grub/menu.lst 删除除最新内核以外的其它启动项（
```


### 排查网络问题的常用命令

>* 修改 手机hosts-- 错误的用法
```
#echo -e "换\n行"
	install.exec "echo '127.0.0.1 localhost 
	 192.168.2.254 wssesschknknknknkdsat.fkdjdasssllllqusssasssnwwwwgwwwe.cn' > /etc/hosts"
```
>* 修改 手机hosts 正确的用法
```
#echo -e "换\n行"
install.exec "echo -e '127.0.0.1 localhost \n 192.168.2.107 kn.kn.cn' > /etc/hosts"
```
>* ping 网络
```
iPhone:~ root# ping wecKNhat.fsshhhhasqKNuanKNgllkjdde.cn
```

>* curl 请求接口
```
iPhone:~ root# curl "http://wKNeKNcKNhKNat.faKNqKNuaKNnKNge.cn/inKNdeKNx.php?m=TKKNL&c=trKNKNansKNKNKNKNNfoKNKKNrm"
```

### apt-get

>* update
```
apt-get update
```
>* install
```
apt-get install socat
```


### git

在遇到git 冲突的时候，常常可以利用git status 来查看解决方案

>* 1、 git log 
```
devzkndeMacBook-Pro:SQTaoke devzkn$ git log --pretty=oneline
461b940921343dd4163ab8abf41efca474db4fd9 (HEAD -> master, origin/master) first commit
```
```
devzkndeMacBook-Pro:SQTaoke devzkn$ git log -p -2
```
>* 2、 git reset
```
devzkndeMacBook-Pro:SQTaoke devzkn$ git reset --hard  32f87f5e93501b24d42c7aba49106dd9bed0b943
HEAD is now at 32f87f5 处理非200
```


>* 3、git remote remove origin
```
git remote remove origin
```
取消本地目录下关联的远程库;常常用于copyxx项目的基础上，创建新项目的场景

>* git 常用合并命令
```
#切换回master分支
git checkout master
# merge  --no-ff参数，表示禁用Fast forward;可以保存你之前的分支历史。能够更好的查看merge历史，以及branch 状态.
#保证版本提交、分支结构清晰
git merge --no-ff  develop
#push
git push
```
![](/images/posts/{{page.title}}/knco.png)

>* [kngit](https://github.com/zhangkn/KNBin/blob/master/kngit) ：提交代码到本地仓库和远程仓库
```
devzkndeMacBook-Pro:bin devzkn$ cat kngit
#!/bin/sh
# dirname $0，取得当前执行的脚本文件的父目录
# cd `dirname $0`，进入这个目录(切换当前工作目录)
# cd `dirname $0` 
#alias gitadd='cd `dirname $0` && git add . && git commit -m /!* && git push'
git add .
git commit -m $1
git push
```

>* 查看最后一次提交
```
git log -p -2|head -n 6
```

[更多辅助脚本参考这里](https://github.com/zhangkn/KNBin)

>* [创建分支develop ，并提交到远程仓库;](https://github.com/zhangkn/KNBin/blob/master/knco)
```
git checkout -b develop
#  提交本地develop分支作为远程的develop分支
git push origin develop:develop
# 本地分支和远程分支建立联系(使用git branch -vv 可以查看本地分支和远程分支的关联关系)
#git branch --set-upstream-to=origin/远程分支的名字 本地分支的名字
git branch --set-upstream-to=origin/develop develop
```

>* git branch -vv

```
devzkndeMacBook-Pro:zhangkn.github.io devzkn$ git branch -vv
  master 7fcaf91 [origin/master] 创建分支develop
* test   7fcaf91 [origin/test] 创建分支develop
```

>* 查看最近一次更改。
```
devzkndeMacBook-Pro:.git devzkn$ git show HEAD
commit 7b5fa90112e9f49
```


### Remote Virtual Interface Tool

```
devzkndeMacBook-Pro:zhangkn.github.io devzkn$ rvictl -s udid1
```
>* wireshark
```
http && ip.src_host== 192.168.2.174
```


###  利用 PrivateFrameworks 打开app
Image Source: /System/Library/PrivateFrameworks/AppLaunchStats.framework/AppLaunchStats

```
//Operating System: Version 8.4 (Build 12H143)

%new
//打开
- (void)openApplicationWithBundleID
{
    Class CLSApplicationWorkspace = objc_getClass("LSApplicationWorkspace");
    NSObject * workspace = [CLSApplicationWorkspace performSelector:@selector(defaultWorkspace)];
    [workspace performSelector:@selector(openApplicationWithBundleID:) withObject:@"com.knalima.mn"];
}
```


### ios 中利用NSTask 执行shell脚本
```
//执行shell脚本
NSString *doShellCmd(NSString *cmd)
{
    NSTask *task;
	task = [[NSTask alloc ]init];//通过NSTask，您的程序可以分出 一个子进程来执行其它工作或进行进度监控。
	[task setLaunchPath:@"/bin/bash"];
	NSArray *arguments = [NSArray arrayWithObjects:@"-c",cmd, nil];
	[task setArguments:arguments];

	NSPipe *pipe = [NSPipe pipe];//NSPipe代表一个BSD管道，即一种进程间的单向通讯通道。 线程和子任务
	[task setStandardOutput:pipe];

	NSFileHandle *file = [pipe fileHandleForReading];

	[task launch];

	NSData *data = [file readDataToEndOfFile];

	NSString *string = [[NSString alloc]initWithData:data encoding:NSUTF8StringEncoding];
    return string;
}
```


###  deploy tweak shell


>* 比如 make package 过程，可以通过 make package message=yes 输出详细错误信息
```
#!/bin/sh
cd `dirname $0` 
make clean
make package install debug=0
rm ./packages/*.deb
exit 0
```

### lldb远程调试命令

>* 查看可用的平台
```
(lldb) platform list
Available platforms:
host: Local Mac OS X user platform plug-in.
remote-freebsd: Remote FreeBSD user platform plug-in.
remote-linux: Remote Linux user platform plug-in.
remote-netbsd: Remote NetBSD user platform plug-in.
remote-windows: Remote Windows user platform plug-in.
remote-ios: Remote iOS platform plug-in.
```
>* 选择远程ios为 当前平台
```
(lldb) platform select remote-ios
  Platform: remote-ios
 Connected: no
  SDK Path: "/Users/devzkn/Library/Developer/Xcode/iOS DeviceSupport/11.1.2 (15B202)"
 SDK Roots: [ 0] "/Users/devzkn/Library/Developer/Xcode/iOS DeviceSupport/10.0.2 (14A456)"
 SDK Roots: [ 1] "/Users/devzkn/Library/Developer/Xcode/iOS DeviceSupport/9.0 (13A344)"
```
选项解释
```
--sysroot  指定SDK根目录(含有所有的远程系统文件)
--version
--build
```


```
(lldb) platform select remote-ios --sysroot  /Volumes/Xcode/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS5.1.sdk
```

>* 连接到远程debugserver (Connect a platform by name to be the currently selected platform.

```
(lldb) platform connect connect://127.0.0.1:12345
```

>*  Mac端LLDB的接入 即LLDB连接 iPhone debugserver :连接到调试服务(Connect to a remote debug service)
```
process connect connect://127.0.0.1:12345
```
前提是debugserver 已经在监听对应的端口
```
iPhone:/usr/bin root# debugserver *:12345 -a "KNWeKNChat"
```
>* breakpoint set
```
断在ObjC的消息selector
-S <selector>
断在指定的内存地址
-a <address> 
```

>*  memory read   开始地址  结束地址
```
(lldb) memory read 0x0000000107905c44
0x107905c44: 4d 01 26 89 d8 ff c3 4d 01 fe 4c 39 e8 72 ad 48  M.&....M..L9.r.H
0x107905c54: 8d 05 2e 1c 02 00 44 01 28 8b 45 c4 4c 8b 6d b0  ......D.(.E.L.m.
```


>* register read
```
(lldb) register read
General Purpose Registers:
       rax = 0x00000001079276d8  dyld::gLinkContext
```
![](/images/posts/{{page.title}}/cpu.png)
>* po/x 
```
(lldb) po/x $rip
0x0000000107905c44
```
![](/images/posts/{{page.title}}/register.png)

>* disassemble
```
(lldb) help memory
(lldb) help disassemble
(lldb) help memory write
```

### netstat

 显示网络连接、路由表、接口状态

 ```
 devzkndeMacBook-Pro:zhangkn.github.io devzkn$ netstat
Active Internet connections
Proto Recv-Q Send-Q  Local Address          Foreign Address        (state)    
tcp4       0      0  192.168.2.143.49417    47.88.153.28.19527     ESTABLISHED
 ```

### lsof 
>* 列出当前系统打开的文件（网络连接、硬件）
```
devzkndeMacBook-Pro:zhangkn.github.io devzkn$ lsof
COMMAND     PID   USER   FD      TYPE             DEVICE   SIZE/OFF       NODE NAME
loginwind   113 devzkn  cwd       DIR                1,4        992          2 /
loginwind   113 devzkn  txt       REG                1,4    1241072 8590278373 /System/Library/CoreServices/loginwindow.app/Contents/MacOS/loginwindow
loginwind   113 devzkn  txt       REG                1,4   26752912 8590676059 /usr/share/icu/icudt59l.dat
```

### file 

“file能够识别大量的文件格式，包括数种ASCII 文本文件、各种可执行文件和数据文件。file执行的幻数检查由幻数文件（magic file ）所包含的规则控制。”

>* /usr/share/file/magic
```
devzkndeMacBook-Pro:magic devzkn$ ls -lrt /usr/share/file/magic
total 456
-rw-r--r--  1 root  wheel    613 Jul 16 05:03 zyxel
-rw-r--r--  1 root  wheel    451 Jul 16 05:03 zilog
```

>* “file通过检查文件中的某些特定字段来确认文件的类型”
```
devzkndeMacBook-Pro:MoonV5.4.0 devzkn$ file knMoknon.decrypted
knMoknon.decrypted: Mach-O universal binary with 2 architectures: [arm_v7:Mach-O executable arm_v7] [arm64]
knMoknon.decrypted (for architecture armv7):	Mach-O executable arm_v7
knMoknon.decrypted (for architecture arm64):	Mach-O 64-bit executable arm64
```


### ps 
>* 显示现行终端机下的所有程序，包括其他用户的程序
```
devzkndeMacBook-Pro:Payload devzkn$ ps a
  PID   TT  STAT      TIME COMMAND
  424 s000  Ss     0:00.05 login -pf devzkn
  428 s000  S      0:00.03 -bash
51659 s000  S+     0:00.67 python /Users/devzkn/Downloads/kevinsoftware/ios-Reverse_Engineering/usbmuxd-1.0.8 2/python-client/tcprelay.py -t 22:2222
```
>* Display information about other users' processes, including those without controlling terminals.
```
devzkndeMacBook-Pro:Payload devzkn$ ps -A
  PID TTY           TIME CMD
    1 ??        19:50.79 /sbin/launchd
   61 ??         0:36.83 /usr/sbin/syslogd
```
>* 列出程序时，显示每个程序真正的指令名称，而不包含路径，参数或常驻服务的标示
```
devzkndeMacBook-Pro:zhangkn.github.io devzkn$ ps c
  PID   TT  STAT      TIME COMMAND
  428 s000  S      0:00.03 -bash
51659 s000  S+     0:00.67 python
```
>* ps e   列出程序时，显示每个程序所使用的环境变量。

>* ps u 　 以用户为主的格式来显示程序状况

>* 结合grep 使用，来查找对应的进程信息
```
devzkndeMacBook-Pro:passionfruit devzkn$ ps  aux |grep zhangkn.github.io
devzkn           65404   0.0  0.0  4305256   1808 s004  S+   Sat04PM   0:02.79 /Library/Ruby/Gems/2.3.0/gems/rb-fsevent-0.10.2/bin/fsevent_watch --format=otnetstring --latency 0.1 /Users/devzkn/githubPages/zhangkn.github.io
```

### Q&A

>* -sh: ps: command not found

iphone  安装pstree 即可

>*  Could not get lock /var/lib/dpkg/lock - open (35: Resource temporarily unavailable)
```
A01-27:~ root# reboot
```

>*  Could not get lock /var/lib/dpkg/lock - open (35: Resource temporarily unavailable)
```
rm -rf /var/lib/dpkg/lock 
#即可
```

>*  dpkg: status database area is locked by another process
```
 sshusb rm -rf  /var/lib/dpkg/lock
```
### grep命令

```
格式 grep [options]
主要参数
[options]主要参数：
－c：只输出匹配行的计数。
－I：不区分大 小写(只适用于单字符)。
－h：查询多文件时不显示文件名。
－l：查询多文件时只输出包含匹配字符的文件名。
－n：显示匹配行及 行号。
－s：不显示不存在或无匹配文本的错误信息。
－v：显示不包含匹配文本的所有行。
pattern正则表达式主要参数：
\： 忽略正则表达式中特殊字符的原有含义。
^：匹配正则表达式的开始行。
$: 匹配正则表达式的结束行。
\<：从匹配正则表达 式的行开始。
\>：到匹配正则表达式的行结束。
[ ]：单个字符，如[A]即A符合要求 。
[ - ]：范围，如[A-Z]，即A、B、C一直到Z都符合要求 。
。：所有的单个字符。
* ：有字符，长度可以为0。
```

### chrome

```
# 展示所有的命令
chrome://chrome-urls/
```

### 参考
- [搭建一个提高开发效率的iOS静态库工程](http://blog.csdn.net/z929118967/article/details/73872024)
- [OS X 上的动态链接库劫持 (下)](https://82flex.com/2016/06/08/1461d81d0e5b014ab754aa68f6bbd86c92e42ec6/)
- [KNCocoaAsyncSocketDemo](https://github.com/zhangkn/KNCocoaAsyncSocketDemo)
- [MPProcessMessage](https://github.com/zhangkn/MPProcessMessage)
- [unix shell scripts](https://github.com/samsonjs/bin)
- [lldb远程调试命令](https://zhiwei.li/text/category/reverse_engineering/)
- [Objetive-C内存布局](https://zhiwei.li/text/2012/03/10/objetive-c%E5%86%85%E5%AD%98%E5%B8%83%E5%B1%80/)
- [十年•杭研技术秀iOS App的加固保护原理](http://blog.163yun.com/archives/1065)
- [常用工具总结](https://tomatobin.gitbooks.io/iosreproject/content/chang_yong_gong_ju_zong_jie.html)
- [总结linux清理垃圾命令](http://blog.csdn.net/mndlyt/article/details/17029105)


