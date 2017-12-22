---
layout: post
title: Passionfruit
date: 2017-12-22
tag: iOSre
site: https://zhangkn.github.io
---

### 前言

我在使用Passionfruit 的时候，安装步骤碰到的问题是[fatal error: 'frida-core.h' file not found](https://github.com/frida/frida-node/issues/17),具体的请看Q&A。

###  安装

- brew install libimobiledevice

```
devzkndeMacBook-Pro:passionfruit devzkn$ brew install libimobiledevice
```

-   brew install yarn

```
devzkndeMacBook-Pro:passionfruit devzkn$  brew install yarn
```
或者
```
brew install npm
```

- npm install --save frida@latest   then $ npm install
```
devzkndeMacBook-Pro:passionfruit devzkn$ npm install
> Passionfruit@0.0.3 postinstall /Users/devzkn/code/demo/passionfruit
> cd gui && (yarn || npm install)
yarn install v1.3.2
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
[4/4] 📃  Building fresh packages...
✨  Done in 231.08s.
up to date in 235.627s
```

- npm run build

```
devzkndeMacBook-Pro:passionfruit devzkn$ npm run build

> Passionfruit@0.0.3 build /Users/devzkn/code/demo/passionfruit
> frida-compile agent -o _agent.js && cd gui && (yarn run build || npm run build)

yarn run v1.3.2
```

- npm start

```
devzkndeMacBook-Pro:passionfruit devzkn$ npm start

> Passionfruit@0.0.3 start /Users/devzkn/code/demo/passionfruit
> cross-env NODE_ENV=production node .

listening on http://localhost:31337
```
### Q&A

- prebuild-install http 404 https://github.com/frida/frida/releases/download/10.6.13/frida-v10.6.13-node-v59-darwin-x64.tar.gz

```
devzkndeMacBook-Pro:passionfruit devzkn$ npm install --save frida@latest

> frida@10.6.28 install /Users/devzkn/code/demo/passionfruit/node_modules/frida
> prebuild-install || node-gyp rebuild

prebuild-install info begin Prebuild-install version 2.3.0
prebuild-install info looking for local prebuild @ prebuilds/frida-v10.6.28-node-v59-darwin-x64.tar.gz
prebuild-install info looking for cached prebuild @ /Users/devzkn/.npm/_prebuilds/https-github.com-frida-frida-releases-download-10.6.28-frida-v10.6.28-node-v59-darwin-x64.tar.gz
prebuild-install http request GET https://github.com/frida/frida/releases/download/10.6.28/frida-v10.6.28-node-v59-darwin-x64.tar.gz
prebuild-install http 200 https://github.com/frida/frida/releases/download/10.6.28/frida-v10.6.28-node-v59-darwin-x64.tar.gz
prebuild-install info downloading to @ /Users/devzkn/.npm/_prebuilds/https-github.com-frida-frida-releases-download-10.6.28-frida-v10.6.28-node-v59-darwin-x64.tar.gz.47369-6d6afc3ef5581.tmp
prebuild-install info renaming to @ /Users/devzkn/.npm/_prebuilds/https-github.com-frida-frida-releases-download-10.6.28-frida-v10.6.28-node-v59-darwin-x64.tar.gz
prebuild-install info unpacking @ /Users/devzkn/.npm/_prebuilds/https-github.com-frida-frida-releases-download-10.6.28-frida-v10.6.28-node-v59-darwin-x64.tar.gz
prebuild-install info unpack resolved to /Users/devzkn/code/demo/passionfruit/node_modules/frida/build/Release/frida_binding.node
prebuild-install info unpack required /Users/devzkn/code/demo/passionfruit/node_modules/frida/build/Release/frida_binding.node successfully
prebuild-install info install Successfully installed prebuilt binary!
+ frida@10.6.28
added 32 packages in 18.574s
```


### 参考资料

- [给 frida 做了个图形界面，动态分析 iOS 应用](http://iosre.com/t/frida-ios/9815)


