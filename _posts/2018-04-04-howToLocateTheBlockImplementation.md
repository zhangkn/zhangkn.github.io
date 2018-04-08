---
layout: post
title: howToLocateTheBlockImplementation
date: 2018-04-04
tag: iOSre
site: https://zhangkn.github.io
---


### 前言


>* 分析方法


```
hopper + lldb + knhook + cycript + powerful-private-methods 

```

>* [dump](https://github.com/zhangkn/KNBin/blob/master/kndump)

```

即砸壳
```

>* [获取头文件](https://github.com/zhangkn/KNBin/blob/master/swiftOCclass-dump)

```

https://github.com/zhangkn/KNBin/blob/master/swiftOCclass-dump

可以class-dump混编的
```

>* [获取头文件，并简单的猜测使用了哪些第三方库CocoaPods](https://github.com/zhangkn/KNBin/blob/master/knhead)


## 正文


### howToLocateTheBlockImplementation


>*  br s -a 0x000aad7c+0x000b1000

```
(lldb)  br s -a 0x000aad7c+0x000b1000
Breakpoint 4: where = FaceBeautyV1PLUS`_mh_execute_header + 657772, address = 0x0015bd7c
Process 1141 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = breakpoint 4.1
    frame #0: 0x0015bd7c FaceBeautyV1PLUS`_mh_execute_header + 683388
FaceBeautyV1PLUS`_mh_execute_header:
->  0x15bd7c <+683388>: svcge  #0x3b5f0
    0x15bd80 <+683392>: stchi  p8, c15, [r4, #-308]
    0x15bd84 <+683396>: strmi  r11, [r4], -r7, lsl #1
    0x15bd88 <+683400>: vmin.s32 d4, d12, d0
Target 0: (FaceBeautyV1PLUS) stopped.


000aad7c         push       {r4, r5, r6, r7, lr}                                ; Objective C Implementation defined at 0xd2927c (instance method)
000aad7e         add        r7, sp, #0xc
000aad80         str        r8, [sp, #0xc + var_10]
000aad84         sub        sp, #0x1c
000aad86         mov        r4, r0
000aad88         mov        r0, r2         


(lldb)  po (char *)$r4
<NSInvocation: 0x17e756b0>
return value: {v} void
target: {@} 0x18bd9400
selector: {:} KNLog_executeSketch:
argument 2: {@?} 0x11335f8 (block)


 KNHooklog :-(void)executeSketch:(have 1 value)
	return:(null)
	value1:__NSMallocBlock__--><__NSMallocBlock__: 0x1a3da940>
	object:<WutaSketchPen: 0x18bd9400>
	 ##########################################

```

>* memory read –size 4 –format x 进行代码定位

```

(lldb)  memory read --size 4 --format x 0x1a3da940
0x1a3da940: 0x353fa0dc 0xc3000002 0x00000000 0x0054ed75
0x1a3da950: 0x00d897b4 0x192b3360 0x00000000 0x00020000
(lldb)  memory read --size 4 --format x 0x11335f8
0x011335f8: 0x353fa05c 0xc2000000 0x00000000 0x0054ed75
0x01133608: 0x00d897b4 0x192b3360 0x1a2bb4e0 0x18bd9400

0x0054ed75 - 0x000b1000 = 0x49DD75


```




### debugserver的开启与LLDB的连接

 


>* 1、 debugserver *:12345 -a knPenFaceBeauty  

```

<!-- debugserver host:port --attach=<process_name> -->


<!-- 在越狱设备中 -->
Taokeceshiji1:~ root# ps -e |grep knPenFaceBeauty
 1141 ??         0:11.73 /var/mobile/Containers/Bundle/Application/84053C29-8C7E-4BF0-AE53-5D461A1521B2/knPenFaceBeauty.app/knPenFaceBeauty
 1153 ttys000    0:00.01 grep knPenFaceBeauty
 <!-- 启动debugserver来监听来自任何IP地址的接入 ,iOS设备的接入端口是12345-->
Taokeceshiji1:~ root# debugserver *:12345 -a knPenFaceBeauty
debugserver-@(#)PROGRAM:debugserver  PROJECT:debugserver-320.2.89
 for armv7.
Attaching to process knPenFaceBeauty...
Listening to port 12345 for a connection from *...

debugserver-@(#)PROGRAM:debugserver  PROJECT:debugserver-320.2.89
 for armv7.
Attaching to process knPenFaceBeauty...
Listening to port 12345 for a connection from *...
Waiting for debugger instructions for process 0.


```


>* 2.LLDB连接debugserver----（1）进行端口的转发

```

LLDB连接debugserver可以使用WIFI进行连接，可是WIFI是不稳定的，而且特别的慢，所以此处我们要使用usbmuxd进行LLDB和debugserver的连接


<!-- （1）进行端口的转发-->


使用usbmuxd进行端口的转发，将上述的“12345”端口对接到Mac本地的某个端口， 

此处我们使用“12345”端口。进入到usbmuxd-1.0.8目录下的python-client下执行下方的命令

devzkndeMacBook-Pro:python-client devzkn$ python tcprelay.py -t 12345:12345
Forwarding local port 12345 to remote port 12345

Incoming connection to 12345
Waiting for devices...
Connecting to device <MuxDevice: ID 1307 ProdID 0x12a8 Serial 'fa6770acd2e0625c36a6f2a6c402e454bb0fdd96' Location 0x14210000>
Connection established, relaying data



<!-- 设置 alias:   devzkndeMacBook-Pro:~ devzkn$ open -e .bash_profile-->

alias port22='python ~/Downloads/kevinsoftware/ios-Reverse_Engineering/usbmuxd-1.0.8\ 2/python-client/tcprelay.py  -t 22:2222'

alias relay33='python ~/Downloads/kevin－software/ios-Reverse_Engineering/usbmuxd-1.0.8\ 2/python-client/tcprelay.py  -t 22:3333'
alias relay12345='python ~/Downloads/kevin－software/ios-Reverse_Engineering/usbmuxd-1.0.8\ 2/python-client/tcprelay.py  -t  12345:12345'

alias spro='source ~/.bash_profile'


<!-- devzkndeMacBook-Pro:~ devzkn$ relay12345 -->
Forwarding local port 12345 to remote port 12345

```

>* 2.LLDB连接debugserver---（2）Mac端LLDB的接入

```

在terminal上输入lldb命令，然后输入下方的地址进行连接。因为我们使用usbmuxd进行了端口的转发，因此可以使用本地的环回测试地址来进行debugserver的连接。

devzkndeMacBook-Pro:~ devzkn$ lldb
(lldb) process connect connect://127.0.0.1:12345
Process 1141 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
    frame #0: 0x32b45474 libsystem_kernel.dylib`mach_msg_trap + 20
libsystem_kernel.dylib`mach_msg_trap:
->  0x32b45474 <+20>: pop    {r4, r5, r6, r8}
    0x32b45478 <+24>: bx     lr

libsystem_kernel.dylib`mach_msg_overwrite_trap:
    0x32b4547c <+0>:  mov    r12, sp
    0x32b45480 <+4>:  push   {r4, r5, r6, r8}
Target 0: (knPenFaceBeauty) stopped.

```

>*  进程在虚拟内存相对于模块基地址的偏移量ASLR offset :image list -o -f 

```
image list -o -f |grep knPenFaceBeauty

(lldb) image list -o -f |grep knPenFaceBeauty
[  0] 0x000b1000 /private/var/mobile/Containers/Bundle/Application/84053C29-8C7E-4BF0-AE53-5D461A1521B2/knPenFaceBeauty.app/knPenFaceBeauty(0x00000000000b5000)


<!-- 基础概念 -->



1) 模块在内存中的起始地址----模块基地址,

2) ASLR偏移 ---- 虚拟内存起始地址与模块基地址的偏移量

左边 0x000b1000 就是ASLR偏移量（随机偏移量）,ASLR偏移量其实就是虚拟内存的启始地址，相对于模块基地址的偏移量

右边 0x00000000000b5000 的地址就是偏移后的地址。

<!-- MachO Header -->



        ; Segment __TEXT
        ; Range: [0x4000; 0xcc8000[ (13385728 bytes)
        ; File offset : [0; 13385728[ (13385728 bytes)
        ; Permissions: readable / executable

                             ; 
                             ; MachO Header
                             ; 
             __mh_execute_header:
00004000         struct __macho_header {                                        ; DATA XREF=-[WutaCamera getStandardPointsBinary]+38, -[WutaWidget configConMat]+450, -[WutaWidget configConMat]+1068, -[WutaWidget configConMat]+1722, -[WutaWidget configConMat]+2376, -[WutaWidget configConMat]+3030, -[WutaWidget configConMat]+3684, -[WutaWidget bindWidget:]+1462, -[WutaWidget bindWidget:]+1644, -[WutaWidget bindWidget:]+1862, -[WutaWidget detect]+5176, …
                     0xfeedface,                          // mach magic number identifier
                     0xc,                                 // cpu specifier
                     0x9,                                 // machine specifier
                     MH_EXECUTE,                          // type of file
                     61,                                  // number of load commands
                     6300,                                // the size of all the load commands
                     MH_NOUNDEFS|MH_DYLDLINK|MH_TWOLEVEL|MH_WEAK_DEFINES|MH_BINDS_TO_WEAK|MH_PIE // flags
                 }
                             ; 
                             ; Load Command 0
                             ; 

0x4000, 这个地址就是模块偏移前的地址，也就是模块在虚拟内存中的起始地址。

从Hopper中我们可以知道：模块偏移前的基地址=0x4000 

<!-- 计算方法 -->

　　模块偏移后的基地址（ 0x00000000000b5000 ）= ASLR偏移量（  0x000b1000  ）+ 模块偏移前基地址（ 0x4000 ）

<!-- ps: -->

1) Hopper中显示的都是“ 模块偏移前基地址”，而LLDB要操作的都是“模块偏移后的基地址”。 

2) Hopper与LLDB所选择的AMR架构的位数得一致，要么是32位，要么都是64位，如果位数不匹配的话，那么计算出来的内存地址肯定是不对的

```


### executeSketch 的分析

 "executeSketch:", 0                                 ; DATA XREF=-[SketchViewModel setTargetImage:]+324, 0xd2927c, 0xdff1b4

 [stack[2026] executeSketch:sp + 0x8, r3, stack[2016], stack[2017]];



>* hopper 中的代码

```
000aad7c         push       {r4, r5, r6, r7, lr}                                ; Objective C Implementation defined at 0xd2927c (instance method)
000aad7e         add        r7, sp, #0xc
000aad80         str        r8, [sp, #0xc + var_10]
000aad84         sub        sp, #0x1c
000aad86         mov        r4, r0
000aad88         mov        r0, r2                                              ; argument "instance" for method imp___picsymbolstub4__objc_retain
000aad8a         blx        imp___picsymbolstub4__objc_retain
000aad8e         mov        r5, r0
000aad90         movs       r0, #0x0                                            ; argument "label" for method imp___picsymbolstub4__dispatch_queue_create
000aad92         movs       r1, #0x0                                            ; argument "attr" for method imp___picsymbolstub4__dispatch_queue_create
000aad94         mov.w      r8, #0x0
000aad98         blx        imp___picsymbolstub4__dispatch_queue_create
000aad9c         mov        r6, r0
000aad9e         movw       r0, #0xd3f6                                         ; :lower16:(0xcc81a4 - 0xaadae)
000aada2         movt       r0, #0xc1                                           ; :upper16:(0xcc81a4 - 0xaadae)
000aada6         movw       r1, #0x51                                           ; :lower16:(0xaae09 - 0xaadb8)
000aadaa         add        r0, pc                                              ; __NSConcreteStackBlock_cc81a4
000aadac         movt       r1, #0x0                                            ; :upper16:(0xaae09 - 0xaadb8)
000aadb0         movw       r2, #0x1cc6                                         ; :lower16:(0xccca8c - 0xaadc6)
000aadb4         add        r1, pc                                              ; 0xaae09
000aadb6         movt       r2, #0xc2                                           ; :upper16:(0xccca8c - 0xaadc6)
000aadba         ldr        r0, [r0]                                            ; __NSConcreteStackBlock_cc81a4,__NSConcreteStackBlock
000aadbc         str        r0, [sp, #0x28 + var_28]
000aadbe         mov.w      r0, #0xc2000000
000aadc2         add        r2, pc                                              ; 0xccca8c
000aadc4         strd       r0, r8, [sp, #0x28 + var_24]
000aadc8         mov        r0, r4                                              ; argument "instance" for method imp___picsymbolstub4__objc_retain
000aadca         strd       r1, r2, [sp, #0x28 + var_1C]
000aadce         blx        imp___picsymbolstub4__objc_retain
000aadd2         strd       r0, r5, [sp, #0x28 + var_14]
000aadd6         mov        r0, r5                                              ; argument "instance" for method imp___picsymbolstub4__objc_retain
000aadd8         blx        imp___picsymbolstub4__objc_retain
000aaddc         mov        r4, r0
000aadde         mov        r1, sp                                              ; argument "block" for method imp___picsymbolstub4__dispatch_async
000aade0         mov        r0, r6                                              ; argument "queue" for method imp___picsymbolstub4__dispatch_async
000aade2         blx        imp___picsymbolstub4__dispatch_async
000aade6         mov        r0, r6                                              ; argument "instance" for method imp___picsymbolstub4__objc_release
000aade8         blx        imp___picsymbolstub4__objc_release
000aadec         ldr        r0, [sp, #0x28 + var_10]                            ; argument "instance" for method imp___picsymbolstub4__objc_release
000aadee         blx        imp___picsymbolstub4__objc_release
000aadf2         ldr        r0, [sp, #0x28 + var_14]                            ; argument "instance" for method imp___picsymbolstub4__objc_release
000aadf4         blx        imp___picsymbolstub4__objc_release
000aadf8         mov        r0, r4                                              ; argument "instance" for method imp___picsymbolstub4__objc_release
000aadfa         blx        imp___picsymbolstub4__objc_release
000aadfe         add        sp, #0x1c
000aae00         ldr        r8, [sp, #0xc + var_C], #0x4
000aae04         pop        {r4, r5, r6, r7, pc}
                        ; endp
000aae06         nop
```

>* 地址计算

```


             -[knPen executeSketch:]:
000aad7c         push       {r4, r5, r6, r7, lr} 


executeSketch 内存地址 = 000aad7c + 0x000b1000 （ALSR偏移） = 0x0015bd7c


```

>* 添加断点

```

(lldb)  br s -a 0x000aad7c+0x000b1000
Breakpoint 2: where = knPenFaceBeauty`_mh_execute_header + 657772, address = 0x0015bd7c

<!-- 删除（delete）编号为1的断点 -->
(lldb) br del 1
1 breakpoints deleted; 0 breakpoint locations disabled.


```

>* 运行app 触发断点 0x0015bd7c

```

Process 1141 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = breakpoint 2.1
    frame #0: 0x0015bd7c knPenFaceBeauty`_mh_execute_header + 683388
knPenFaceBeauty`_mh_execute_header:
->  0x15bd7c <+683388>: svcge  #0x3b5f0
    0x15bd80 <+683392>: stchi  p8, c15, [r4, #-308]
    0x15bd84 <+683396>: strmi  r11, [r4], -r7, lsl #1
    0x15bd88 <+683400>: vmin.s32 d4, d12, d0
Target 0: (knPenFaceBeauty) stopped.



000aad7c         push       {r4, r5, r6, r7, lr}                                ; Objective C Implementation defined at 0xd2927c (instance method)
000aad7e         add        r7, sp, #0xc
000aad80         str        r8, [sp, #0xc + var_10]
000aad84         sub        sp, #0x1c
000aad86         mov        r4, r0
000aad88         mov        r0, r2 



<!-- 对应的log -->

 [iTracer]: [knPen executeSketch:]: <type(@?)>


 KNHooklog :-(void)executeSketch:(have 1 value)
	return:(null)
	value1:__NSMallocBlock__--><__NSMallocBlock__: 0x1a5da3c0>
	object:<knPen: 0x18362200>
	 ##########################################

```

>* 3.输出寄存器的值(p, po)

```

(lldb)  po (char *)$sp
<NSBlock: 0x11332d0>

https://github.com/nst/iOS-Runtime-Headers/blob/master/Frameworks/CoreFoundation.framework/NSBlock.h


(lldb)  po (char *)$r0
<knPen: 0x18362200>



(lldb)  po (char *)$r2
<__NSStackBlock__: 0x1133518>


(lldb)  po (char *)$r3
<SketchViewModel: 0x192b3360>



(lldb) po $r4
<NSInvocation: 0x1a5c90b0>
return value: {v} void
target: {@} 0x18362200
selector: {:} KNLog_executeSketch:
argument 2: {@?} 0x1133518 (block)


(lldb) p (char *)$r1

<!-- 我们还可以将一个地址所存放的值进行打印，下方这个命令就是输出了$sp指针所指的地址处所存放的值： -->

(lldb) p/x $sp
(unsigned int) $2 = 0x039a3c74

<!-- 4.修改寄存器中的值 -->

register write 寄存器 值

<!-- register read --all -->

General Purpose Registers:
        r0 = 0x18362200
        r1 = 0x17d34d60
        r2 = 0x01133518
        r3 = 0x192b3360
        r4 = 0x1a5c90b0
        r5 = 0x1922bd00
        r6 = 0x1922bbc0
        r7 = 0x011332f4
        r8 = 0x32e61094  CoreFoundation`NSInvocation._frame
        r9 = 0x00000000
       r10 = 0x17e13a30
       r11 = 0x00000004
       r12 = 0x0151705c  (void *)0x3251d009: objc_autoreleasePoolPop + 1
        sp = 0x011332d0
        lr = 0x24255664  CoreFoundation`__invoking___ + 68
        pc = 0x0015bd7c  knPenFaceBeauty.__TEXT.__text + 657772
      cpsr = 0x600f0030

Floating Point Registers:
        s0 = 1.35632e-19
        s1 = 1.35632e-19
        s2 = 0
        s3 = 0
        s4 = 7.27059e+31
        s5 = 6.15111e+22
        s6 = 1.8589e+34
        s7 = 7.77666e+31
        s8 = 8.84369e-18
        s9 = 8.84369e-18
       s10 = 8.84369e-18
       s11 = 8.84369e-18
       s12 = 0
       s13 = 1.51473
       s14 = 2.68435e+08
       s15 = 24.0284
       s16 = 0
       s17 = 0
       s18 = 0
       s19 = 0
       s20 = 2.60083e+08
       s21 = 24.0284
       s22 = 0
       s23 = 0
       s24 = 0
       s25 = 0
       s26 = 0
       s27 = 0
       s28 = 0
       s29 = 0
       s30 = 0
       s31 = 0
        d0 = 6.01347001699907e-154
        d1 = 0
        d2 = 1.06381698331187e+180
        d3 = 9.80058440676473e+252
        d4 = 2.00877667922349e-139
        d5 = 2.00877667922349e-139
        d6 = 0.139732003211975
        d7 = 544504475
        d8 = 0
        d9 = 0
       d10 = 544504474.937768
       d11 = 0
       d12 = 0
       d13 = 0
       d14 = 0
       d15 = 0
       d16 = 0
       d17 = 0
       d18 = 0
       d19 = 0
       d20 = 1522811675
       d21 = 0
       d22 = -978307200
       d23 = 2.12199579145934e-314
       d24 = 0
       d25 = 0
       d26 = 0
       d27 = 0
       d28 = 0
       d29 = 0
       d30 = -485.5
       d31 = 1
        q0 = {0x20 0x20 0x20 0x20 0x20 0x20 0x20 0x20 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00}
        q1 = {0x53 0x6b 0x65 0x74 0x63 0x68 0x50 0x65 0x6e 0x20 0x65 0x78 0x65 0x63 0x75 0x74}
        q2 = {0x23 0x23 0x23 0x23 0x23 0x23 0x23 0x23 0x23 0x23 0x23 0x23 0x23 0x23 0x23 0x23}
        q3 = {0x00 0x00 0x00 0x00 0xbd 0xe2 0xc1 0x3f 0x00 0x00 0x80 0x4d 0x3d 0x3a 0xc0 0x41}
        q4 = {0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00}
        q5 = {0xc8 0x08 0x78 0x4d 0x3d 0x3a 0xc0 0x41 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00}
        q6 = {0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00}
        q7 = {0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00}
        q8 = {0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00}
        q9 = {0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00}
       q10 = {0x00 0x00 0xc0 0xc6 0x10 0xb1 0xd6 0x41 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00}
       q11 = {0x00 0x00 0x00 0x40 0xe4 0x27 0xcd 0xc1 0x01 0x00 0x00 0x00 0x01 0x00 0x00 0x00}
       q12 = {0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00}
       q13 = {0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00}
       q14 = {0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00}
       q15 = {0x00 0x00 0x00 0x00 0x00 0x58 0x7e 0xc0 0x00 0x00 0x00 0x00 0x00 0x00 0xf0 0x3f}
     fpscr = 0x8b00009f
  exception = 0x20202020
       fsr = 0x20202020
       far = 0x00000000

```


>* 4)、断点的单步执行（ni, si）


```

你可以通过nexti (简写：ni)和stepi (简写：si)来进行单步的调试。ni遇到跳转不会进入到跳转中去，而si则会跳转到相应的分支中去。

```

>* (5) 放开执行该断点（c）

```

Process 1141 resuming


```


>* (6)断点的禁用和开启

```
br dis 1 -- 禁用（disable）编号为1的断点

br en 1 -- 启用（enable）编号为1的断点

br dis  -- 禁用所有断点

br en  -- 启用所有断点

```

>* 7、删除breakpoints

```
(lldb) br dis
All breakpoints disabled. (5 breakpoints)
(lldb) br del
About to delete all breakpoints, do you want to do that?: [Y/n] y
All breakpoints removed. (5 breakpoints)

```


>* [知道__NSMallocBlock__的地址，如何查看具体的内容？](http://iosre.com/t/nsmallocblock/11448/2)

```

cy# [#0x17a0a930  _ivarDescription].toString()
`<__NSMallocBlock__: 0x17a0a930>:
in __NSMallocBlock__:
in __NSMallocBlock:
in NSBlock:
in NSObject:
\tisa (Class): __NSMallocBlock__`

[#0x17a0a930 _methodDescription].toString()

<!-- http://iosre.com/t/when-you-come-across---nsxxxblock---0xrandomnumber-while-debugging-a-block-in-lldb-how-to-locate-the-block/4977 -->

(lldb)  po (char *)$r2
<__NSStackBlock__: 0x1133518>


memory read --size 4  --format x 0x1133518

(lldb) memory read --size 4  --format x 0x1133518
0x01133518: 0x353fa05c 0xc2000000 0x00000000 0x0054ed75
0x01133528: 0x00d897b4 0x192b3360 0x1a5b4540 0x18362200


The value you get from 0x00000001022d4638 - ASLR offset is the address of the block. Go to this address in IDA or hopper then you'll see the block implementation.

0x0054ed75 - 0x000b1000 = 0x49DD75


int sub_49dd74(int arg0) {
    [*(arg0 + 0x14) processTheImage];
    [*(arg0 + 0x14) showLoading:0x0];
    r0 = loc_ad3970();
    return r0;
}

sub_49dd74:
push       {r4, r7, lr}
add        r7, sp, #0x4
movw       r1, #0x1424  ; :lower16:(0xdff1ac - 0x49dd88), &@selector(processTheImage)
mov        r4, r0
movt       r1, #0x96    ; :upper16:(0xdff1ac - 0x49dd88), &@selector(processTheImage)
ldr        r0, [r4, #0x14] ; argument "instance" for method imp___picsymbolstub4__objc_msgSend
add        r1, pc       ; &@selector(processTheImage)
ldr        r1, [r1]     ; argument "selector" for method imp___picsymbolstub4__objc_msgSend, "processTheImage",@selector(processTheImage)
blx        imp___picsymbolstub4__objc_msgSend
movw       r0, #0x140a  ; :lower16:(0xdff1a4 - 0x49dd9a), &@selector(showLoading:)
movs       r2, #0x0
movt       r0, #0x96    ; :upper16:(0xdff1a4 - 0x49dd9a), &@selector(showLoading:)
add        r0, pc       ; &@selector(showLoading:)
ldr        r1, [r0]     ; argument "selector" for method imp___picsymbolstub4__objc_msgSend, "showLoading:",@selector(showLoading:)
ldr        r0, [r4, #0x14] ; argument "instance" for method imp___picsymbolstub4__objc_msgSend
blx        imp___picsymbolstub4__objc_msgSend
movw       r0, #0x1404  ; :lower16:(0xdff1b0 - 0x49ddac), &@selector(refreshImage)
movt       r0, #0x96    ; :upper16:(0xdff1b0 - 0x49ddac), &@selector(refreshImage)
add        r0, pc       ; &@selector(refreshImage)
ldr        r1, [r0]     ; "refreshImage",@selector(refreshImage)
ldr        r0, [r4, #0x14]
pop.w      {r4, r7, lr}
b.w        sub_ad36e2+654

void -[SketchViewModel processTheImage](void * self, void * _cmd) {

knPen executeSketch -> SketchViewModel processTheImage

<!-- 使用快捷键 G   进行地址定位，或者偏移量定位 -->
```

>* 小结

```
<!-- block的内容 -->
   -[knPen executeSketch:]:

1） 从log 来看，参数分析的地址有两个

 KNHooklog :-(void)executeSketch:(have 1 value)
	return:(null)
	value1:__NSMallocBlock__--><__NSMallocBlock__: 0x1a5da3c0>
	object:<knPen: 0x18362200>
	 ##########################################

(lldb) po $r4
<NSInvocation: 0x1a5c90b0>
return value: {v} void
target: {@} 0x18362200
selector: {:} KNLog_executeSketch:
argument 2: {@?} 0x1133518 (block)


(lldb) memory read --size 4  --format x 0x1a5da3c0
0x1a5da3c0: 0x353fa0dc 0xc3000002 0x00000000 0x0054ed75
0x1a5da3d0: 0x00d897b4 0x192b3360 0x00000000 0x00020000
(lldb) memory read --size 4  --format x 0x1133518
0x01133518: 0x353fa05c 0xc2000000 0x00000000 0x0054ed75
0x01133528: 0x00d897b4 0x192b3360 0x1a5b4540 0x18362200

其中公共的一个地址就是0x0054ed75 

0x0054ed75 - 0x000b1000 = 0x49DD75


2） 0x49DD75 对应的code 如下

int sub_49dd74(int arg0) {
    [*(arg0 + 0x14) processTheImage];
    [*(arg0 + 0x14) showLoading:0x0];
    r0 = loc_ad3970();
    return r0;
}

<!-- 因此得知- (void)executeSketch:(CDUnknownBlockType)arg1;的block的真实面目 -->


int sub_49dd74(int arg0) {//arg0 == SketchViewModel的对象
//block的代码逻辑就是处理SketchViewModel
    [*(arg0 + 0x14) processTheImage];//SketchViewModel processTheImage
    [*(arg0 + 0x14) showLoading:0x0];
    r0 = loc_ad3970();
    return r0;// block的返回值 
}

<!-- loc_ad3970(); -->

int EntryPoint_42() {
    r8 = [[NSAutoreleasePool alloc] init];
    r0 = objc_getClass("WBSDKJKArray");
    *0xe331b0 = r0;
    r0 = class_getInstanceSize(r0);
    if (r0 < 0x10) {
            asm { movslo     r0, #0x10 };
    }
    *0xe331b4 = r0;
    r0 = objc_getClass("WBSDKJKDictionary");
    *0xe331b8 = r0;
    r0 = class_getInstanceSize(r0);
    if (r0 < 0x10) {
            asm { movslo     r0, #0x10 };
    }
    *0xe331bc = r0;
    *0xe331c0 = objc_msgSend(@class(NSNumber), *@selector(class));
    *0xe331c4 = [NSNumber methodForSelector:@selector(alloc)];
    r6 = [NSNumber alloc];
    *0xe331c8 = [r6 methodForSelector:@selector(initWithUnsignedLongLong:)];
    [[r6 init] release];
    r0 = r8;
    r1 = @selector(release);
    r0 = objc_msgSend(r0, r1);
    return r0;//返回一个WBSDKJKArray 数组
}

<!-- 分析思路 -->
分析汇编代码时不一定要细致到一条一条分析的粒度，因为很多汇编代码只是做了常规操作，对实际执行的函数结果影响不大。

直接定位各种函数调用（BLX，BL等），对二进制文件中一段代码里的函数调用顺序形成一个大致的了解后，再进一步分析函数周围的汇编代码，看看各种参数是如何形成的

blx        imp___picsymbolstub4__objc_msgSend


```




###  第二次分析 结合私有方法CycriptTricks

lldb 相比于cycript 使用Powerful private methods 的好处是，不需要使用toString() 进行格式化。

>* [CycriptTricks](https://zhangkn.github.io/2017/12/CycriptTricks/)

```
_ivarDescription
_shortMethodDescription
nextResponder
_autolayoutTrace
recursiveDescription
_methodDescription
```

>* br s -a 0x000aad7c+0x000b1000

```

(lldb)  br s -a 0x000aad7c+0x000b1000
Breakpoint 4: where = FaceBeautyV1PLUS`_mh_execute_header + 657772, address = 0x0015bd7c
Process 1141 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = breakpoint 4.1
    frame #0: 0x0015bd7c FaceBeautyV1PLUS`_mh_execute_header + 683388
FaceBeautyV1PLUS`_mh_execute_header:
->  0x15bd7c <+683388>: svcge  #0x3b5f0
    0x15bd80 <+683392>: stchi  p8, c15, [r4, #-308]
    0x15bd84 <+683396>: strmi  r11, [r4], -r7, lsl #1
    0x15bd88 <+683400>: vmin.s32 d4, d12, d0
Target 0: (FaceBeautyV1PLUS) stopped.


(lldb)  po (char *)$r0
<WutaSketchPen: 0x18bd9400>

(lldb)  po [0x18bd9400 originImage]
<UIImage: 0x1a2bb4e0> size {960, 1280} orientation 0 scale 1.000000

(lldb)  po (char *)$r2
<__NSStackBlock__: 0x11335f8>


 KNHooklog :-(void)executeSketch:(have 1 value)
	return:(null)
	value1:__NSMallocBlock__--><__NSMallocBlock__: 0x1a3da940>
	object:<WutaSketchPen: 0x18bd9400>
	 ##########################################



(lldb)  po (char *)$r3
<SketchViewModel: 0x192b3360>


(lldb)  po [0x192b3360 signatureTitle]
相机




```

>* _ivarDescription

```
(lldb)  po [0x192b3360 _ivarDescription]
<SketchViewModel: 0x192b3360>:
in SketchViewModel:
	_dirty (BOOL): 1
	_targetImage (UIImage*): <UIImage: 0x1a2bb4e0>
	_delegate (<SketchViewModelDelegate>*): <SketchViewController: 0x191d4e20>
	_signatureDate (NSDate*): <__NSDate: 0x1a39e720>
	_signatureTitle (NSString*): @"相机"<__NSCFString: 0x17f682f0>
	_type (unsigned int): 0
	_controller (SketchViewController*): <SketchViewController: 0x191d4e20>
	_sketchPen (WutaSketchPen*): <WutaSketchPen: 0x18bd9400>
	_switchInnerData (NSArray*): <__NSArrayI: 0x17e2ef50>
	_processedImages (NSArray*): <__NSArrayM: 0x1a220930>
	_processedImagesSaved (NSMutableArray*): <__NSArrayM: 0x1a2176d0>
in NSObject:
	isa (Class): SketchViewModel

```

>* _shortMethodDescription

```

(lldb) po [0x18bd9400 _shortMethodDescription]
<WutaSketchPen: 0x18bd9400>:
in WutaSketchPen:
	Properties:
		@property (retain, nonatomic) NSMutableArray* sketchResults;  (@synthesize sketchResults = _sketchResults;)
		@property (retain, nonatomic) UIImage* originImage;  (@synthesize originImage = _originImage;)
	Instance Methods:
		- (void) forwardInvocation:(id)arg1; (0x3074019)
		- (id) KNLog_initWithImage:(id)arg1; (0x12fc7e8)
		- (id) KNLog_executeSign:(id)arg1; (0x12fc7bc)
		- (void) KNLog_executeSketch:(^block)arg1; (0x12fc790)
		- (id) KNLog_MatToUIImage:(Mat)arg1; (0x12fc764)
		- (id) KNLog_sketchResults; (0x12fc738)
		- (void) KNLog_setSketchResults:(id)arg1; (0x12fc70c)
		- (id) KNLog_originImage; (0x12fc6e0)
		- (void) KNLog_setOriginImage:(id)arg1; (0x12fc6b4)
		- (void) setOriginImage:(id)arg1; (0x3250f261)
		- (id) originImage; (0x3250f261)
		- (void) setSketchResults:(id)arg1; (0x3250f261)
		- (id) sketchResults; (0x3250f261)
		- (id) MatToUIImage:(Mat)arg1; (0x3250f261)
		- (void) executeSketch:(^block)arg1; (0x3250f261)
		- (id) executeSign:(id)arg1; (0x3250f261)
		- (id) initWithImage:(id)arg1; (0x3250f261)
		- (id) .cxx_construct; (0x12fc814)
		- (void) .cxx_destruct; (0x12fc840)
(NSObject ...)
```


>*  [[[UIWindow keyWindow] rootViewController] _printHierarchy]  -- 快捷的获取ViewController


```
(lldb) po [[[UIWindow keyWindow] rootViewController] _printHierarchy]
<UINavigationController 0x191a1a70>, state: disappeared, view: <UILayoutContainerView 0x191a2370> not in the window
   | <IndexViewController 0x192d8050>, state: disappeared, view: <UIView 0x191d9280> not in the window
   | <SketchViewController 0x191d4e20>, state: disappeared, view: <UIView 0x1a27a4e0> not in the window
   + <UIImagePickerController 0x18ba5c00>, state: appeared, view: <UILayoutContainerView 0x1a23e180>, presented with: <_UIFullscreenPresentationController 0x1a2410a0>
   |    | <PUUIAlbumListViewController 0x18ac5c00>, state: disappeared, view: <UIView 0x1911f010> not in the window
   |    | <PUUIPhotosAlbumViewController 0x18af1a00>, state: appeared, view: <UICollectionViewControllerWrapperView 0x17e37890>

```

>* bundleIdentifier

```
(lldb) po [[NSBundle mainBundle] bundleIdentifier]
com.获取.bundleIdentifier
```

>* UIApp

```
(lldb) po [UIApp description]
<UIApplication: 0x17e4a720>

```

>* nextResponder

```
(lldb) po [0x17e4a720 nextResponder]
<AppDelegate: 0x17e3d1b0>
```

>* [[0x17e4a720 keyWindow]recursiveDescription]

```
(lldb)  po [[0x17e4a720 keyWindow]recursiveDescription]
<UIWindow: 0x192d6e80; frame = (0 0; 320 568); gestureRecognizers = <NSArray: 0x192d74b0>; layer = <UIWindowLayer: 0x192d71e0>>
   | <UITransitionView: 0x1a297590; frame = (0 0; 320 568); autoresize = W+H; layer = <CALayer: 0x1911e060>>
   |    | <UILayoutContainerView: 0x1a23e180; frame = (0 0; 320 568); autoresize = W+H; gestureRecognizers = <NSArray: 0x191a8850>; layer = <CALayer: 0x1a2427b0>>
   |    |    | <UINavigationTransitionView: 0x1a244f50; frame = (0 0; 320 568); clipsToBounds = YES; autoresize = W+H; layer = <CALayer: 0x1a22eec0>>
   |    |    |    | <UIViewControllerWrapperView: 0x1922d180; frame = (0 0; 320 568); autoresize = W+H; layer = <CALayer: 0x1a3d9560>>
   |    |    |    |    | <UICollectionViewControllerWrapperView: 0x17e37890; frame = (0 0; 320 568); autoresize = W+H; gestureRecognizers = <NSArray: 0x192257c0>; layer = <CALayer: 0x19225e20>>
   |    |    |    |    |    | <PUCollectionView: 0x1834e400; baseClass = UICollectionView; frame = (0 0; 320 568); clipsToBounds = YES; autoresize = W+H; gestureRecognizers = <NSArray: 0x1a378a10>; layer = <CALayer: 0x17f7a9b0>; contentOffset: {0, -44}; contentSize: {320, 333.5}> collection view layout: <PUSectionedGridLayout: 0x1a2a13f0> delegate: <PUUIPhotosAlbumViewController: 0x18af1a00>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x192aa210; baseClass = UICollectionViewCell; frame = (0 9.5; 78.5 78.5); layer = <CALayer: 0x1a39aaa0>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a3eb820; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x19216ad0>; layer = <CALayer: 0x17e69420>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a4545e0; baseClass = UICollectionViewCell; frame = (80.5 9.5; 78.5 78.5); layer = <CALayer: 0x191162e0>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a281c40; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1a226ae0>; layer = <CALayer: 0x1910e1f0>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a685fe0; baseClass = UICollectionViewCell; frame = (161 9.5; 78.5 78.5); layer = <CALayer: 0x1a295370>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1910af30; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1911c0e0>; layer = <CALayer: 0x19116450>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a686110; baseClass = UICollectionViewCell; frame = (241.5 9.5; 78.5 78.5); layer = <CALayer: 0x1a226730>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a249bb0; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x19144aa0>; layer = <CALayer: 0x1911e1d0>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a45add0; baseClass = UICollectionViewCell; frame = (0 90; 78.5 78.5); layer = <CALayer: 0x1911fd90>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a27bcc0; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1a45af10>; layer = <CALayer: 0x19123640>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a2a1d30; baseClass = UICollectionViewCell; frame = (80.5 90; 78.5 78.5); layer = <CALayer: 0x1a2a1e30>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x19122100; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1a24d040>; layer = <CALayer: 0x1a2a1e60>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a36f710; baseClass = UICollectionViewCell; frame = (161 90; 78.5 78.5); layer = <CALayer: 0x1a39e6d0>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a3f0360; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1a3f04c0>; layer = <CALayer: 0x192279d0>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a7738e0; baseClass = UICollectionViewCell; frame = (241.5 90; 78.5 78.5); layer = <CALayer: 0x19239430>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a3f4e90; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1a24de50>; layer = <CALayer: 0x1a5cd9b0>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a3956f0; baseClass = UICollectionViewCell; frame = (0 170.5; 78.5 78.5); layer = <CALayer: 0x1a5ce4d0>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a23ece0; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1a23f600>; layer = <CALayer: 0x1a23f5d0>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a68a8d0; baseClass = UICollectionViewCell; frame = (80.5 170.5; 78.5 78.5); layer = <CALayer: 0x19206920>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a3f6e80; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1a3d4850>; layer = <CALayer: 0x19207bb0>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a565260; baseClass = UICollectionViewCell; frame = (161 170.5; 78.5 78.5); layer = <CALayer: 0x1a387140>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a3f56f0; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1a388410>; layer = <CALayer: 0x19208fa0>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a39d260; baseClass = UICollectionViewCell; frame = (241.5 170.5; 78.5 78.5); layer = <CALayer: 0x1a5be8f0>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a3f7770; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1a35c010>; layer = <CALayer: 0x1920c0b0>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a4b3a80; baseClass = UICollectionViewCell; frame = (0 251; 78.5 78.5); layer = <CALayer: 0x1911acb0>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x191e3ec0; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1a433ca0>; layer = <CALayer: 0x191e3f60>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a4aef60; baseClass = UICollectionViewCell; frame = (80.5 251; 78.5 78.5); layer = <CALayer: 0x191f7280>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a2c4dc0; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x1a29e9d0>; layer = <CALayer: 0x1a2c4e60>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x192cff40; baseClass = UICollectionViewCell; frame = (161 251; 78.5 78.5); layer = <CALayer: 0x1a3f6a10>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x1a3fae30; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x191f7220>; layer = <CALayer: 0x1a34ee80>>
   |    |    |    |    |    |    | <PUPhotosGridCell: 0x1a2104e0; baseClass = UICollectionViewCell; frame = (241.5 251; 78.5 78.5); layer = <CALayer: 0x191dfe40>>
   |    |    |    |    |    |    |    | <PUPhotoView: 0x191d76e0; frame = (0 0; 78.5 78.5); clipsToBounds = YES; gestureRecognizers = <NSArray: 0x19070610>; layer = <CALayer: 0x191d7780>>
   |    |    |    |    |    |    |    | <UIView: 0x1a3fa0d0; frame = (0 0; 78.5 78.5); alpha = 0.5; hidden = YES; userInteractionEnabled = NO; layer = <CALayer: 0x1a34a8b0>>
   |    |    |    |    |    |    | <UIImageView: 0x1a28b700; frame = (317.5 480; 2.5 44); alpha = 0; opaque = NO; autoresize = LM; userInteractionEnabled = NO; layer = <CALayer: 0x1a245da0>>
   |    |    |    |    |    |    | <UIImageView: 0x1a2a0710; frame = (313 521.5; 7 2.5); alpha = 0; opaque = NO; autoresize = TM; userInteractionEnabled = NO; layer = <CALayer: 0x191cddb0>>
   |    |    | <UINavigationBar: 0x1a2a0f80; frame = (0 0; 320 44); autoresize = W; gestureRecognizers = <NSArray: 0x191bbf20>; layer = <CALayer: 0x1a2a8610>>
   |    |    |    | <_UINavigationBarBackground: 0x1a29e340; frame = (0 0; 320 44); autoresize = W; userInteractionEnabled = NO; layer = <CALayer: 0x1a2ac8b0>>
   |    |    |    |    | <UIImageView: 0x1a23db50; frame = (0 44; 320 0.5); userInteractionEnabled = NO; layer = <CALayer: 0x191d69d0>>
   |    |    |    | <UINavigationItemView: 0x1911bb10; frame = (126 8; 68 27); opaque = NO; userInteractionEnabled = NO; layer = <CALayer: 0x191195e0>>
   |    |    |    |    | <UILabel: 0x1a211f10; frame = (0 3.5; 68 21.5); text = '相机胶卷'; opaque = NO; userInteractionEnabled = NO; layer = <_UILabelLayer: 0x1a29b310>>
   |    |    |    |    |    | <_UILabelContentLayer: 0x1a5bc3a0> (layer)
   |    |    |    | <UINavigationButton: 0x1a29d2e0; frame = (278 7; 34 30); opaque = NO; layer = <CALayer: 0x1a29b030>>
   |    |    |    |    | <UIButtonLabel: 0x1a36f0e0; frame = (0 5; 34 20.5); text = '取消'; opaque = NO; userInteractionEnabled = NO; layer = <_UILabelLayer: 0x1a5bb8c0>>
   |    |    |    |    |    | <_UILabelContentLayer: 0x1a3f9be0> (layer)
   |    |    |    | <UINavigationItemButtonView: 0x1a4640f0; frame = (8 6; 53 30); opaque = NO; userInteractionEnabled = NO; layer = <CALayer: 0x1a24a790>>
   |    |    |    |    | <UILabel: 0x1a45cc50; frame = (19 5.5; 34 21.5); text = '照片'; opaque = NO; userInteractionEnabled = NO; layer = <_UILabelLayer: 0x1a661bc0>>
   |    |    |    |    |    | <_UILabelContentLayer: 0x17e15fc0> (layer)
   |    |    |    | <_UINavigationBarBackIndicatorView: 0x1a23df60; frame = (8 11.5; 13 21); opaque = NO; userInteractionEnabled = NO; layer = <CALayer: 0x1a2b2800>>

```


>*  memory read --size 4 --format x 进行代码定位

```

(lldb)  memory read --size 4 --format x 0x1a3da940
0x1a3da940: 0x353fa0dc 0xc3000002 0x00000000 0x0054ed75
0x1a3da950: 0x00d897b4 0x192b3360 0x00000000 0x00020000
(lldb)  memory read --size 4 --format x 0x11335f8
0x011335f8: 0x353fa05c 0xc2000000 0x00000000 0x0054ed75
0x01133608: 0x00d897b4 0x192b3360 0x1a2bb4e0 0x18bd9400


0x0054ed75 - 0x000b1000 = 0x49DD75


int sub_49dd74(int arg0) {
    [*(arg0 + 0x14) processTheImage];
    [*(arg0 + 0x14) showLoading:0x0];
    r0 = loc_ad3970();
    return r0;
}


(lldb) po 0x11335f8
<__NSStackBlock__: 0x11335f8>

(lldb) po 0x1a3da940
<__NSMallocBlock__: 0x1a3da940>


(lldb) po [0x11335f8 _ivarDescription]
<__NSStackBlock__: 0x11335f8>:
in __NSStackBlock__:
in __NSStackBlock:
in NSBlock:
in NSObject:
	isa (Class): __NSStackBlock__


Note, for arm64, the 2nd command is memory read --size 8, while for armv7/armv7s it should be memory read --size 4.


The value you get from 0x00000001022d4638 - ASLR offset is the address of the block. Go to this address in IDA or hopper then you'll see the block implementation.


```


### 使用_shortMethodDescription方法，获取方法的地址，避免计算

>* 使用cycript 执行 _shortMethodDescription 

```
cy#  [WutaSketchPen _shortMethodDescription].toString()
`<WutaSketchPen: 0xec7fa0>:
in WutaSketchPen:
\tProperties:
\t\t@property (retain, nonatomic) NSMutableArray* sketchResults;  (@synthesize sketchResults = _sketchResults;)
\t\t@property (retain, nonatomic) UIImage* originImage;  (@synthesize originImage = _originImage;)
\tInstance Methods:
\t\t- (void) forwardInvocation:(id)arg1; (0x3074019)
\t\t- (id) KNLog_initWithImage:(id)arg1; (0x12fc7e8)
\t\t- (id) KNLog_executeSign:(id)arg1; (0x12fc7bc)
\t\t- (void) KNLog_executeSketch:(^block)arg1; (0x12fc790)
\t\t- (id) KNLog_MatToUIImage:(Mat)arg1; (0x12fc764)
\t\t- (id) KNLog_sketchResults; (0x12fc738)
\t\t- (void) KNLog_setSketchResults:(id)arg1; (0x12fc70c)
\t\t- (id) KNLog_originImage; (0x12fc6e0)
\t\t- (void) KNLog_setOriginImage:(id)arg1; (0x12fc6b4)
\t\t- (void) setOriginImage:(id)arg1; (0x3250f261)
\t\t- (id) originImage; (0x3250f261)
\t\t- (void) setSketchResults:(id)arg1; (0x3250f261)
\t\t- (id) sketchResults; (0x3250f261)
\t\t- (id) MatToUIImage:(Mat)arg1; (0x3250f261)
\t\t- (void) executeSketch:(^block)arg1; (0x3250f261)
\t\t- (id) executeSign:(id)arg1; (0x3250f261)
\t\t- (id) initWithImage:(id)arg1; (0x3250f261)
\t\t- (id) .cxx_construct; (0x12fc814)
\t\t- (void) .cxx_destruct; (0x12fc840)
(NSObject ...)`


<!-- \t\t- (void) KNLog_executeSketch:(^block)arg1; (0x12fc790) -->
<!-- \t\t- (void) executeSketch:(^block)arg1; (0x3250f261) -->

<!-- \t\t- (id) executeSign:(id)arg1; (0x3250f261) -->

```


>* 使用lldb  执行私有方法

```
(lldb) po [WutaSketchPen _shortMethodDescription]
error: Process is running.  Use 'process interrupt' to pause execution.
(lldb) process interrupt
Process 1141 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
    frame #0: 0x32b45474 libsystem_kernel.dylib`mach_msg_trap + 20
libsystem_kernel.dylib`mach_msg_trap:
->  0x32b45474 <+20>: pop    {r4, r5, r6, r8}
    0x32b45478 <+24>: bx     lr

libsystem_kernel.dylib`mach_msg_overwrite_trap:
    0x32b4547c <+0>:  mov    r12, sp
    0x32b45480 <+4>:  push   {r4, r5, r6, r8}
Target 0: (FaceBeautyV1PLUS) stopped.

只有在进入interrupt的时候才能执行

(lldb) po [WutaSketchPen _shortMethodDescription]
<WutaSketchPen: 0xec7fa0>:
in WutaSketchPen:
	Properties:
		@property (retain, nonatomic) NSMutableArray* sketchResults;  (@synthesize sketchResults = _sketchResults;)
		@property (retain, nonatomic) UIImage* originImage;  (@synthesize originImage = _originImage;)
	Instance Methods:
		- (void) forwardInvocation:(id)arg1; (0x3074019)
		- (id) KNLog_initWithImage:(id)arg1; (0x12fc7e8)
		- (id) KNLog_executeSign:(id)arg1; (0x12fc7bc)
		- (void) KNLog_executeSketch:(^block)arg1; (0x12fc790)
		- (id) KNLog_MatToUIImage:(Mat)arg1; (0x12fc764)
		- (id) KNLog_sketchResults; (0x12fc738)
		- (void) KNLog_setSketchResults:(id)arg1; (0x12fc70c)
		- (id) KNLog_originImage; (0x12fc6e0)
		- (void) KNLog_setOriginImage:(id)arg1; (0x12fc6b4)
		- (void) setOriginImage:(id)arg1; (0x3250f261)
		- (id) originImage; (0x3250f261)
		- (void) setSketchResults:(id)arg1; (0x3250f261)
		- (id) sketchResults; (0x3250f261)
		- (id) MatToUIImage:(Mat)arg1; (0x3250f261)
		- (void) executeSketch:(^block)arg1; (0x3250f261)
		- (id) executeSign:(id)arg1; (0x3250f261)
		- (id) initWithImage:(id)arg1; (0x3250f261)
		- (id) .cxx_construct; (0x12fc814)
		- (void) .cxx_destruct; (0x12fc840)
(NSObject ...)


		<!-- - (void) KNLog_executeSketch:(^block)arg1; (0x12fc790) -->
				<!-- - (void) executeSketch:(^block)arg1; (0x3250f261) -->
				<!-- - (id) executeSign:(id)arg1; (0x3250f261) -->

				同样的得到了executeSketch的地址




```


>* 进行打断点

```

(lldb) b 0x3250f261
Breakpoint 3: where = libobjc.A.dylib`_objc_msgForward + 1, address = 0x3250f261



(lldb) c
Process 1141 resuming
Process 1141 stopped
* thread #1, queue = 'NSPersistentStoreCoordinator 0x1a334270', stop reason = breakpoint 3.1
    frame #0: 0x3250f260 libobjc.A.dylib`_objc_msgForward
libobjc.A.dylib`_objc_msgForward:
->  0x3250f260 <+0>:  movw   r12, #0x6eb8
    0x3250f264 <+4>:  movt   r12, #0x2ea
    0x3250f268 <+8>:  add    r12, pc
    0x3250f26a <+10>: ldr.w  r12, [r12]
Target 0: (FaceBeautyV1PLUS) stopped.


```




### 我个人常用的分析工具


>* [dump](https://github.com/zhangkn/KNBin/blob/master/kndump)

```

即砸壳
```

>* [获取头文件](https://github.com/zhangkn/KNBin/blob/master/swiftOCclass-dump)

```

https://github.com/zhangkn/KNBin/blob/master/swiftOCclass-dump

可以dump混编的
```


### see also

- [追踪 Objective-C 方法中的 Block 参数对象](http://yulingtianxia.com/blog/2018/03/31/Track-Block-Arguments-of-Objective-C-Method/)

```

https://github.com/yulingtianxia/BlockHook

https://github.com/yulingtianxia/BlockTracker

```

- [supotato](https://github.com/GPUImage/supotato)

```

<!-- supotato 可以根据头文件的前2个字符形成个简单的分类报告。同时可以猜测出使用了哪些第三方库（CocoaPods） -->


<!-- error: could not create '/Library/Python/2.7/site-packages/supotato': Permission denied -->

devzkndeMacBook-Pro:supotato devzkn$ sudo  pip install supotato
Password:

Collecting supotato
  Retrying (Retry(total=4, connect=None, read=None, redirect=None)) after connection broken by 'NewConnectionError('<pip._vendor.requests.packages.urllib3.connection.VerifiedHTTPSConnection object at 0x106656a90>: Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known',)': /simple/supotato/
  Downloading supotato-2.0.4.tar.gz
Requirement already satisfied: six>=1.10.0 in /Library/Python/2.7/site-packages (from supotato)
Installing collected packages: supotato
  Running setup.py install for supotato ... done
Successfully installed supotato-2.0.4


<!-- devzkndeMacBook-Pro:~ devzkn$ supotato -i /Users/devzkn/decrypted/FaceBeautyV1PLUS/FaceBeautyV1PLUShead/FaceBeautyV1PLUShead/head  -o  /Users/devzkn/decrypted/FaceBeautyV1PLUS/FaceBeautyV1PLUShead/FaceBeautyV1PLUShead -->
CocoaPods Index Database not exist , will download from https://everettjf.github.io/app/supotato/supotato.db
Please wait for a moment (about 5 MB file will be downloaded)
Download completed
Start analyzing directory:/Users/devzkn/decrypted/FaceBeautyV1PLUS/FaceBeautyV1PLUShead/FaceBeautyV1PLUShead/head
CocoaPods (39) : 
    GCDWebServer
        https://github.com/swisspol/GCDWebServer
        Lightweight GCD based HTTP server for OS X & iOS (includes web based uploader & WebDAV server)
    JPSVolumeButtonHandler
        https://github.com/jpsim/JPSVolumeButtonHandler
        JPSVolumeButtonHandler provides an easy block interface to hardware volume buttons on iOS devices. Perfect for camera apps!
    AliyunOSSiOS
        https://github.com/aliyun/AliyunOSSiOS
        An iOS SDK for Aliyun Object Storage Service
    RYUtils
        https://github.com/sudotamm/RYUtils
        Ryan Yuan's private library for Objective-C.
    AFImageDownloader
        https://github.com/AshFurrow/AFImageDownloader
        Downloads JPEG images asynchronously and decompresses them on a background thread.
    TRWebImage
        http://lijinchao.sinaapp.com
        just like SBWebImage except the cache
    WHCNetWorkKit
        https://github.com/netyouli/WHCNetWorkKit
        WHCNetWorkKit 是http网络请求开源库(支持GET/POST 文件上传 后台文件下载 UIButton UIImageView 控件设置网络图片 网络数据工具json/xml 转模型类对象 网络状态监听)
    AVOSCloudBeta
        https://github.com/avos/avoscloud-sdk
        AVOS Cloud iOS SDK for mobile backend.
    ZZWeChat
        https://open.weixin.qq.com/
        WeChatSDK for Cocoapods convenience.
    XIAOJIANGJIANG
        https://github.com/mrjlovetian/XIAOJIANGJIANG
        修改的版本信息
    HTY360Player
        https://github.com/islate/HTY360Player
        全景视频播放
    XGPush
        http://xg.qq.com
        Tencent XG Push Service iOS SDK
    PocketSocket
        https://github.com/zwopple/PocketSocket
        Objective-C websocket client/server library for building things that work in realtime on iOS and OS X.
    SocketRocket
        https://github.com/square/SocketRocket
        A conforming WebSocket (RFC 6455) client library.
    StreamingKit
        https://github.com/tumtumtum/StreamingKit/
        A fast and extensible audio streamer for iOS and OSX with support for gapless playback and custom (non-HTTP) sources.
    SnapchatKit
        https://github.com/ThePantsThief/SnapchatKit
        An Objective-C implementation of the unofficial Snapchat API.
    SZTextView
        https://github.com/glaszig/SZTextView
        A drop-in UITextView replacement which gives you a placeholder.
    UAProgressView
        https://github.com/UrbanApps/UAProgressView
        UAProgressView is a simple and lightweight, yet powerful animated circular progress view.
    LFLiveKit
        https://github.com/chenliming777
        LaiFeng ios Live. LFLiveKit.
    YYModel
        https://github.com/ibireme/YYModel
        High performance model framework for iOS/OSX.
    MarqueeLabel
        https://github.com/cbpowell/MarqueeLabel
        A drop-in replacement for UILabel, which automatically adds a scrolling marquee effect when needed.
    UXKit
        https://github.com/rpplusplus/UXKit
        A Pod Repo Wrapper the private framework UXKit
    JTAlertView
        https://github.com/kubatru/JTAlertView
        JTAlertView is the new **wonderful dialog/HUD/alert** kind of View.
    ZHTagButton
        https://github.com/15038777234/ZHTagButton
        自定义标签
    ImageFilesPicker
        https://github.com/mcmatan/ImageFilesPicker
        ImageFilesPicker
    UMengAnalyticsSDK
        http://dev.umeng.com
        UMengAnalyticsSDK for cocoapods convenience
    KAProgressLabel
        https://github.com/kirualex/KAProgressLabel
        Circular progress with styling, animations, interaction and more.
    OLImageView
        https://www.github.com/ondalabs/OLImageView
        Animated GIFs implemented the right way.
    GPUImage
        https://github.com/BradLarson/GPUImage
        An open source iOS framework for GPU-based image and video processing.
    ReactiveCocoa
        https://github.com/ReactiveCocoa/ReactiveCocoa
        A framework for composing and transforming streams of values.
    UIAutomation
        http://github.com/joemasilotti/UIAutomation
        iOS's private UIAutomation framework headers.
    XMNThirdFunction
        https://github.com/ws00801526
        封装第三方SDK 集成分享功能
    HHEAColourfulProgressView
        https://github.com/HH-Medic/HHEAColourfulProgressView
        a progressView in masonry.
    YYKit
        https://github.com/ibireme/YYKit
        A collection of iOS components.
    Two-waySSL-AES
        https://github.com/w6zhou/Two-waySSL-AES
        A useful lib for network security.
    HCSStarRatingView
        https://github.com/hsousa/HCSStarRatingView
        Simple star rating view for iOS written in Objective-C
    ZMWImageCache
        https://github.com/zjjzmw1/ZMWImageCache
        获取网络图片并缓存（默认带内存缓存，可以用不带的方法）
    OnlineKit
        https://github.com/legoless/OnlineKit.git
        All in one solution for web service models.
    SGActionView
        https://github.com/sagiwei/SGActionView
        Combination alert view, table, and share view.
Pods Dummy Files (24):
    PodsDummy_PocketSocket.h
    PodsDummy_SDWebImage.h
    PodsDummy_Aspects.h
    PodsDummy_GCDWebServer.h
    PodsDummy_UICKeyChainStore.h
    PodsDummy_YYModel.h

    --- Total 2462 files ---
Traceback (most recent call last):
  File "/usr/local/bin/supotato", line 9, in <module>
    load_entry_point('supotato==2.0.4', 'console_scripts', 'supotato')()
  File "/Library/Python/2.7/site-packages/supotato/supotato.py", line 296, in main
    classify(input, output, sortby=sortby, asc=is_asc, prefix_length=prefixlength)
  File "/Library/Python/2.7/site-packages/supotato/supotato.py", line 179, in classify
    f.write('        ' + info + '\n')
UnicodeEncodeError: 'ascii' codec can't encode character u'\u662f' in position 22: ordinal not in range(128)


<!-- 运行 指定版本的python-->
devzkndeMacBook-Pro:Python devzkn$ pyenv versions 
* system (set by /Users/devzkn/.pyenv/version)
  3.7.0b2

devzkndeMacBook-Pro:FaceBeautyV1PLUShead devzkn$ supotato -i  head -o .

http://einverne.github.io/post/2017/04/pyenv.html

devzkndeMacBook-Pro:FaceBeautyV1PLUShead devzkn$  echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
devzkndeMacBook-Pro:FaceBeautyV1PLUShead devzkn$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile

bash-3.2$ brew link python3
Linking /usr/local/Cellar/python/3.6.5... 21 symlinks created

<!-- Python2.7和Python3.6安装后，pip2和pip3下载的包仍在Mac OSX系统自带的Python2.7的包目录下，而非Python2.7和Python3.6的安装目录： -->



bash-3.2$ brew info python3
python: stable 3.6.5 (bottled), devel 3.7.0b3, HEAD
Interpreted, interactive, object-oriented programming language
https://www.python.org/
/usr/local/Cellar/python/3.6.3 (7,572 files, 105.4MB)
  Built from source on 2017-12-10 at 10:12:59
/usr/local/Cellar/python/3.6.5 (8,650 files, 148.2MB) *
  Built from source on 2018-04-02 at 13:47:43
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/python.rb
==> Dependencies
Build: pkg-config ✔, sphinx-doc ✔
Required: gdbm ✔, openssl ✔, readline ✔, sqlite ✘, xz ✔
Optional: tcl-tk ✘
==> Options
--with-tcl-tk
	Use Homebrew's Tk instead of macOS Tk (has optional Cocoa and threads support)
--devel
	Install development version 3.7.0b3
--HEAD
	Install HEAD version
==> Caveats
Python has been installed as
  /usr/local/bin/python3

Unversioned symlinks `python`, `python-config`, `pip` etc. pointing to
`python3`, `python3-config`, `pip3` etc., respectively, have been installed into
  /usr/local/opt/python/libexec/bin

If you need Homebrew's Python 2.7 run
  brew install python@2

Pip, setuptools, and wheel have been installed. To update them run
  pip3 install --upgrade pip setuptools wheel

You can install Python packages with
  pip3 install <package>
They will install into the site-package directory
  /usr/local/lib/python3.6/site-packages

See: https://docs.brew.sh/Homebrew-and-Python
bash-3.2$ ls -lrt /usr/local/opt/python/libexec/bin
total 0
lrwxr-xr-x  1 devzkn  admin  58 Apr  2 13:47 idle -> ../../Frameworks/Python.framework/Versions/3.6/bin/idle3.6
lrwxr-xr-x  1 devzkn  admin  59 Apr  2 13:47 pydoc -> ../../Frameworks/Python.framework/Versions/3.6/bin/pydoc3.6
lrwxr-xr-x  1 devzkn  admin  60 Apr  2 13:47 python -> ../../Frameworks/Python.framework/Versions/3.6/bin/python3.6
lrwxr-xr-x  1 devzkn  admin  68 Apr  2 13:47 python-config -> ../../Frameworks/Python.framework/Versions/3.6/bin/python3.6m-config


<!-- export PATH="/usr/local/opt/python/libexec/bin:$PYENV_ROOT/bin:$PATH" -->


I guess it didn't seem necessary since ls -al $(which python3) confirms /usr/local/bin/python3 -> ../Cellar/python/3.6.4_4/bin/python3 etc. But if one wants Homebrew's 3.x python over system Python then suggesting export PATH="/usr/local/opt/python/libexec/bin:$PATH" makes sense.


```

- [BUILD FAILED (OS X 10.13.1 using python-build 20160602)](https://github.com/pyenv/pyenv/issues/655)

```
<!-- https://github.com/pyenv/pyenv/issues/988 -->
CFLAGS="-I$(brew --prefix openssl)/include" \
LDFLAGS="-L$(brew --prefix openssl)/lib" \
pyenv install 3.6.2


<!-- https://github.com/pyenv/pyenv/issues/655 -->

BUILD FAILED (OS X 10.13.1 using python-build 20160602)

^CdevzkndeMacBook-Pro:Python devzkn$ xcode-select --install
xcode-select: note: install requested for command line developer tools

since I had indeed just upgraded to Mac OS Sierra


Found CFLAGS="-I$(xcrun --show-sdk-path)/usr/include" pyenv install -v $VERSION in the wiki.

devzkndeMacBook-Pro:Python devzkn$ pyenv install 3.7.0b2



```
- [dumpdecrypted in LLDB](https://blog.0xbbc.com/2017/08/dumpdecrypted-in-lldb/#more-2725)

```
dump the decrypted binary in LLDB

<!-- Usage -->

https://github.com/BlueCocoa/dumpdecrypted-lldb/blob/master/dumpdecrypted.py


command script import /PATH/TO/THE/dumpdecrypted.py
dumpdecrypted -i Ingress -o /Users/BlueCocoa/Ingress_dumpdecrypted

<!-- https://github.com/hooktweaks/dumpdecrypted-lldb -->

我还是就觉得 https://github.com/zhangkn/KNBin/blob/master/kndump 好用。
```

- [仅供学习：tweak 项目 快速搭建CocoaAsyncSocket（建连、断开、重连、心跳、通用请求）](https://github.com/zhangkn/KNCocoaAsyncSocketDemo)

- [https://github.com/pyenv/pyenv](https://github.com/pyenv/pyenv)

```
用 pyenv 来管理python版本

Simple Python version management


<!-- $ brew install pyenv -->

devzkndeMacBook-Pro:FaceBeautyV1PLUShead devzkn$  echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
devzkndeMacBook-Pro:FaceBeautyV1PLUShead devzkn$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile



alias spro='source ~/.bash_profile'


<!-- bash_profile -->
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
export PYENV_VIRTUALENV_DISABLE_PROMPT=1

==> Installing dependencies for pyenv: autoconf
==> Installing pyenv dependency: autoconf
==> Downloading https://homebrew.bintray.com/bottles/autoconf-2.69.high_sierra.bottle.4.tar.gz
######################################################################## 100.0%
==> Pouring autoconf-2.69.high_sierra.bottle.4.tar.gz
==> Caveats
Emacs Lisp files have been installed to:
  /usr/local/share/emacs/site-lisp/autoconf
==> Summary
🍺  /usr/local/Cellar/autoconf/2.69: 71 files, 3.0MB
==> Installing pyenv
==> Downloading https://homebrew.bintray.com/bottles/pyenv-1.2.3.high_sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring pyenv-1.2.3.high_sierra.bottle.tar.gz
🍺  /usr/local/Cellar/pyenv/1.2.3: 597 files, 2.4MB


<!-- pyenv install <version>安装其他版本的Python。 -->

devzkndeMacBook-Pro:Python devzkn$ pyenv install 3.6.5
python-build: use openssl from homebrew
python-build: use readline from homebrew
Downloading Python-3.6.5.tar.xz...
-> https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tar.xz

https://www.python.org/ftp/python/ 查看最新版本的python 3.7.0/  

python-build: use readline from homebrew

BUILD FAILED (OS X 10.13.1 using python-build 20160602)

Inspect or clean up the working tree at /var/folders/8s/t119mw8d4lsdztx8h9q8113m0000gn/T/python-build.20180404150336.21909
Results logged to /var/folders/8s/t119mw8d4lsdztx8h9q8113m0000gn/T/python-build.20180404150336.21909.log

Last 10 log lines:
  File "/private/var/folders/8s/t119mw8d4lsdztx8h9q8113m0000gn/T/python-build.20180404150336.21909/Python-3.6.5/Lib/ensurepip/__main__.py", line 5, in <module>
    sys.exit(ensurepip._main())
  File "/private/var/folders/8s/t119mw8d4lsdztx8h9q8113m0000gn/T/python-build.20180404150336.21909/Python-3.6.5/Lib/ensurepip/__init__.py", line 204, in _main
    default_pip=args.default_pip,
  File "/private/var/folders/8s/t119mw8d4lsdztx8h9q8113m0000gn/T/python-build.20180404150336.21909/Python-3.6.5/Lib/ensurepip/__init__.py", line 117, in _bootstrap
    return _run_pip(args + [p[0] for p in _PROJECTS], additional_paths)
  File "/private/var/folders/8s/t119mw8d4lsdztx8h9q8113m0000gn/T/python-build.20180404150336.21909/Python-3.6.5/Lib/ensurepip/__init__.py", line 27, in _run_pip
    import pip
zipimport.ZipImportError: can't decompress data; zlib not available
make: *** [install] Error 1



devzkndeMacBook-Pro:Python devzkn$ pyenv install 3.7.0


See all available versions with `pyenv install --list'.

If the version you need is missing, try upgrading pyenv:

  brew update && brew upgrade pyenv


devzkndeMacBook-Pro:Python devzkn$ pyenv install --list
Available versions:
  2.1.3
  2.2.3
  3.6.5
3.7.0b2
  3.7-dev
  3.8-dev
devzkndeMacBook-Pro:Python devzkn$ pyenv install 3.7.0b2
python-build: use openssl from homebrew
python-build: use readline from homebrew
Downloading Python-3.7.0b2.tar.xz...
-> https://www.python.org/ftp/python/3.7.0/Python-3.7.0b2.tar.xz
Installing Python-3.7.0b2...
python-build: use readline from homebrew


<!-- pyenv local <version>切换python版本。 -->

$ cd python35
$ pyenv local 3.5.1    #使当前工作目录使用python3.5.1版本
$ python -V            #查看一下当前目录用python的版本，确实是3.5.1
Python3.5.1
$ pip -V               #查看一下pip版本,是3.5的pip
pip 7.1.2 from /usr/local/var/pyenv/versions/3.5.1/lib/python3.5/site-packages (python 3.5)
$ cd                   #回到家目录

(如果是用系统自带版本，用pyenv local system即可使当前工作目录使用系统自带的Python2.7.10，不过一般很少用系统自带的Python)

<!-- $ pyenv versions -->

查看系统的上安装的Python版本

devzkndeMacBook-Pro:Python devzkn$ brew list python
/usr/local/Cellar/python/3.6.5/bin/2to3
/usr/local/Cellar/python/3.6.5/bin/2to3-3.6
/usr/local/Cellar/python/3.6.5/bin/idle3



^CdevzkndeMacBook-Pro:Python devzkn$ xcode-select --install
xcode-select: note: install requested for command line developer tools


devzkndeMacBook-Pro:Python devzkn$ pyenv install 3.7.0b2
python-build: use openssl from homebrew
python-build: use readline from homebrew
Downloading Python-3.7.0b2.tar.xz...
-> https://www.python.org/ftp/python/3.7.0/Python-3.7.0b2.tar.xz
Installing Python-3.7.0b2...
python-build: use readline from homebrew
Installed Python-3.7.0b2 to /Users/devzkn/.pyenv/versions/3.7.0b2


$ pyenv rehash

devzkndeMacBook-Pro:Python devzkn$ pyenv versions 
* system (set by /Users/devzkn/.pyenv/version)
  3.7.0b2


<!-- $ pyenv uninstall 2.7.3 # 卸载python -->

<!-- python切换 -->

$ pyenv global 2.7.3  # 设置全局的 Python 版本，通过将版本号写入 ~/.pyenv/version 文件的方式。

$ pyenv local 2.7.3 # 设置 Python 本地版本，通过将版本号写入当前目录下的 .python-version 文件的方式。通过这种方式设置的 Python 版本优先级较 global 高。

<!-- python优先级 -->

shell > local > global

pyenv 会从当前目录开始向上逐级查找 .python-version 文件，直到根目录为止。若找不到，就用 global 版本。


$ pyenv shell 2.7.3 # 设置面向 shell 的 Python 版本，通过设置当前 shell 的 PYENV_VERSION 环境变量的方式。这个版本的优先级比 local 和 global 都要高。–unset 参数可以用于取消当前 shell 设定的版本。
$ pyenv shell --unset

$ pyenv rehash  # 创建垫片路径（为所有已安装的可执行文件创建 shims，如：~/.pyenv/versions/*/bin/*，因此，每当你增删了 Python 版本或带有可执行文件的包（如 pip）以后，都应该执行一次本命令）





<!-- 运行 -->

```
- [iOS10 越狱开发环境搭建](http://everettjf.com/2017/05/08/ios10-jailbreak-dev-env-setup-tutorial/)

- [AppleTrace 搭配 MonkeyDev Trace任意App](http://everettjf.com/2017/10/12/appletrace-dancewith-monkeydev/)

- [让你的微信不再被人撤回消息](http://yulingtianxia.com/blog/2016/05/06/Let-your-WeChat-for-Mac-never-revoke-messages/)


- [_shortMethodDescription 用于LLDB 进行打断点](https://segmentfault.com/a/1190000012196362)

```

找到需要下断点的类，如CMessageMgr，然后在LLDB命令行输入po [className
_shortMethodDescription]。  -> 找到对应放的内存地址，避免了之前的地址计算。

<!-- 试试这个不用借用hoper 基地址计算的方法 -->
(lldb) br del
About to delete all breakpoints, do you want to do that?: [Y/n] y
All breakpoints removed. (1 breakpoint)





<!--cycript  和lldb   不能同时进行  -->
Taokeceshiji1:~ root# cycript -p FaceBeautyV1PLUS
cy# 

只有当Process 1141 resuming 的时候cycript 才能注入成功。



```

- [Hopper + LLDB](https://blog.csdn.net/z929118967/article/details/78264816)

```
<!-- LLDB是Low Level Debugger -->

LLDB是Xcode内置的动态调试工具,如果你不做其他的额外处理，因为debugserver缺少task_for_pid权限，所以你只能使用LLDB来调试你自己的App。

- [ 配置debugserver](https://blog.csdn.net/z929118967/article/details/78262460)




```

- [usbmuxd ](http://blog.csdn.net/z929118967/article/details/78201784)

- [Anti ptrace:去掉AlipayWallet的ptrace 反调试保护，进行lldb调试---仅用于参考学习](https://segmentfault.com/a/1190000012199291)

- [ iOS逆向工程之Hopper+LLDB调试第三方App](https://blog.csdn.net/z929118967/article/details/78263055)

- [一步一步用debugserver + lldb代替gdb进行动态调试](http://iosre.com/t/debugserver-lldb-gdb/65)

- [ 配置debugserver](https://blog.csdn.net/z929118967/article/details/78262460)

- [how-to-locate-the-block](http://iosre.com/t/when-you-come-across---nsxxxblock---0xrandomnumber-while-debugging-a-block-in-lldb-how-to-locate-the-block/4977)

```

根据Block的结构体，函数指针是void (*invoke)(void *, ...); 所以楼主选了17个字节（也即第3个16Byte）的那个：
struct Block_literal_1 {
void *isa; // initialized to &_NSConcreteStackBlock or &_NSConcreteGlobalBlock
int flags;
int reserved;
void (*invoke)(void *, ...);
struct Block_descriptor_1 {
unsigned long int reserved;         // NULL
    unsigned long int size;         // sizeof(struct Block_literal_1)
    // optional helper functions
    void (*copy_helper)(void *dst, void *src);     // IFF (1<<25)
    void (*dispose_helper)(void *src);             // IFF (1<<25)
    // required ABI.2010.3.16
    const char *signature;                         // IFF (1<<30)
} *descriptor;
// imported variables
};
```


- [powerful-private-methods](http://iosre.com/t/powerful-private-methods-for-debugging-in-cycript-lldb/3414)

- [多年前 写了个 XXtouch 自动注册 ICloud 账号的lua 留念一下](https://www.linpblog.com/2018/03/18/%E5%A4%9A%E5%B9%B4%E5%89%8D-%E5%86%99%E4%BA%86%E4%B8%AA-XXtouch-%E8%87%AA%E5%8A%A8%E6%B3%A8%E5%86%8C-Icloud-%E8%B4%A6%E5%8F%B7%E7%9A%84lua-%E7%95%99%E5%BF%B5%E4%B8%80%E4%B8%8B/)

```
https://www.zybuluo.com/chendbdb/note/545142

XXTouch iOS 开发手册: https://www.zybuluo.com/xxtouch/note/370734



```

