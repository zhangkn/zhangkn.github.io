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

### Q&A

>* -sh: ps: command not found

iphone  安装pstree 即可

>*  Could not get lock /var/lib/dpkg/lock - open (35: Resource temporarily unavailable)
```
A01-27:~ root# reboot
```

### 参考资源



