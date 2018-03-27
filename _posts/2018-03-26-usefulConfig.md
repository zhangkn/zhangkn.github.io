---
layout: post
title: usefulConfig
date: 2018-03-26
tag: tool
site: https://zhangkn.github.io
---

## 前言




## 正文


### tweak


>* /Package/Library/MobileSubstrate/DynamicLibraries/.plist

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Filter</key>
  <dict>
    <key>Executables</key>
    <array>
      
      <string>nesessionmanager</string>
            <string>racoon</string>
            <string>configd</string>
            <string>mDNSResponder</string>
            <string>networkd</string>
    </array>
        
        <key>Bundles</key>
        <array>
            <string>com.apple.nesessionmanager</string>
        </array>
  </dict>
</dict>
</plist>
```





### see also


- [ln](http://www.cnblogs.com/peida/archive/2012/12/11/2812294.html)

```



必要参数:

-b 删除，覆盖以前建立的链接

-d 允许超级用户制作目录的硬链接

-f 强制执行

-i 交互模式，文件存在则提示用户是否覆盖

-n 把符号链接视为一般目录

-s 软链接(符号链接)

-v 显示详细的处理过程


实例5：给目录创建软链接

# 先备份 cp -r /Library/MobileSubstrate/DynamicLibraries ~/
     # rm -rf /Library/MobileSubstrate/DynamicLibraries
    # aso11 代码： dylib   干脆给他建立个软连接算了        /bin/ln -s   /usr/lib/TweakInject /Library/MobileSubstrate/DynamicLibraries 
    # cp -r  ~/DynamicLibraries /Library/MobileSubstrate/DynamicLibraries


<!-- ln -sv /opt/soft/test/test3 /opt/soft/test/test5 -->

ln -sv   /usr/lib/TweakInject /Library/MobileSubstrate/DynamicLibraries      这个相当于创建同名的软连接


WL-47:~ root# ls -lrt /Library/MobileSubstrate/DynamicLibraries
lrwxr-xr-x 1 root wheel 25 Feb 13 12:48 /Library/MobileSubstrate/DynamicLibraries -> ../../usr/lib/TweakInject


```