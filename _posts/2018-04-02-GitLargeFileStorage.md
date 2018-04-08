---
layout: post
title: GitLargeFileStorage
date: 2018-04-02
tag: iOSre
site: https://zhangkn.github.io
---


### 前言



https://git-lfs.github.com


### 正文

![](/images/posts/{{page.title}}/graphic.gif)

>* remote: error: File FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector is 106.49 MB; this exceeds GitHub's file size limit of 100.00 MB

```


<!-- Homebrew: brew install git-lfs -->


remote: Resolving deltas: 100% (47/47), done.
remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com
remote: error: Trace: 8d1daee3e9422245a2a0fa9935b3bb8b
remote: error: See http://git.io/iEPt8g for more information.
remote: error: File FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector is 106.49 MB; this exceeds GitHub's file size limit of 100.00 MB


<!-- https://github.com/git-lfs/git-lfs/releases/download/v2.4.0/git-lfs-darwin-amd64-2.4.0.tar.gz -->
MacPorts: port install git-lfs


devzkndeMacBook-Pro:GoogleMobileVision-1.1.0 devzkn$ /Users/devzkn/Downloads/git-lfs-2.4.0/install.sh 



devzkndeMacBook-Pro:GoogleMobileVision-1.1.0 devzkn$ git lfs track FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector
Tracking "FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector"
devzkndeMacBook-Pro:GoogleMobileVision-1.1.0 devzkn$ git lfs status
On branch master

Git LFS objects to be committed:


Git LFS objects not staged for commit:

	FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector (Git: e4cea14 -> File: e4cea14)

devzkndeMacBook-Pro:GoogleMobileVision-1.1.0 devzkn$ git lfs ls-files
<!-- devzkndeMacBook-Pro:GoogleMobileVision-1.1.0 devzkn$ git lfs version -->
git-lfs/2.4.0 (GitHub; darwin amd64; go 1.8.3; git 1a98fa5c)

<!-- devzkndeMacBook-Pro:GoogleMobileVision-1.1.0 devzkn$ git lfs track -->
Listing tracked patterns
    FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector (.gitattributes)


<!-- devzkndeMacBook-Pro:GoogleMobileVision-1.1.0 devzkn$ kngitinit git@github.com:GPUImage/GoogleMobileVision-1.1.0.git -->
Reinitialized existing Git repository in /Users/devzkn/code/GPU/GoogleMobileVision-1.1.0/.git/
[master 35680f5] first commit
 3 files changed, 2 insertions(+)
 create mode 100644 .gitattributes
 rewrite FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector (99%)
Counting objects: 161, done.(1/1), 179 MB | 477 KB/s                                                                                                                                                                                                        
Delta compression using up to 4 threads.
Compressing objects: 100% (141/141), done.
packet_write_wait: Connection to 52.74.223.119 port 22: Broken pipe
fatal: The remote end hung up unexpectedly
fatal: The remote end hung up unexpectedly
devzkndeMacBook-Pro:GoogleMobileVision-1.1.0 devzkn$ 




```


### see also

- [request-failed-on-channel-0](https://stackoverflow.com/questions/27021641/how-to-fix-request-failed-on-channel-0)

```


<!-- 获取尝试换下端口 -->


<!-- 以下这个方法行不通，危险-->
Taokeceshiji1:~ root# cat /etc/fstab

Taokeceshiji1:~ root# cat /etc/fstab
/dev/disk0s1s1 / hfs ro 0 1
/dev/disk0s1s2 /private/var hfs rw 0 2



Taokeceshiji1:~ root# echo "none    /dev/pts    devpts    defaults    0    0
> " >> /etc/fstab
Taokeceshiji1:~ root# cat /etc/fstab
/dev/disk0s1s1 / hfs ro 0 1
/dev/disk0s1s2 /private/var hfs rw 0 2
none    /dev/pts    devpts    defaults    0    0

Just add these lines to your /etc/mtab and /etc/fstab, and reboot the system.


<!-- -sh: fork: retry: Resource temporarily unavailable -->

文件/etc/fstab包含了静态文件系统信息，定义了存储设备和分区整合到整个系统的方式。mount 命令会读取这个文件，确定设备和分区的挂载选项。


```

- [Git LFS 操作指南](https://zzz.buzz/zh/2016/04/19/the-guide-to-git-lfs/)

```


<!-- 配置 -->

使用 git lfs track 追踪需要使用 Git LFS 管理的文件。如：

git lfs track "*.psd"


<!-- 也可以手动编辑 Git 仓库根目录下的 .gitattributes 文件，如： -->

*.psd filter=lfs diff=lfs merge=lfs -text

<!-- 常用 Git LFS 命令 -->
git lfs track

devzkndeMacBook-Pro:GoogleMobileVision-1.1.0 devzkn$ git lfs track
Listing tracked patterns
    FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector (.gitattributes)



# 使用 Git LFS 管理指定的文件

devzkndeMacBook-Pro:GoogleMobileVision-1.1.0 devzkn$ git lfs track FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector
Tracking "FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector"

# 不再使用 Git LFS 管理指定的文件
git lfs untrack "*.psd"


# 类似 `git status`，查看当前 Git LFS 对象的状态

devzkndeMacBook-Pro:GoogleMobileVision-1.1.0 devzkn$ git lfs status
On branch master

Git LFS objects to be committed:


Git LFS objects not staged for commit:

	FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector (Git: e4cea14 -> File: e4cea14)


# 枚举目前所有被 Git LFS 管理的具体文件
git lfs ls-files


# 检查当前所用 Git LFS 的版本
git lfs version

# 针对使用了 LFS 的仓库进行了特别优化的 clone 命令，显著提升获取
# LFS 对象的速度，接受和 `git clone` 一样的参数。 [1] [2]
git lfs clone https://github.com/user/repo.git

<!-- 目前最新版本的 git clone 已经能够提供与 git lfs clone 一致的性能 -->

```

- [ 'openssl/aes.h' file not found](https://www.anintegratedworld.com/mac-osx-fatal-error-opensslsha-h-file-not-found/)

```
-rw-r--r--  1 devzkn  admin  6146 Mar 27 21:55 /usr/local/opt/openssl/include/openssl/aes.h


<!-- devzkndeMacBook-Pro:include devzkn$ ln -s  -->
devzkndeMacBook-Pro:include devzkn$ ln -s /usr/local/opt/openssl/include/openssl /usr/local/include/


<!-- 将/usr/local/include 添加到对应过程的header search paths -->



```