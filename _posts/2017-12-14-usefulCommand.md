---
layout: post
title: usefulCommand
date: 2017-12-14
tag: iOSre
---

### ssh

>* 1、ssh-copy-id
```
devzkndeMacBook-Pro:.ssh devzkn$ ssh-copy-id -i ~/.ssh/id_rsa_Theos125 root@192.168.2.144
```
```
# 指定端口
devzkndeMacBook-Pro:SQTaoke devzkn$ ssh-copy-id -i -p 2222  ~/.ssh/id_rsa_Theos125 root@localhost:2222
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

### killall
```
killall -9 SpringBoard
```

### socat 的常用命令

>* UNIX-CONNECT
```
socat - UNIX-CONNECT:/var/run/lockdown/syslog.sock
```

### 排查网络问题的常用命令

>* 修改 手机hosts
```
	install.exec "echo '127.0.0.1 localhost 
	 192.168.2.254 wssesschknknknknkdsat.fkdjdasssllllqusssasssnwwwwgwwwe.cn' > /etc/hosts"
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


### Q&A

>* -sh: ps: command not found

iphone  安装pstree 即可

>*  Could not get lock /var/lib/dpkg/lock - open (35: Resource temporarily unavailable)
```
A01-27:~ root# reboot
```

### 参考资源

- [unix shell scripts](https://github.com/samsonjs/bin)
- [lldb远程调试命令](https://zhiwei.li/text/category/reverse_engineering/)
- [Objetive-C内存布局](https://zhiwei.li/text/2012/03/10/objetive-c%E5%86%85%E5%AD%98%E5%B8%83%E5%B1%80/)


