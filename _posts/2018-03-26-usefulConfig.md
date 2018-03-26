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