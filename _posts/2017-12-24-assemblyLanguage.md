---
layout: post
title: assemblyLanguage
date: 2017-12-24
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

汇编语言是用助记符，符号和数字等来表示指令的程序设计语言，它与机器语言指令是一一对应的

>* 不同内核的CPU，必须有对应的汇编语言编译器将汇编语言编写的程序编译成对应CPU的机器语言代码，CPU才能正确识别和执行这些代码;不同架构的CPU的汇编指令集并不相同

>* 汇编的本质就是在（寄存器和寄存器）或 （寄存器和内存）之间来回move 数据.
```
汇编程序员可以使用指令来读写CPU中的寄存器，从而实现对于CPU的控制
```
![](/images/posts/{{page.title}}/control.png)


### 8086的寻址方式

>* 8086是用“基础地址（段地址×16） + 偏移地址 = 物理地址”的方式给出物理地址
![](/images/posts/{{page.title}}/x8086.png)
![](/images/posts/{{page.title}}/x8086Addressing.png)

>* 基于寻址方式，产生了段寄存器
```
 CS ―― 代码寄存器 
 DS ―― 数据寄存器  
 SS ―― 堆栈寄存器  
 ES ――  附加段器 
```
![](/images/posts/usefulCommand/register.png)

>* IP，FLAG 属于控制器
```
 IP――指令指针寄存器（Instruction Pointer），指示要执行指令所在存储单元的地址；IP寄存器是一个专用寄存器。
 FLAG――状态标志，只要控制内存是否溢出
```
>* 通用指令
```
  AX――累加器（Accumulator），使用频度最高，效率最高 
  BX――基址寄存器（Base Register），常存放存储器地址
  CX――计数器（Count Register），常作为计数器
  DX――数据寄存器（Data Register），存放数据
  SI――源变址寄存器（Source Index），常保存存储单元地址
  DI――目的变址寄存器（Destination Index），常保存存储单元地址
  BP――基址指针寄存器（Base Pointer），表示堆栈区域中的基地址
  SP――堆栈指针寄存器（Stack Pointer），指示堆栈区域的栈顶地址
```

>* 为了向下兼容 数据寄存器分为高8位和低8位
```
 AX 分为 AH ，AL（AH  表示高8位 ，AL  表示低8位）
```

### 常用指令

>* 常用指令
```
(1）  数据传送指令：MOV/XCHG，PUSH/POP，LEA
(2）  算数运算类指令：ADD/ADC/INC，SUB/SBB/DEC/CMP/NEG，MUL/IMUL，DIV/IDIV
(3）  位操作类指令：AND/OR/XOR/NOT/TEST
(4）  控制转移类指令：JMP/JCC/LOOP，CALL/RET，INT n
(5）  处理机控制类指令：NOP
```

### 参考

- [从汇编角度分析C语言的过程调用](http://blog.tingyun.com/web/article/detail/1132)
- [逆向分析](http://zhuanlan.freebuf.com/column/index/?name=%E9%80%86%E5%90%91%E5%88%86%E6%9E%90)
- [4hou.win](https://4hou.win/wordpress/?m=201712)
- [Hacking](https://4hou.win/wordpress/?p=6027)
- [iOS-Runtime-Headers](https://github.com/nst/iOS-Runtime-Headers)


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

>* 汇编码和机器码是1：1的关系



