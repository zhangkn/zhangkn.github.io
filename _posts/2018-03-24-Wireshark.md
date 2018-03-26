---
layout: post
title: Wireshark
date: 2018-03-24
tag: iOSre
site: https://zhangkn.github.io
---



### 前言


### 正文


### Remote Virtual Interface Tool 

>* rvictl -l

```
devzkndeMacBook-Pro:~ devzkn$ rvictl -l

Could not get list of devices

```

>*  rvictl -s UDID

```
devzkndeMacBook-Pro:~ devzkn$  rvictl -s  fa6770acd2e

Starting device fa6770acd2e0625c36a6f2a6c6 [SUCCEEDED] with interface rvi0
<!-- -x, -X		Stop a device or set of devices -->

```

### wireshark基本用法及过虑规则

>* !mdns

```
<!-- mDNS即组播DNS（multicast DNS）。使用5353端口，在内网没有DNS服务器时，就会出现此组播信息。 -->

less than 小于 < lt 

小于等于 le

等于 eq

大于 gt

大于等于 ge

不等 ne

<!-- !mdns && http && ip.dst_host eq 11.190.181.63  -->
<!-- !mdns && http.request.full_uri contains  "http://...com" -->


```

>* 过滤端口

```
tcp.port eq 443

```

>* 过滤IP

```
!mdns || ip.src_host eq 58.216.8.110 

```


### 分析L2TP 的 类型VPN 的连接和断开过程

>* 虚拟专用网（Virtual Private Network，VPN）的连接和断开分析

```
<!-- 1、连接的过程 -->
801	59.648579	192.168.2.61	114.114.114.114	DNS	81	Standard query 0xd39d A origin.guzzoni-apple.com.akadns.net
802	59.649083	192.168.2.61	114.114.114.114	DNS	59	Standard query 0x1828 A www.apple.com
803	59.649547	114.114.114.114	192.168.2.61	DNS	121	Standard query response 0xd39d A origin.guzzoni-apple.com.akadns.net CNAME sg01p01sa.guzzoni-apple.com.akadns.net A 17.252.172.5
804	59.650102	114.114.114.114	192.168.2.61	DNS	204	Standard query response 0x1828 A www.apple.com CNAME www.apple.com.edgekey.net CNAME www.apple.com.edgekey.net.globalredir.akadns.net CNAME e6858.e19.s.tl88.net A 27.148.139.136
<!-- 807	59.651656	192.168.2.61	114.114.114.114	DNS	55	Standard query 0xc06e A knip.com  就是VPN服务器-->
<!-- 808	59.652224	114.114.114.114	192.168.2.61	DNS	349	Standard query response 0xc06e A knip.com CNAME t-qc77a.knip.com A 61.160.210.234 A 61.160.210.241 A 58.216.8.107 A 61.160.210.254 A 61.160.210.235 A 58.216.8.118 A 61.160.210.214 A 61.160.233.223 A 61.160.210.244 A 58.216.8.117 A 61.160.233.239 A 61.160.233.222 A 61.160.210.223 A 61.160.210.229 A 61.160.210.221 A 61.160.210.222 A 61.160.233.240 -->
811	59.653845	192.168.2.61	61.160.210.221	ISAKMP	528	Identity Protection (Main Mode)
831	61.669742	192.168.2.61	61.160.210.221	ISAKMP	348	Quick Mode
834	61.672065	192.168.2.61	61.160.210.221	ESP	160	ESP (SPI=0x0ec12f58)

<!-- 2、断开VPN的过程 -->
29	2.030142	192.168.2.61	114.114.114.114	DNS	51	Standard query 0x553f SOA local
37	2.035028	114.114.114.114	192.168.2.61	DNS	126	Standard query response 0x553f No such name SOA local SOA a.root-servers.net
39	2.036104	192.168.2.61	114.114.114.114	DNS	63	Standard query 0x24ea A guzzoni.apple.com
40	2.036631	192.168.2.61	114.114.114.114	DNS	59	Standard query 0x04de A www.apple.com
41	2.037140	192.168.2.61	114.114.114.114	DNS	55	Standard query 0x2267 A apple.com
42	2.037740	114.114.114.114	192.168.2.61	DNS	152	Standard query response 0x24ea A guzzoni.apple.com CNAME origin.guzzoni-apple.com.akadns.net CNAME sg01p01sa.guzzoni-apple.com.akadns.net A 17.252.172.5
43	2.038264	114.114.114.114	192.168.2.61	DNS	204	Standard query response 0x04de A www.apple.com CNAME www.apple.com.edgekey.net CNAME www.apple.com.edgekey.net.globalredir.akadns.net CNAME e6858.e19.s.tl88.net A 27.148.139.136
44	2.038788	114.114.114.114	192.168.2.61	DNS	103	Standard query response 0x2267 A apple.com A 17.142.160.59 A 17.172.224.47 A 17.178.96.59
46	2.040273	192.168.2.61	61.160.210.221	ISAKMP	108	Informational

52	2.044263	192.168.2.61	114.114.114.114	DNS	71	Standard query 0x0680 A 35-courier.push.apple.com
53	2.044977	114.114.114.114	192.168.2.61	DNS	288	Standard query response 0x0680 A 35-courier.push.apple.com CNAME 35.courier-push-apple.com.akadns.net CNAME china-courier.push-apple.com.akadns.net A 17.252.156.218 A 17.252.156.53 A 17.252.156.51 A 17.252.156.37 A 17.252.157.22 A 17.252.156.63 A 17.252.157.30 A 17.252.156.221




79	2.062457	192.168.2.61	61.160.210.221	L2TP	70	Control Message - CDN (tunnel id=12764, session id=1)

```

