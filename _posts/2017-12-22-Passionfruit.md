---
layout: post
title: Passionfruit
date: 2017-12-22
tag: iOSre
site: https://zhangkn.github.io
---

### 前言

- 我在使用Passionfruit 的时候，安装步骤碰到的问题是[fatal error: 'frida-core.h' file not found](https://github.com/frida/frida-node/issues/17),具体的请看Q&A。
- 安全审计的工具 我觉得iNalyzer 已经过时了，推荐这款[Passionfruit](https://github.com/chaitin/passionfruit);
- Passionfruit 通过frida注入代码到目标应用实现了个“动态分析iOS应用”的图形界面。



### Passionfruit 的实现原理


![](/images/posts/{{page.title}}/{{page.title}}open.png)

Passionfruit 通过 frida 注入代码到目标应用实现功能，再通过 node.js 服务端消息代理与浏览器通信，用户通过访问网页即可对 App 实现常规的检测任务。



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

- Unable to launch iOS app: timeout

启动应用程序失败之后，装置就重启了。这个问题 有点类似[Failed to spawn: unable to launch iOS app: timeout](http://iosre.com/t/failed-to-spawn-unable-to-launch-ios-app-timeout/10422)

临时解决方式： 手动启动app ，还是可以正常分析的

### 参考资料

- [给 frida 做了个图形界面，动态分析 iOS 应用](http://iosre.com/t/frida-ios/9815)


