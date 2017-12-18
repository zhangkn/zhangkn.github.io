---
layout: post
title: WebSocket
date: 2017-12-15
tag: iOSre
site: https://zhangkn.github.io
---

### 前言

[websocket](http://websocket.org/) 是一种网络通信协议,提供全双工通信信道,以基于事件的方式，赋予浏览器实时通信能力。

协议标识符是ws（如果加密，则为wss），服务器网址就是 URL;

![](/images/posts/{{page.title}}/ws.jpg)


### [Socket](https://en.wikipedia.org/wiki/Network_socket) 与 WebScoket

Socket 其实并不是一个协议。它工作在 OSI 模型会话层（第5层），是为了方便大家直接使用更底层协议（一般是 TCP 或 UDP ）而存在的一个抽象层。
![](/images/posts/{{page.title}}/socket.gif)

而 [WebSocket](http://www.websocket.org/) 则不同，它是一个完整的 [应用层协议](https://datatracker.ietf.org/doc/rfc6455/)，包含一套标准的 [API](https://html.spec.whatwg.org/multipage/web-sockets.html#network) 。

所以，从使用上来说，WebSocket 更易用，而 Socket 更灵活。


### 代表框架

>* 基于Scoket原生：代表框架 CocoaAsyncSocket。
>* 基于WebScoket：代表框架 SocketRocket。
>* 基于MQTT：代表框架 MQTTKit。
>* 基于XMPP：代表框架 XMPPFramework。


### 参考资源

>* [WebSocket 教程](http://www.ruanyifeng.com/blog/2017/05/websocket.html?utm_source=tuicool&utm_medium=referral)
>* [WebSocket wiki](https://en.wikipedia.org/wiki/WebSocket)
>* [Push technology wiki](https://en.wikipedia.org/wiki/Push_technology)
>* [Socket 与 WebSocket](https://blog.zengrong.net/post/2199.html)
>* [WebSocket 在线测试](http://www.blue-zero.com/WebSocket/)
>* [LuaSocket 初探](http://www.photoneray.com/luasocket/)
>* [一个LuaSocket封装](https://blog.zengrong.net/post/1980.html)
>* [cocos](http://forum.cocos.com/)
>* [lua-websockets](https://luarocks.org/modules/lipp/lua-websockets)
>* [FLSocketManager](https://github.com/gitkong/FLSocketManager)
>* [WebScoketTest](https://github.com/tuyaohui/IM_iOS/tree/master/iOS%E5%8D%B3%E6%97%B6%E9%80%9A%E8%AE%AF%EF%BC%8C%E4%BB%8E%E5%85%A5%E9%97%A8%E5%88%B0%E2%80%9C%E6%94%BE%E5%BC%83%E2%80%9D%EF%BC%9F/WebScoket(SRScoket)/WebScoketTest/WebScoketTest)

