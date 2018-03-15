---
layout: post
title: document-ch-controlfields
date: 2018-03-13
tag: iOSre
site: https://zhangkn.github.io
---

### 前言

>* 本文重点说明Syntax of control files的three types of fields:simple、folded、multiline；

```
---因为一不小心就容易出错：missing final newline、Status: install ok half-configured、 EOF before value of field `Icon' (missing final newline)
 end of file during value of field 'Depiction' (missing final newline)

 <!-- 这些错误，临时的解决方案是 -->
 iPhone:/var/lib/dpkg root# rm -rf updates/*
 <!-- 要本质上解决错误，请看下文 -->
```


### 正文


>* simple
```
The field, including its value, must be a single line. 
```
>* folded
```
The value of a folded field is a logical line that may span several lines. The lines after the first are called continuation lines and must start with a space or a tab.
Whitespace, including any newlines, is not significant in the field values of folded fields.
```

>* multiline
```
The value of a multiline field may comprise multiple continuation lines. The first line of the value, the part on the same line as the field name, often has special significance or may have to be empty. Other lines are added following the same syntax as the continuation lines of the folded fields. Whitespace, including newlines, is significant in the values of multiline fields.
```


>* The fields in the general paragraph (the first one, for the source package) are:
```
Source (mandatory)
Maintainer (mandatory)
Uploaders
Section (recommended)
Priority (recommended)
Build-Depends et al
Standards-Version (recommended)
Homepage
Version Control System (VCS) fields
Testsuite
```
>* The fields in the binary package paragraphs are:
```
Package (mandatory)
Architecture (mandatory)
Section (recommended)
Priority (recommended)
Essential
Depends et al
Description (mandatory)
Homepage
Built-Using
Package-Type
```

### Sections

>* value
```
Section: System
Section: Tweaks
```


### dpkg

>*  查看其他deb 包的preinst、Postinst 脚本

```
iPhone:/var/lib/dpkg/info root# ls -lrt
total 1688
-rwxr-xr-x 1 root   wheel    114 Apr  8  2009 pam.preinst
-rwxr-xr-x 1 root   wheel    114 Apr  8  2009 pam-modules.preinst
-rwxr-xr-x 1 root   wheel    375 Mar 27  2010 firmware-sbin.postrm
```

>* 查看其他deb源对应的control文件信息

```
iPhone:/var/lib/dpkg root# cat available
iPhone:/var/lib/dpkg root# cat available
Package: debianutils
Essential: yes
Priority: required
Section: Utilities
Installed-Size: 40
Maintainer: Jay Freeman (saurik) <saurik@saurik.com>
Architecture: iphoneos-arm
Version: 3.3.3ubuntu1-1p
Pre-Depends: dpkg (>= 1.14.25-8)
Size: 6164
Description: pretty much just run-parts. yep? run-parts
Name: Debian Utilities

Package: com.saurik.patcyh
Priority: optional
Section: System
Installed-Size: 224
Maintainer: Jay Freeman (saurik) <saurik@saurik.com>
Architecture: iphoneos-arm
Version: 1.2.0
Depends: firmware (>= 8.3), coreutils-bin, ldid
Size: 27528
Description: patch for installd to support uikittools
Name: Patcyh
Author: Jay Freeman (saurik) <saurik@saurik.com>
Depiction: http://cydia.saurik.com/info/com.saurik.patcyh/
Tag: cydia::obsolete
```

### /var/lib/dpkg/available 

>* 通过/var/lib/dpkg/available 文件，查看Section的种类

```
iPhone:/var/lib/dpkg root# cat available| grep Section | sort | uniq -d | less

<!-- 或者使用sublime 进行排序和去重 -->
Section: Administration
Section: Archiving
Section: Data_Storage
Section: Development
Section: Networking
Section: Packaging
Section: Repositories
Section: Security
Section: System
Section: Terminal_Support
Section: Tweaks
Section: Utilities
```

>* 查看当前安装的deb包名
```
iPhone:/var/lib/dpkg root# cat available| grep Package 
Package: debianutils
Package: libxml2
Package: apt7-lib
Package: lzma
Package: mobilesubstrate
Package: sqlite3
Package: com.saurik.substrate.safemode
Package: bzip2
Package: openssh
Package: openssl
Package: sqlite3-lib
Package: diffutils
Package: apt7-key
Package: tar
Package: shell-cmds
Package: uikittools
Package: readline
Package: darwintools
Package: lighttpd
Package: sed
Package: pcre
Package: com.saurik.afc2d
Package: gnupg
Package: grep
Package: pam-modules
Package: cydia
Package: com.rpetrich.rocketbootstrap
Package: findutils
Package: coreutils-bin
Package: gzip
Package: dpkg
Name: Debian Packager
Package: ldid
Package: libxml2-lib
Package: profile.d
Package: basic-cmds
Package: cydia-lproj
Package: org.thebigboss.repo.icons
Package: pam
Package: firmware-sbin
Package: base
Package: bash
Package: system-cmds
Package: com.saurik.patcyh
Package: ncurses
```


### iPhone:/var/lib/dpkg root# cat status

status 的内容比available 更丰富

### see also

- [https://www.debian.org/doc/debian-policy/#document-ch-controlfields](https://www.debian.org/doc/debian-policy/#document-ch-controlfields)

```

<!-- 5.2. Source package control files – debian/control¶ -->
<!-- 7.6.2. Replacing whole packages, forcing their removal¶ -->
Second, Replaces allows the packaging system to resolve which package should be removed when there is a conflict (see Conflicting binary packages - Conflicts). This usage only takes effect when the two packages do conflict, so that the two usages of this field do not interfere with each other.

Provides: mail-transport-agent
Conflicts: mail-transport-agent
Replaces: mail-transport-agent
```