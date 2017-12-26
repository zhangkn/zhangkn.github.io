---
layout: post
title: assemblyLanguage
date: 2017-12-24
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

>* 汇编就是在（寄存器和寄存器）或 （寄存器和内存）之间来回move 数据.
![](/images/posts/{{page.title}}/control.png)


### 8086的寻址方式

>* 8086是用“基础地址（段地址×16） + 偏移地址 = 物理地址”的方式给出物理地址
![](/images/posts/{{page.title}}/x8086.png)
![](/images/posts/{{page.title}}/x8086Addressing.png)

>* 基于寻址方式，产生了段寄存器
![](/images/posts/usefulCommand/register.png)

### 参考

- [从汇编角度分析C语言的过程调用](http://blog.tingyun.com/web/article/detail/1132)

### 附

>* iPhone ARM汇编架构

<table>
<thead>
<tr>
<th>架构</th>
<th>设备</th>
</tr>
</thead>
<tbody>
<tr>
<td>armv6</td>
<td>iPhone, iPhone2, iPhone3G, 第一代、第二代 iPod Touch</td>
</tr>
<tr>
<td>armv7</td>
<td>iPhone3GS, iPhone4, iPhone4S,iPad, iPad2, iPad3(The New iPad), iPad mini, iPod Touch 3G, iPod Touch4</td>
</tr>
<tr>
<td>armv7s</td>
<td>iPhone5, iPhone5C, iPad4(iPad with Retina Display)</td>
</tr>
<tr>
<td>arm64</td>
<td>iPhone6s , iphone6s plus,iPhone6, iPhone6 plus,iPhone5S ,iPad Air, iPad mini2</td>
</tr>
</tbody>
</table>


>* APP/程序的执行过程

![](/images/posts/{{page.title}}/app.png)
mach-o_execution:
![](/images/posts/{{page.title}}/mach-o_execution.png)