>* 协议知识补充

```
<!-- ISAKMP：Internet Security Association and Key Management Protocol， -->

Internet 安全关联和密钥管理协议. 一种协议框架，定义了有效负载的格式、实现密钥交换协议的机制以及SA协商。 使用TCP和UDP的端口500，一般使用UDP。

<!-- ESP，封装安全载荷协议(Encapsulating SecurityPayloads)， -->
是一种Ipsec协议，用于对IP协议在传输过程中进行数据完整性度量、来源认证、加密以及防回放攻击。

<!-- IP in IP， 又被称为 ipencap，是将IP协议封装入传输用的IP协议的一个例子， -->

<!-- IP隧道是指一种可在两网络间用网际协议进行通信的通道。在该通道里，会先封装其他网络协议的数据包，之后再传输信息。 -->
若IP隧道与两个或多个IPSec一起使用时，可以创建虚拟专用网（Virtual Private Network，VPN）


<!-- SSL（Secure Sockets Layer，安全套接层），及其继任者TLS（Transport Layer Security，传输层安全） -->
 是为网络通信提供安全及数据完整性的一种安全协议。TLS与SSL在传输层对网络连接进行加密。
```

>* VPN 

```
1、Cisco IPSec，此协议通过密码、RSA SecurID 或 CRYPTOCard 进行用户认证，并通过共享密钥和证书进行机器认证。对于在设备配置期间指定的域，Cisco IPSec 支持“请求 VPN 域”。

2、L2TP/IPSec，此协议通过 MS-CHAPV2 密码、RSA SecurID 或 CRYPTOCard 进行用户认证，并通过共享密钥进行机器认证。

3、PPTP，此协议通过 MS-CHAPV2 密码和 RSA SecurID 或 CRYPTOCard 进行用户认证。

```



### see also

- [IOS高级开发～开机启动&无限后台运行&监听进程](http://www.cnblogs.com/abasolution/p/4108593.html)



- [ios 部署参考](http://help.apple.com/deployment/ios/#/iorce42113ea)

- [tcpdump](https://zhangkn.github.io/2018/03/tcpdump/)