---
layout: post
title: symbolicatecrash
date: 2018-03-12
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

>* 每次编译后生成的 dSYM 文件

```
dSYM 文件里存储了函数地址映射，这样调用栈里的地址可以通过 dSYM 这个映射表能够获得具体函数的位置。一般都会用来处理 crash 时获取到的调用栈 .crash 文件将其符号化

<!--  1、 通过 Xcode 进行符号化-->

将 .crash 文件，.dSYM 和 .app 文件放到同一个目录下，打开 Xcode 的 Window 菜单下的 organizer，再点击 Device tab，最后选中左边的 Device Logs。选择 import 将 .crash 文件导入就可以看到 crash 的详细 log 了。


<!-- 2、通过命令行工具 symbolicatecrash 来手动符号化 crash log -->
本文的重点
export DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer
symbolicatecrash appName.crash appName.app > appName.log


```

>* 查找symbolicatecrash 工具的目录

```
在Xcode目录下查找/Applications/Xcode.app/Contents
find . -name  'symbolicatecrash' -ls
/Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash

<!-- 便于以后的使用，最好采用ln ，这样避免拷贝，尤其是后台的工程师尤其注意这点-->
devzkndeMacBook-Pro:LatestBuild devzkn$ ln -s /Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash ~/bin

devzkndeMacBook-Pro:LatestBuild devzkn$ ls -lrt ~/bin
lrwxr-xr-x  1 devzkn  staff     111 Mar 12 17:28 symbolicatecrash -> /Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash
```

### 正文


>* symbolicatecrash
```
Error: "DEVELOPER_DIR" is not defined at /Users/devzkn/bin/symbolicatecrash line 69.
```
>* devzkndeMacBook-Pro:Contents devzkn$ export DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer
```
<!-- 想要永久生效，就配置到配置文件 -->
devzkndeMacBook-Pro:~ devzkn$ open -e  .bash_profile
export DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer
source ~/.bash_profile
```

>* 符号化
```
devzkndeMacBook-Pro:LatestBuild devzkn$ /Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash --dsym=/Users/devzkn/work/.git//LatestBuild/.dylib.dSYM /Users/devzkn/Desktop/SpringBoard-2018-03-12-162524.ips  --output=/Users/devzkn/Desktop/kn.crash
```

>* 效果

```
Exception Type:  EXC_BAD_ACCESS (SIGSEGV)
Exception Subtype: KERN_INVALID_ADDRESS at 0x0000000000000010
Triggered by Thread:  16

Thread 16 name:  com..
Thread 16 Crashed:
0   .dylib              	0x0000000109b639b8 0x109b54000 + 63928
1   .dylib              	0x0000000109b639a0 0x109b54000 + 63904
2   .dylib              	0x0000000109b63330 0x109b54000 + 62256
3   .dylib              	0x0000000109b631f0 0x109b54000 + 61936
4   .dylib              	0x0000000109b8084c 0x109b54000 + 182348
5   .dylib              	0x0000000109b812b4 0x109b54000 + 185012
6                       	0x0000000188225048 0x18811b000 + 1089608

<!-- 符号化之后的结果 -->
Thread 16 Crashed:
0   .dylib              	0x0000000109b639b8 +[ASWindowKNTool setupSwitchIpABUYUN1:] (ASWindowKNTool.m:289)
1   .dylib              	0x0000000109b639a0 +[ASWindowKNTool setupSwitchIpABUYUN1:] (ASWindowKNTool.m:287)
2   .dylib              	0x0000000109b63330 +[ASWindowKNTool setupSwitchIp:] (ASWindowKNTool.m:88)
3   .dylib              	0x0000000109b631f0 +[ASWindowKNTool setupCloseOpenWifi] (ASWindowKNTool.m:62)
4   .dylib              	0x0000000109b8084c -[ ] (.m:501)
5   .dylib              	0x0000000109b812b4 -[ ] (.m:644)
6                       	0x0000000188225048 __NSThreadPerformPerform + 340


```

### CrashReporter


>* find . -name  'SpringBoard*' -ls
```
12598152   92 -rw-rw-rw-   1 mobile   mobile      94165 Mar 12 16:25 ./private/var/mobile/Library/Logs/CrashReporter/SpringBoard-2018-03-12-162524.ips
```

>* /private/var/mobile/Library/Logs/CrashReporter
```
-rw-rw-rw- 1 mobile mobile 373035 Mar 12 16:09 panic-2018-03-12-160904.ips
-rw-rw-rw- 1 mobile mobile  93633 Mar 12 16:17 SpringBoard-2018-03-12-161753.ips
-rw-rw-rw- 1 mobile mobile  94099 Mar 12 16:18 SpringBoard-2018-03-12-161853.ips
-rw-rw-rw- 1 mobile mobile  93633 Mar 12 16:20 SpringBoard-2018-03-12-162025.ips
-rw-rw-rw- 1 mobile mobile  94015 Mar 12 16:21 SpringBoard-2018-03-12-162151.ips
-rw-rw-rw- 1 mobile mobile  94165 Mar 12 16:25 SpringBoard-2018-03-12-162524.ips
```

>* 简单的例子

```

devzkndeMacBook-Pro:com.wl..git devzkn$ scp usb2222:/private/var/mobile/Library/Logs/CrashReporter/SpringBoard-2018-03-23-153316.ips ~


devzkndeMacBook-Pro:com.wl..git devzkn$ symbolicatecrash --dsym=/Users/devzkn/com..dylib.dSYM /Users/devzkn/SpringBoard-2018-03-23-153316.ips | open -f

```

### Mach-O 文件


记录编译后的可执行文件，对象代码，共享库，动态加载代码和内存转储的文件格式。

>* _CodeSignature

```
里面包含了程序代码的签名，这个签名的作用就是保证签名后 .app 里的文件，包括资源文件，Mach-O 文件都不能够更改。
```

>* [Mach-O 文件包含三个区域](https://zhangkn.github.io/2018/03/Hopper/)

```
1、Mach-O Header：包含字节顺序，magic，cpu 类型，加载指令的数量等

2、Load Commands：包含很多内容的表，包括区域的位置，符号表，动态符号表等。每个加载指令包含一个元信息，比如指令类型，名称，在二进制中的位置等。
<!-- The existing load commands are listed below: -->

LC_SEGMENT — contains different information on a certain segment: size, number of sections, offset in the file and in memory (after the load)

LC_SYMTAB — loads the table of symbols and strings

LC_DYSYMTAB — creates an import table; data on symbols is taken from the symbol table

LC_LOAD_DYLIB — defines the dependency from a certain third-party library

<!-- The most important segments are the following: -->

__TEXT — the executed code and other read-only data

__DATA — data available for writing; including import tables that can be changed by the dynamic loader during lazy binding

__OBJC — different information of the standard library of Objective-C language of execution time

__IMPORT — import table only for 32-bit architecture (I managed to generate it only on Mac OS 10.5)

__LINKEDIT — here, the dynamic loader places its data for already loaded modules (symbol tables, string tables, etc.)


3、Data：最大的部分，包含了代码，数据，比如符号表，动态符号表等。

```

>* load command

```

<!-- Any load command starts with the following fields: -->

struct load_command
{
  uint32_t cmd;  //command numeric code
  uint32_t cmdsize;  //size of the current command in bytes
};

```
>* sections

```
__TEXT,__text — the code itself

__TEXT,__cstring — constant strings (in double quotes)

__TEXT,__const — different constants

__DATA,__data — initialized variables (strings and arrays)

__DATA,__la_symbol_ptr — table of pointers to imported functions

__DATA,__bss — non-initialized static variables

__IMPORT,__jump_table — stubs for calls of imported functions

<!-- struct -->

struct section
{
  char sectname[16];
  char segname[16];
  uint32_t addr;
  uint32_t size;
  uint32_t offset;
  uint32_t align;
  uint32_t reloff;
  uint32_t nreloc;
  uint32_t flags;
  uint32_t reserved1;
  uint32_t reserved2;
};

```

>* mach_header

```

struct mach_header
{
  uint32_t magic;
  cpu_type_t cputype;
  cpu_subtype_t cpusubtype;
  uint32_t filetype;
  uint32_t ncmds;
  uint32_t sizeofcmds;
  uint32_t flags;
};
```

>* Fat Binary

```
struct fat_header
{
  uint32_t magic;
  uint32_t nfat_arch;
};


struct fat_arch
{
  cpu_type_t cputype;
  cpu_subtype_t cpusubtype;
  uint32_t offset;
  uint32_t size;
  uint32_t align;
};

```

>* Mach-O 文件的解析

```
<!-- touch test.c -->
devzkndeMacBook-Pro:~ devzkn$ echo "#include <stdio.h>
> int main(int argc, char *argv[])
> {
>     printf("hi there!\n");
-bash: !\n": event not found
>     return 0;
> }" >  test.c

<!-- a.out   编译运行，没有起名默认为 a.out-->
devzkndeMacBook-Pro:~ devzkn$ xcrun clang test.c
a.out 就是编译生成的二进制文件

<!--查看生成的汇编代码 devzkndeMacBook-Pro:~ devzkn$ xcrun clang -S -o - test.c | open -f -->
	.section	__TEXT,__text,regular,pure_instructions ##指定接下来执行哪一个段
	.macosx_version_min 10, 12
	.globl	_main ##_main 是一个外部符号
	.p2align	4, 0x90  ##指出后面代码的对齐方式，16(2^4) 字节对齐， 0x90 补齐
_main:                                  ## @main   main 函数头部部分，是函数开始的地址，二进制文件会有这个位置的引用
	.cfi_startproc    ##  用于函数的开始； CFI 是 Call Frame Infomation 的缩写是调用帧信息的意思，在用 debugger 时实际上就是 stepping in / out 的一个调用帧
## BB#0:
	pushq	%rbp  ## 将 rbp 的值 push 到栈中。
Ltmp0:
	.cfi_def_cfa_offset 16
Ltmp1:
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp   ##把局部变量放到栈上
Ltmp2:
	.cfi_def_cfa_register %rbp
	xorl	%eax, %eax     ## 将 ecx 寄存器设置为0
	movl	$0, -4(%rbp)
	movl	%edi, -8(%rbp)
	movq	%rsi, -16(%rbp)
	popq	%rbp
	retq
	.cfi_endproc   ##.cfi_endproc 和 .cfi_startproc 配对标记结束。


.subsections_via_symbols  ## 静态链接器用的

<!-- . 开头的行是汇编指令不是汇编代码，其它的都是汇编代码 -->

<!-- 接下来通过 size 工具来看看 a.out 里的 section。 -->
<!-- devzkndeMacBook-Pro:~ devzkn$ xcrun size -x -l -m tmp -->
xcrun size -x -l -m a.out

devzkndeMacBook-Pro:~ devzkn$  xcrun size -x -l -m a.out
Segment __PAGEZERO: 0x100000000 (vmaddr 0x0 fileoff 0)
Segment __TEXT: 0x1000 (vmaddr 0x100000000 fileoff 0)
	Section __text: 0x16 (addr 0x100000fa0 offset 4000)
	Section __unwind_info: 0x48 (addr 0x100000fb8 offset 4024)
	total 0x5e
Segment __LINKEDIT: 0x1000 (vmaddr 0x100001000 fileoff 4096)
total 0x100002000
<!-- 在运行时，虚拟内存会把 segment 映射到进程的地址空间 -->
1）__PAGEZERO segment 的大小是 4GB，不是文件真实大小，是规定进程地址空间前 4GB 被映射为不可执行，不可写和不可读。

2）__TEXT segment 包含被执行的代码以只读和可执行的方式映射。

__text section 包含编译后的机器码。

stubs 和 stub_helper 是给动态链接器 dyld 使用，可以允许延迟链接。

__cstring 可执行文件中的字符串。

__const 不可变的常量。

3）__DATA segment 以可读写和不可执行的方式映射，里面是会被更改的数据。

__nl_symbol_ptr 非延迟指针。可执行文件加载同时加载。

__la_symbol_ptr 延迟符号指针。延迟用于可执行文件中调用未定义的函数，可执行文件里没有包含的函数会延迟加载。

__const 需要重定向的常量，例如 char * const c = “foo”; c指针指向可变的数据。

__bss 不用初始化的静态变量，例如 static int i; ANSI C 标准规定静态变量必须设置为0。运行时静态变量的值是可修改的。

__common 包含外部全局变量。例如在函数外定义 int i;

__dyld 是section占位符，用于动态链接器。


<!-- 接下来用 otool 查看下 section 里的内容： -->

devzkndeMacBook-Pro:~ devzkn$  xcrun otool -v -s __TEXT __text a.out
a.out:
(__TEXT,__text) section
_main:
0000000100000fa0	pushq	%rbp
0000000100000fa1	movq	%rsp, %rbp
0000000100000fa4	xorl	%eax, %eax
0000000100000fa6	movl	$0x0, -0x4(%rbp)
0000000100000fad	movl	%edi, -0x8(%rbp)
0000000100000fb0	movq	%rsi, -0x10(%rbp)
0000000100000fb4	popq	%rbp
0000000100000fb5	retq

<!-- -s TEXT text 有个缩写 -t -->
devzkndeMacBook-Pro:~ devzkn$  xcrun otool -v -t a.out


<!-- 通过 otool 来看看可执行文件头部， 通过 -h 可以打印出头部信息： -->
devzkndeMacBook-Pro:~ devzkn$ otool -v -h a.out


<!-- 通过 -l 可以查看加载命令 -->
devzkndeMacBook-Pro:~ devzkn$ otool -v -l a.out | open -f

<!-- 加载命令结构体 -->

struct segment_command {
  uint32_t  cmd;
  uint32_t  cmdsize;
  char      segname[16];
  uint32_t  vmaddr;
  uint32_t  vmsize;
  uint32_t  fileoff;
  uint32_t  filesize;
  vm_prot_t maxprot;
  vm_prot_t initprot;
  uint32_t  nsects;
  uint32_t  flags;
};

查看 Load command 1 这个部分可以找到 initprot r-x ，表示只读和可执行。

```

>* 多个文件是怎么合成一个可执行文件?


```
devzkndeMacBook-Pro:~ devzkn$ echo "#import <Foundation/Foundation.h>
> @interface Foo : NSObject
> - (void)say;
> @end" > Foo.h

devzkndeMacBook-Pro:~ devzkn$ echo "#import “Foo.h”
> @implementation Foo
> - (void)say
> {
>     NSLog(@“hi there again!\n”);
> }
> @end" > Foo.m


devzkndeMacBook-Pro:~ devzkn$ echo "#import “Foo.h”
> int main(int argc, char *argv[])
> {
>     @autoreleasepool {
>         Foo *foo = [[Foo alloc] init];
>         [foo say];
>         return 0;
>     }
> }" > SayHi.m

<!-- 先编译多个文件 Foo.o、SayHi.o  -c 标识符指明需要运行预处理器，语法分析，类型检查，LLVM生成优化以及汇编代码生成.o文件
-->

devzkndeMacBook-Pro:~ devzkn$ xcrun clang -c Foo.m
devzkndeMacBook-Pro:~ devzkn$ xcrun clang -c SayHi.m

<!-- 再将编译后的文件链接起来，这样就可以生成 a.out 可执行文件了。 -W 以-W开头的，可以通过这些定制编译警告 -->

xcrun clang SayHi.o Foo.o -Wl,'xcrun —show-sdk-path'/System/Library/Frameworks/Foundation.framework/Foundation

devzkndeMacBook-Pro:~ devzkn$   xcrun clang SayHi.o Foo.o -Wl, /System/Library/Frameworks/Foundation.framework/Foundation



<!-- devzkndeMacBook-Pro:~ devzkn$ xcrun size -x -l -m a.out -->

Segment __PAGEZERO: 0x100000000 (vmaddr 0x0 fileoff 0)
Segment __TEXT: 0x1000 (vmaddr 0x100000000 fileoff 0)
	Section __text: 0xa7 (addr 0x100000e90 offset 3728)
	Section __stubs: 0x18 (addr 0x100000f38 offset 3896)
	Section __stub_helper: 0x38 (addr 0x100000f50 offset 3920)
	Section __objc_methname: 0xf (addr 0x100000f88 offset 3976)
	Section __cstring: 0x11 (addr 0x100000f97 offset 3991)
	Section __objc_classname: 0x4 (addr 0x100000fa8 offset 4008)
	Section __objc_methtype: 0x8 (addr 0x100000fac offset 4012)
	Section __unwind_info: 0x48 (addr 0x100000fb4 offset 4020)
	total 0x16b
Segment __DATA: 0x1000 (vmaddr 0x100001000 fileoff 4096)
	Section __nl_symbol_ptr: 0x10 (addr 0x100001000 offset 4096)
	Section __la_symbol_ptr: 0x20 (addr 0x100001010 offset 4112)
	Section __cfstring: 0x20 (addr 0x100001030 offset 4144)
	Section __objc_classlist: 0x8 (addr 0x100001050 offset 4176)
	Section __objc_imageinfo: 0x8 (addr 0x100001058 offset 4184)
	Section __objc_const: 0xb0 (addr 0x100001060 offset 4192)
	Section __objc_selrefs: 0x18 (addr 0x100001110 offset 4368)
	Section __objc_classrefs: 0x8 (addr 0x100001128 offset 4392)
	Section __objc_data: 0x50 (addr 0x100001130 offset 4400)
	total 0x180
Segment __LINKEDIT: 0x1000 (vmaddr 0x100002000 fileoff 8192)
total 0x100003000

__stubs 和 __stub_helper 是给动态链接器 (dyld) 使用的。通过这两个 section，在动态链接代码中，可以允许延迟链接。



devzkndeMacBook-Pro:~ devzkn$ xcrun otool -v -t a.out
a.out:
(__TEXT,__text) section
_main:
-[Foo say]:

<!-- devzkndeMacBook-Pro:~ devzkn$ otool -l a.out -->

<!-- devzkndeMacBook-Pro:~ devzkn$ otool -v -s __TEXT __cstring a.out -->
a.out:
Contents of (__TEXT,__cstring) section
0000000100000f97  hi there again!\n



```


### Clang Attributes

以 attribute(xx) 的语法格式出现，是 Clang 提供的一些能够让开发者在编译过程中参与一些源码控制的方法。

>* attribute((format(NSString, F, A))) 格式化字符串

```
以NSLog 为例子
<!-- FOUNDATION_EXPORT void NSLog(NSString *format, ...) NS_FORMAT_FUNCTION(1,2) NS_NO_TAIL_CALL; -->

<!-- NS_FORMAT_FUNCTION(1,2)   格式化字符串-->
// Marks APIs which format strings by taking a format string and optional varargs as arguments
#if !defined(NS_FORMAT_FUNCTION)
    #if (__GNUC__*10+__GNUC_MINOR__ >= 42) && (TARGET_OS_MAC || TARGET_OS_EMBEDDED)
	#define NS_FORMAT_FUNCTION(F,A) __attribute__((format(__NSString__, F, A)))
    #else
	#define NS_FORMAT_FUNCTION(F,A)
    #endif
#endif

<!-- NS_NO_TAIL_CALL -->
#if __has_attribute(not_tail_called)
#define NS_NO_TAIL_CALL __attribute__((not_tail_called))
#else
#define NS_NO_TAIL_CALL
#endif

```

>* attribute((deprecated(s))) 版本弃用提示

```

- (void)preMethod:( NSString *)string __attribute__((deprecated(“preMethod已经被弃用，请使用newMethod”)));
- (void)deprecatedMethod DEPRECATED_ATTRIBUTE; //也可以直接使用DEPRECATED_ATTRIBUTE这个系统定义的宏

```

>* attribute((availability(os,introduced=m,deprecated=n, obsoleted=o,message=“” VA_ARGS))) 指明使用版本范围

```
<!-- #define CF_DEPRECATED(_macIntro, _macDep, _iosIntro, _iosDep, ...) __attribute__((availability(ios,introduced=_iosIntro,deprecated=_iosDep,message="" __VA_ARGS__))) -->

- (BOOL)connection:(NSURLConnection *)connection canAuthenticateAgainstProtectionSpace:(NSURLProtectionSpace *)protectionSpace NS_DEPRECATED(10_6, 10_10, 3_0, 8_0, "Use -connection:willSendRequestForAuthenticationChallenge: instead.");

os 指系统的版本，m 指明引入的版本，n 指明过时的版本，o 指完全不用的版本，message 可以写入些描述信息。
```

>* attribute((unavailable(…))) 方法不可用提示

```

// Marks methods and functions which cannot be used when compiling in automatic reference counting mode.
#if __has_feature(objc_arc)
#define NS_AUTOMATED_REFCOUNT_UNAVAILABLE __attribute__((unavailable("not available in automatic reference counting mode")))
#else
#define NS_AUTOMATED_REFCOUNT_UNAVAILABLE
#endif

```

>* attribute((unused)) 没有被使用也不报警告

>* attribute((warn_unused_result))

```
不使用方法的返回值就会警告，目前 swift3 已经支持该特性了。oc中也可以通过定义这个attribute来支持。
```

>* #define CF_AVAILABLE_IOS(_ios) __attribute__((availability(ios,introduced=_ios)))

>* attribute((availability(swift, unavailable, message=_msg)))

```
OC 的方法不能在 Swift 中使用。
// Swift availability macro
#if __has_feature(attribute_availability_swift)
#define CF_SWIFT_UNAVAILABLE(_msg) __attribute__((availability(swift, unavailable, message=_msg)))
#else
#define CF_SWIFT_UNAVAILABLE(_msg)
#endif

```


>* attribute((cleanup(…))) 作用域结束时自动执行一个指定方法

```
作用域结束包括大括号结束，return，goto，break，exception 等情况。这个动作是先于这个对象的 dealloc 调用的。

<!-- Reactive Cocoa 中有个比较好的使用范例，@onExit 这个宏，定义如下： -->


#define onExit \
    rac_keywordify \
    __strong rac_cleanupBlock_t metamacro_concat(rac_exitBlock_, __LINE__) __attribute__((cleanup(rac_executeCleanupBlock), unused)) = ^
static inline void rac_executeCleanupBlock (__strong rac_cleanupBlock_t *block) {
    (*block)();
}

<!-- 这样可以在就可以很方便的把需要成对出现的代码写在一起了。同样可以在 Reactive Cocoa 看到其使用 -->

if (property != NULL) {
		rac_propertyAttributes *attributes = rac_copyPropertyAttributes(property);
		if (attributes != NULL) {
			@onExit {
				free(attributes);
			};
			BOOL isObject = attributes->objectClass != nil || strstr(attributes->type, @encode(id)) == attributes->type;
			BOOL isProtocol = attributes->objectClass == NSClassFromString(@“Protocol”);
			BOOL isBlock = strcmp(attributes->type, @encode(void(^)())) == 0;
			BOOL isWeak = attributes->weak;
			shouldAddDeallocObserver = isObject && isWeak && !isBlock && !isProtocol;
		}
	}

//可以看出 attributes 的设置和释放都在一起使得代码的可读性得到了提高。
```



>* attribute((overloadable)) 方法重载

```

<!-- 能够在 c 的函数上实现方法重载。即同样的函数名函数能够对不同参数在编译时能够自动根据参数来选择定义的函数 -->

__attribute__((overloadable)) void printArgument(int number){
    NSLog(@“Add Int %i”, number);
}
__attribute__((overloadable)) void printArgument(NSString *number){
    NSLog(@“Add NSString %@“, number);
}
__attribute__((overloadable)) void printArgument(NSNumber *number){
    NSLog(@“Add NSNumber %@“, number);
}

```

>* attribute((objc_designated_initializer)) 指定内部实现的初始化方法

```

#ifndef NS_DESIGNATED_INITIALIZER
#if __has_attribute(objc_designated_initializer)
#define NS_DESIGNATED_INITIALIZER __attribute__((objc_designated_initializer))
#else
#define NS_DESIGNATED_INITIALIZER
#endif
#endif


<!-- - (instancetype)initWithFrame:(CGRect)frame NS_DESIGNATED_INITIALIZER; -->

1)如果是 objc_designated_initializer 初始化的方法必须调用覆盖实现 super 的 objc_designated_initializer 方法。


2)如果不是 objc_designated_initializer 的初始化方法，但是该类有 objc_designated_initializer 的初始化方法，那么必须调用该类的 objc_designated_initializer 方法或者非 objc_designated_initializer 方法，而不能够调用 super 的任何初始化方法。

```

>* attribute((objc_subclassing_restricted)) 指定不能有子类


>* attribute((objc_requires_super)) 子类继承必须调用 super

>* attribute((const)) 重复调用相同数值参数优化返回

```
编译器的一种优化处理方式
```

>* attribute((constructor(PRIORITY))) 和 attribute((destructor(PRIORITY)))

```
1、PRIORITY 是指执行的优先级，main 函数执行之前会执行 constructor，main 函数执行后会执行 destructor，+load 会比 constructor 执行的更早点，因为动态链接器加载 Mach-O 文件时会先加载每个类，需要 +load 调用 之后，然后才会调用所有的 constructor 方法。


2、通过这个特性，可以做些比较好玩的事情，比如说类已经 load 完了，是不是可以在 constructor 中对想替换的类进行替换，而不用加在特定类的 +load 方法里。

```



### 逆向 Mach-O 文件


>* Mobilesubstrate 提供了三个模块来方便开发。

```

<!-- 1、MobileHooker：利用 method swizzling 技术定义一些宏和函数来替换系统或者目标函数。 -->

<!-- 2、MobileLoader：在程序启动时将我们写的破解程序用的第三方库tweak注入进去。 -->
怎么注入的呢，还记得先前说的 clang attribute 里的一个 attribute((constructor)) 么，它会在 main 执行之前执行，所以把我们的 hook 放在这里就可以了。

<!-- 3、Safe mode：类似安全模式，会禁用的改动。 -->

Mach-O 的结构有 Header，Load commands 和 Data；
Mobileloader 会通过修改二进制的 loadCommands 来先把自己注入然后再把我们写的第三方库注入进去，这样破解程序tweak就会放在 Load commands 段里面了。


```

>* 分析

```
1、网络相关的分析：可以用常用那些抓包工具，比如 Charles、WireShark、tcpdump

2、静态分析： 通过砸壳，反汇编，classdump 头文件来分析 app 的架构，对应的常用工具dumpdecrypted，hopper disassembler 和 class_dump

3、运行时的分析： cycript，远程断点调试lldb+debugserver，logify。

<!-- http://blog.imjun.net/posts/convert-iOS-app-to-dynamic-library/ -->


```

### dyld动态链接

>* 生成可执行文件后就是在启动时进行动态链接了，进行符号和地址的绑定。

```
首先会加载所依赖的 dylibs，修正地址偏移，因为 iOS 会用 ASLR 来做地址偏移避免攻击，确定 Non-Lazy Pointer 地址进行符号地址绑定，加载所有类;
最后执行 load 方法和 clang attribute 的 constructor 修饰函数。

```

>* 符号表

```
<!-- devzkndeMacBook-Pro:~ devzkn$   xcrun clang SayHi.o Foo.o -Wl, /System/Library/Frameworks/Foundation.framework/Foundation -->

每个函数，全局变量和类都是通过符号的形式来定义和使用的，当把目标文件（.o）链接成一个执行文件(.out)时，
链接器在目标文件和动态库之间对符号做解析处理.

<!-- 一、使用nm 工具 具体分析-->


<!--1、 devzkndeMacBook-Pro:~ devzkn$ xcrun nm -nm SayHi.o -->
                 (undefined) external _OBJC_CLASS_$_Foo      <!-- Foo 的 OC 符号 -->
                 (undefined) external _objc_autoreleasePoolPop <!--  (undefined) external 表示未实现非私有，如果是私有就是 non-external -->
                 (undefined) external _objc_autoreleasePoolPush
                 (undefined) external _objc_msgSend
0000000000000000 (__TEXT,__text) external _main <!--  external _main 表示 main() 函数，处理 0 地址，将要到 TEXT,text section -->


<!-- 2、Foo.o  目标文件 -->
devzkndeMacBook-Pro:~ devzkn$ xcrun nm -nm Foo.o
                 (undefined) external _NSLog
                 (undefined) external _OBJC_CLASS_$_NSObject
                 (undefined) external _OBJC_METACLASS_$_NSObject
                 (undefined) external ___CFConstantStringClassReference
                 (undefined) external __objc_empty_cache
0000000000000000 (__TEXT,__text) non-external -[Foo say]
0000000000000060 (__DATA,__objc_const) non-external l_OBJC_METACLASS_RO_$_Foo
00000000000000a8 (__DATA,__objc_const) non-external l_OBJC_$_INSTANCE_METHODS_Foo
00000000000000c8 (__DATA,__objc_const) non-external l_OBJC_CLASS_RO_$_Foo
0000000000000110 (__DATA,__objc_data) external _OBJC_METACLASS_$_Foo
0000000000000138 (__DATA,__objc_data) external _OBJC_CLASS_$_Foo

<!-- 因为 undefined 符号表示该文件类未实现的，所以在目标文件和 Fundation framework 动态库做链接处理时，链接器会尝试解析所有的 undefined 符号。 -->

<!-- 3、链接器通过动态库解析成符号会记录是通过哪个动态库解析的，路径也会一起记录  -->
<!-- 可执行文件 -->
devzkndeMacBook-Pro:~ devzkn$ xcrun nm -nm a.out
                 (undefined) external _NSLog (from Foundation)
                 (undefined) external _OBJC_CLASS_$_NSObject (from CoreFoundation)
                 (undefined) external _OBJC_METACLASS_$_NSObject (from CoreFoundation)
                 (undefined) external ___CFConstantStringClassReference (from CoreFoundation)
                 (undefined) external __objc_empty_cache (from libobjc)
                 (undefined) external _objc_autoreleasePoolPop (from libobjc)
                 (undefined) external _objc_autoreleasePoolPush (from libobjc)
                 (undefined) external _objc_msgSend (from libobjc)
                 (undefined) external dyld_stub_binder (from libSystem)
0000000100000000 (__TEXT,__text) [referenced dynamically] external __mh_execute_header
0000000100000e90 (__TEXT,__text) external _main
0000000100000f10 (__TEXT,__text) non-external -[Foo say]
0000000100001130 (__DATA,__objc_data) external _OBJC_METACLASS_$_Foo
0000000100001158 (__DATA,__objc_data) external _OBJC_CLASS_$_Foo

undefined 的符号，有了更多信息，可以知道在哪个动态库能够找到


<!-- 4、通过 otool 可以找到所需库在哪 -->

devzkndeMacBook-Pro:~ devzkn$ xcrun otool -L a.out
a.out:
	/System/Library/Frameworks/Foundation.framework/Versions/C/Foundation (compatibility version 300.0.0, current version 1349.63.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1238.50.2)
	/System/Library/Frameworks/CoreFoundation.framework/Versions/A/CoreFoundation (compatibility version 150.0.0, current version 1349.64.0)
	/usr/lib/libobjc.A.dylib (compatibility version 1.0.0, current version 228.0.0)


<!-- libSystem 里有很多我们熟悉的lib-->

1）libdispatch：GCD

2）libsystem_c：C语言库

3）libsystem_blocks：Block

4）libcommonCrypto：加密，比如md5

<!-- dylib 这种格式的表示是动态链接的，编译的时候不会被编译到执行文件中，在程序执行的时候才 link，这样就不用算到包的大小里，而且也能够不更新执行程序就能够更新库。 -->


<!-- 5、打印什么库被加载了 -->
devzkndeMacBook-Pro:~ devzkn$ (export DYLD_PRINT_LIBRARIES=; ./a.out )

dyld: loaded: /Users/devzkn/./a.out
dyld: loaded: /System/Library/Frameworks/Foundation.framework/Versions/C/Foundation
dyld: loaded: /usr/lib/libSystem.B.dylib
dyld: loaded: /System/Library/Frameworks/CoreFoundation.framework/Versions/A/CoreFoundation
dyld: loaded: /usr/lib/libobjc.A.dylib
dyld: loaded: /usr/lib/libauto.dylib
dyld: loaded: /System/Library/Frameworks/DiskArbitration.framework/Versions/A/DiskArbitration
dyld: loaded: /usr/lib/libarchive.2.dylib
dyld: loaded: /usr/lib/libDiagnosticMessagesClient.dylib
dyld: loaded: /usr/lib/libicucore.A.dylib
dyld: loaded: /usr/lib/libxml2.2.dylib
dyld: loaded: /usr/lib/libz.1.dylib
dyld: loaded: /System/Library/Frameworks/CFNetwork.framework/Versions/A/CFNetwork
dyld: loaded: /System/Library/Frameworks/SystemConfiguration.framework/Versions/A/SystemConfiguration
dyld: loaded: /System/Library/Frameworks/CoreServices.framework/Versions/A/CoreServices
dyld: loaded: /usr/lib/liblangid.dylib
dyld: loaded: /System/Library/Frameworks/IOKit.framework/Versions/A/IOKit
dyld: loaded: /usr/lib/libCRFSuite.dylib
dyld: loaded: /usr/lib/libc++.1.dylib
dyld: loaded: /usr/lib/libc++abi.dylib
dyld: loaded: /usr/lib/system/libcache.dylib
dyld: loaded: /usr/lib/system/libcommonCrypto.dylib
dyld: loaded: /usr/lib/system/libcompiler_rt.dylib
dyld: loaded: /usr/lib/system/libcopyfile.dylib
dyld: loaded: /usr/lib/system/libcorecrypto.dylib
dyld: loaded: /usr/lib/system/libdispatch.dylib
dyld: loaded: /usr/lib/system/libdyld.dylib
dyld: loaded: /usr/lib/system/libkeymgr.dylib
dyld: loaded: /usr/lib/system/liblaunch.dylib
dyld: loaded: /usr/lib/system/libmacho.dylib
dyld: loaded: /usr/lib/system/libquarantine.dylib
dyld: loaded: /usr/lib/system/libremovefile.dylib
dyld: loaded: /usr/lib/system/libsystem_asl.dylib
dyld: loaded: /usr/lib/system/libsystem_blocks.dylib
dyld: loaded: /usr/lib/system/libsystem_c.dylib
dyld: loaded: /usr/lib/system/libsystem_configuration.dylib
dyld: loaded: /usr/lib/system/libsystem_coreservices.dylib
dyld: loaded: /usr/lib/system/libsystem_darwin.dylib
dyld: loaded: /usr/lib/system/libsystem_dnssd.dylib
dyld: loaded: /usr/lib/system/libsystem_info.dylib
dyld: loaded: /usr/lib/system/libsystem_m.dylib
dyld: loaded: /usr/lib/system/libsystem_malloc.dylib
dyld: loaded: /usr/lib/system/libsystem_network.dylib
dyld: loaded: /usr/lib/system/libsystem_networkextension.dylib
dyld: loaded: /usr/lib/system/libsystem_notify.dylib
dyld: loaded: /usr/lib/system/libsystem_sandbox.dylib
dyld: loaded: /usr/lib/system/libsystem_secinit.dylib
dyld: loaded: /usr/lib/system/libsystem_kernel.dylib
dyld: loaded: /usr/lib/system/libsystem_platform.dylib
dyld: loaded: /usr/lib/system/libsystem_pthread.dylib
dyld: loaded: /usr/lib/system/libsystem_symptoms.dylib
dyld: loaded: /usr/lib/system/libsystem_trace.dylib
dyld: loaded: /usr/lib/system/libunwind.dylib
dyld: loaded: /usr/lib/system/libxpc.dylib
dyld: loaded: /usr/lib/closure/libclosured.dylib
dyld: loaded: /System/Library/Frameworks/Security.framework/Versions/A/Security
dyld: loaded: /usr/lib/libenergytrace.dylib
dyld: loaded: /usr/lib/libbsm.0.dylib
dyld: loaded: /usr/lib/system/libkxld.dylib
dyld: loaded: /usr/lib/libOpenScriptingUtil.dylib
dyld: loaded: /usr/lib/libcoretls.dylib
dyld: loaded: /usr/lib/libcoretls_cfhelpers.dylib
dyld: loaded: /usr/lib/libpam.2.dylib
dyld: loaded: /usr/lib/libsqlite3.dylib
dyld: loaded: /usr/lib/libxar.1.dylib
dyld: loaded: /usr/lib/libbz2.1.0.dylib
dyld: loaded: /usr/lib/liblzma.5.dylib
dyld: loaded: /usr/lib/libnetwork.dylib
dyld: loaded: /usr/lib/libapple_nghttp2.dylib
dyld: loaded: /usr/lib/libpcap.A.dylib
dyld: loaded: /usr/lib/libboringssl.dylib
dyld: loaded: /usr/lib/libusrtcp.dylib
dyld: loaded: /usr/lib/libapple_crypto.dylib
dyld: loaded: /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/FSEvents.framework/Versions/A/FSEvents
dyld: loaded: /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/CarbonCore.framework/Versions/A/CarbonCore
dyld: loaded: /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/Metadata.framework/Versions/A/Metadata
dyld: loaded: /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/OSServices.framework/Versions/A/OSServices
dyld: loaded: /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/SearchKit.framework/Versions/A/SearchKit
dyld: loaded: /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/AE.framework/Versions/A/AE
dyld: loaded: /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/LaunchServices
dyld: loaded: /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/DictionaryServices.framework/Versions/A/DictionaryServices
dyld: loaded: /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/SharedFileList.framework/Versions/A/SharedFileList
dyld: loaded: /System/Library/Frameworks/NetFS.framework/Versions/A/NetFS
dyld: loaded: /System/Library/PrivateFrameworks/NetAuth.framework/Versions/A/NetAuth
dyld: loaded: /System/Library/PrivateFrameworks/login.framework/Versions/A/Frameworks/loginsupport.framework/Versions/A/loginsupport
dyld: loaded: /System/Library/PrivateFrameworks/TCC.framework/Versions/A/TCC
dyld: loaded: /usr/lib/libmecabra.dylib
dyld: loaded: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/ApplicationServices
dyld: loaded: /System/Library/Frameworks/CoreGraphics.framework/Versions/A/CoreGraphics
dyld: loaded: /System/Library/Frameworks/CoreText.framework/Versions/A/CoreText
dyld: loaded: /System/Library/Frameworks/ImageIO.framework/Versions/A/ImageIO
dyld: loaded: /System/Library/Frameworks/ColorSync.framework/Versions/A/ColorSync
dyld: loaded: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/ATS.framework/Versions/A/ATS
dyld: loaded: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/ColorSyncLegacy.framework/Versions/A/ColorSyncLegacy
dyld: loaded: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/HIServices.framework/Versions/A/HIServices
dyld: loaded: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/LangAnalysis.framework/Versions/A/LangAnalysis
dyld: loaded: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/PrintCore.framework/Versions/A/PrintCore
dyld: loaded: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/QD.framework/Versions/A/QD
dyld: loaded: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/SpeechSynthesis.framework/Versions/A/SpeechSynthesis
dyld: loaded: /System/Library/PrivateFrameworks/SkyLight.framework/Versions/A/SkyLight
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Accelerate
dyld: loaded: /System/Library/Frameworks/CoreDisplay.framework/Versions/A/CoreDisplay
dyld: loaded: /System/Library/Frameworks/IOSurface.framework/Versions/A/IOSurface
dyld: loaded: /System/Library/Frameworks/Metal.framework/Versions/A/Metal
dyld: loaded: /System/Library/PrivateFrameworks/MultitouchSupport.framework/Versions/A/MultitouchSupport
dyld: loaded: /System/Library/Frameworks/QuartzCore.framework/Versions/A/QuartzCore
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vImage.framework/Versions/A/vImage
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/vecLib
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/libvDSP.dylib
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/libBNNS.dylib
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/libQuadrature.dylib
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/libvMisc.dylib
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/libLAPACK.dylib
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/libBLAS.dylib
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/libLinearAlgebra.dylib
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/libSparse.dylib
dyld: loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/libSparseBLAS.dylib
dyld: loaded: /System/Library/PrivateFrameworks/IOAccelerator.framework/Versions/A/IOAccelerator
dyld: loaded: /System/Library/PrivateFrameworks/IOPresentment.framework/Versions/A/IOPresentment
dyld: loaded: /System/Library/PrivateFrameworks/DSExternalDisplay.framework/Versions/A/DSExternalDisplay
dyld: loaded: /System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libCoreFSCache.dylib
dyld: loaded: /System/Library/Frameworks/CoreImage.framework/Versions/A/CoreImage
dyld: loaded: /System/Library/Frameworks/CoreVideo.framework/Versions/A/CoreVideo
dyld: loaded: /System/Library/Frameworks/OpenGL.framework/Versions/A/OpenGL
dyld: loaded: /System/Library/PrivateFrameworks/GraphVisualizer.framework/Versions/A/GraphVisualizer
dyld: loaded: /System/Library/Frameworks/MetalPerformanceShaders.framework/Versions/A/MetalPerformanceShaders
dyld: loaded: /usr/lib/libFosl_dynamic.dylib
dyld: loaded: /System/Library/PrivateFrameworks/FaceCore.framework/Versions/A/FaceCore
dyld: loaded: /System/Library/Frameworks/OpenCL.framework/Versions/A/OpenCL
dyld: loaded: /usr/lib/libcompression.dylib
dyld: loaded: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/ATS.framework/Versions/A/Resources/libFontParser.dylib
dyld: loaded: /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/ATS.framework/Versions/A/Resources/libFontRegistry.dylib
dyld: loaded: /System/Library/Frameworks/ImageIO.framework/Versions/A/Resources/libJPEG.dylib
dyld: loaded: /System/Library/Frameworks/ImageIO.framework/Versions/A/Resources/libTIFF.dylib
dyld: loaded: /System/Library/Frameworks/ImageIO.framework/Versions/A/Resources/libPng.dylib
dyld: loaded: /System/Library/Frameworks/ImageIO.framework/Versions/A/Resources/libGIF.dylib
dyld: loaded: /System/Library/Frameworks/ImageIO.framework/Versions/A/Resources/libJP2.dylib
dyld: loaded: /System/Library/Frameworks/ImageIO.framework/Versions/A/Resources/libRadiance.dylib
dyld: loaded: /System/Library/PrivateFrameworks/AppleJPEG.framework/Versions/A/AppleJPEG
dyld: loaded: /System/Library/PrivateFrameworks/MetalTools.framework/Versions/A/MetalTools
dyld: loaded: /System/Library/Frameworks/MetalPerformanceShaders.framework/Frameworks/MPSCore.framework/Versions/A/MPSCore
dyld: loaded: /System/Library/Frameworks/MetalPerformanceShaders.framework/Frameworks/MPSImage.framework/Versions/A/MPSImage
dyld: loaded: /System/Library/Frameworks/MetalPerformanceShaders.framework/Frameworks/MPSMatrix.framework/Versions/A/MPSMatrix
dyld: loaded: /System/Library/Frameworks/MetalPerformanceShaders.framework/Frameworks/MPSNeuralNetwork.framework/Versions/A/MPSNeuralNetwork
dyld: loaded: /System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGLU.dylib
dyld: loaded: /System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGFXShared.dylib
dyld: loaded: /System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGL.dylib
dyld: loaded: /System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGLImage.dylib
dyld: loaded: /System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libCVMSPluginSupport.dylib
dyld: loaded: /System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libCoreVMClient.dylib
dyld: loaded: /usr/lib/libcups.2.dylib
dyld: loaded: /System/Library/Frameworks/Kerberos.framework/Versions/A/Kerberos
dyld: loaded: /System/Library/Frameworks/GSS.framework/Versions/A/GSS
dyld: loaded: /usr/lib/libresolv.9.dylib
dyld: loaded: /usr/lib/libiconv.2.dylib
dyld: loaded: /System/Library/PrivateFrameworks/Heimdal.framework/Versions/A/Heimdal
dyld: loaded: /usr/lib/libheimdal-asn1.dylib
dyld: loaded: /System/Library/Frameworks/OpenDirectory.framework/Versions/A/OpenDirectory
dyld: loaded: /System/Library/PrivateFrameworks/CommonAuth.framework/Versions/A/CommonAuth
dyld: loaded: /System/Library/Frameworks/OpenDirectory.framework/Versions/A/Frameworks/CFOpenDirectory.framework/Versions/A/CFOpenDirectory
dyld: loaded: /System/Library/Frameworks/SecurityFoundation.framework/Versions/A/SecurityFoundation
dyld: loaded: /System/Library/PrivateFrameworks/APFS.framework/Versions/A/APFS
dyld: loaded: /usr/lib/libutil.dylib
dyld: loaded: /System/Library/Frameworks/CoreAudio.framework/Versions/A/CoreAudio
dyld: loaded: /System/Library/Frameworks/AudioToolbox.framework/Versions/A/AudioToolbox
dyld: loaded: /System/Library/PrivateFrameworks/AppleSauce.framework/Versions/A/AppleSauce
dyld: loaded: /System/Library/PrivateFrameworks/LinguisticData.framework/Versions/A/LinguisticData
dyld: loaded: /usr/lib/libmarisa.dylib
dyld: loaded: /System/Library/PrivateFrameworks/Lexicon.framework/Versions/A/Lexicon
dyld: loaded: /usr/lib/libChineseTokenizer.dylib
dyld: loaded: /usr/lib/libcmph.dylib
dyld: loaded: /System/Library/PrivateFrameworks/LanguageModeling.framework/Versions/A/LanguageModeling
dyld: loaded: /System/Library/Frameworks/CoreData.framework/Versions/A/CoreData
dyld: loaded: /System/Library/PrivateFrameworks/CoreEmoji.framework/Versions/A/CoreEmoji
dyld: loaded: /System/Library/Frameworks/ServiceManagement.framework/Versions/A/ServiceManagement
dyld: loaded: /System/Library/PrivateFrameworks/BackgroundTaskManagement.framework/Versions/A/BackgroundTaskManagement
dyld: loaded: /usr/lib/libxslt.1.dylib
2018-03-21 18:54:18.703 a.out[62468:19635763] hi there again!

<!-- 因为 Fundation 还会依赖一些其它的动态库，其它的库还会再依赖更多的库，这样相互依赖的符号会很多，需要处理的时间也会比较长，这里系统上的动态链接器会使用共享缓存，共享缓存在 /var/db/dyld/ -->
devzkndeMacBook-Pro:~ devzkn$ ls -lrt /var/db/dyld/
total 3255808
-rw-r--r--  1 root  wheel   518811648 Nov 27 18:08 dyld_shared_cache_i386
-rw-r--r--  1 root  wheel      137234 Nov 27 18:08 dyld_shared_cache_i386.map
-rw-r--r--  1 root  wheel  1147727872 Nov 27 18:09 dyld_shared_cache_x86_64h
-rw-r--r--  1 root  wheel      291583 Nov 27 18:09 dyld_shared_cache_x86_64h.map


当加载 Mach-O 文件时动态链接器会先检查共享内存是否有。每个进程都会在自己地址空间映射这些共享缓存，这样可以优化启动速度。

https://www.mikeash.com/pyblog/friday-qa-2012-11-09-dyld-dynamic-linking-on-os-x.html



```


>*[dyld 做了些什么事](https://www.mikeash.com/pyblog/friday-qa-2012-11-09-dyld-dynamic-linking-on-os-x.html)

```

1)kernel 做启动程序初始准备，开始由dyld负责。


2)基于非常简单的原始栈为 kernel 设置进程来启动自身。

3)使用共享缓存来处理递归依赖带来的性能问题，ImageLoader 会读取二进制文件，其中包含了我们的类，方法等各种符号。

4)立即绑定 non-lazy 的符号并设置用于 lazy bind 的必要表，将这些库 link 到执行文件里。

5)为可执行文件运行静态初始化。

6)设置参数到可执行文件的 main 函数并调用它。

7)在执行期间，通过绑定符号处理对 lazily-bound 符号存根的调用提供 runtime 动态加载服务（通过 dl*() 这个 API ），并为gdb和其它调试器提供钩子以获得关键信息。runtime 会调用 map_images 做解析和处理，load_images 来调用 call_load_methods 方法遍历所有加载了的 Class，按照继承层级依次调用 +load 方法。

8)在 mian 函数返回后运行 static terminator。

在某些情况下，一旦 main 函数返回，就需要调用 libSystem 的 _exit。

<!-- 原文 https://www.mikeash.com/pyblog/friday-qa-2012-11-09-dyld-dynamic-linking-on-os-x.html -->

<!-- void load_images(const char *path __unused, const struct mach_header *mh) -->

void
load_images(const char *path __unused, const struct mach_header *mh)
{
    // Return without taking locks if there are no +load methods here.
    if (!hasLoadMethods((const headerType *)mh)) return;

    recursive_mutex_locker_t lock(loadMethodLock);

    // Discover load methods
    {
        rwlock_writer_t lock2(runtimeLock);
        prepare_load_methods((const headerType *)mh);
    }

    // Call +load methods (without runtimeLock - re-entrant)
    call_load_methods();
}



<!-- load_images(const char *path __unused, const struct mach_header *mh) -->
获取所有类的列表然后收集其中的 +load 方法；Class 的 +load 是先执行的，然后执行 Category 的

<!-- void prepare_load_methods(const headerType *mhdr) -->

遍历 Class 的 +load 方法时会执行 schedule_class_load 这个方法，这个方法会递归到根节点来满足 Class 收集完整关系树的需求。

<!-- void call_load_methods(void)  创建一个 autoreleasePool 使用函数指针来动态调用类和 Category 的 +load 方法。-->

void call_load_methods(void)
{
    static bool loading = NO;
    bool more_categories;

    loadMethodLock.assertLocked();

    // Re-entrant calls do nothing; the outermost call will finish the job.
    if (loading) return;
    loading = YES;

    void *pool = objc_autoreleasePoolPush();

    do {
        // 1. Repeatedly call class +loads until there aren't any more
        while (loadable_classes_used > 0) {
            call_class_loads();
        }

        // 2. Call category +loads ONCE
        more_categories = call_category_loads();

        // 3. Run more +loads if there are classes OR more untried categories
    } while (loadable_classes_used > 0  ||  more_categories);

    objc_autoreleasePoolPop(pool);

    loading = NO;
}

```

>*  [Cocoa 的 Fundation 库](https://github.com/AaronYi/gnustep-base)

```
<!-- https://github.com/zhangkn/gnustep-base -->
/Users/devzkn/code/github/gnustep-base/Headers/Foundation 

<!-- /Users/devzkn/code/github/gnustep-base/Source/NSFileManager.m  -->

```

>* [dyld 是开源](https://github.com/opensource-apple/dyld)

```
https://github.com/iOSHacking/dyld

Optimizing App Startup Time: https://developer.apple.com/videos/play/wwdc2016/406/

```


### LLVM 工具链


>* 获取 LLVM

```
<!-- #先下载 LLVM -->

svn co http://llvm.org/svn/llvm-project/llvm/trunk llvm

<!-- #在 LLVM 的 tools 目录下下载 Clang -->

cd llvm/tools

svn co http://llvm.org/svn/llvm-project/cfe/trunk clang

<!-- #在 LLVM 的 projects 目录下下载 compiler-rt，libcxx，libcxxabi -->

cd ../projects

svn co http://llvm.org/svn/llvm-project/compiler-rt/trunk compiler-rt

svn co http://llvm.org/svn/llvm-project/libcxx/trunk libcxx

svn co http://llvm.org/svn/llvm-project/libcxxabi/trunk libcxxabi

<!-- #在 Clang 的 tools 下安装 extra 工具 -->

cd ../tools/clang/tools
svn co http://llvm.org/svn/llvm-project/clang-tools-extra/trunk extra

```

>* 编译 LLVM

```
brew install gcc

brew install cmake

mkdir build

cd build

cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=ON -DLLVM_TARGETS_TO_BUILD="AArch64;X86" -G "Unix Makefiles" ..

make j8

<!-- #安装 -->
make install
#如果找不到标准库，Xcode 需要安装 xcode-select --install

<!-- #如果希望是 xcodeproject 方式 build 可以使用 -GXcode -->
mkdir xcodeBuild

cd xcodeBuild

cmake -GXcode /path/to/llvm/source

<!-- 在 bin 下存放着工具链，有了这些工具链就能够完成源码编译了。 -->



```

>* LLVM 源码工程目录介绍

```
llvm/examples/ - 使用 LLVM IR 和 JIT 的例子。

llvm/include/ - 导出的头文件。

llvm/lib/ - 主要源文件都在这里。

llvm/project/ - 创建自己基于 LLVM 的项目的目录。

llvm/test/ - 基于 LLVM 的回归测试，健全检察。

llvm/suite/ - 正确性，性能和基准测试套件。

llvm/tools/ - 基于 lib 构建的可以执行文件，用户通过这些程序进行交互，-help 可以查看各个工具详细使用。

llvm/utils/ - LLVM 源代码的实用工具，比如，查找 LLC 和 LLI 生成代码差异工具， Vim 或 Emacs 的语法高亮工具等。

```

>* lib 目录介绍

```
llvm/lib/IR/ - 核心类比如 Instruction 和 BasicBlock。

llvm/lib/AsmParser/ - 汇编语言解析器。

llvm/lib/Bitcode/ - 读取和写入字节码

llvm/lib/Analysis/ - 各种对程序的分析，比如 Call Graphs，Induction Variables，Natural Loop Identification 等等。

llvm/lib/Transforms/ - IR-to-IR 程序的变换。

llvm/lib/Target/ - 对像 X86 这样机器的描述。

llvm/lib/CodeGen/ - 主要是代码生成，指令选择器，指令调度和寄存器分配。

llvm/lib/ExecutionEngine/ - 在解释执行和JIT编译场景能够直接在运行时执行字节码的库。

```

>* 工具链命令介绍

```
llvm-as - 汇编器，将 .ll 汇编成字节码。

llvm-dis - 反汇编器，将字节码编成可读的 .ll 文件。

opt - 字节码优化器。

llc - 静态编译器，将字节码编译成汇编代码。

lli - 直接执行 LLVM 字节码。

llvm-link - 字节码链接器，可以把多个字节码文件链接成一个。

llvm-ar - 字节码文件打包器。

llvm-lib - LLVM lib.exe 兼容库工具。

llvm-nm - 列出字节码和符号表。

llvm-config - 打印 LLVM 编译选项。

llvm-diff - 对两个进行比较。

llvm-cov - 输出 coverage infomation。

llvm-profdata - Profile 数据工具。

llvm-stress - 生成随机 .ll 文件。

llvm-symbolizer - 地址对应源码位置，定位错误。

llvm-dwarfdump - 打印 DWARF。

```

>* 调试工具

```
bugpoint - 自动测试案例工具

llvm-extract - 从一个 LLVM 的模块里提取一个函数。

llvm-bcanalyzer - LLVM 字节码分析器。

```

>* 开发工具

```
FileCheck - 灵活的模式匹配文件验证器。

tblgen - C++ 代码生成器。

lit - LLVM 集成测试器。

llvm-build - LLVM 构建工程时需要的工具。

llvm-readobj - LLVM Object 结构查看器。

```

>* 静态分析在整个 LLVM 中的位置

![](/images/posts/{{page.title}}/llvm.png)


### [Swift 编译](https://github.com/apple/swift)

https://swift.org/contributing/#contributing-code







### see also 

- [LLDB与汇编调试-提高你的调试效率](https://elliotsomething.github.io/2017/03/14/LLDB%E4%B8%8E%E6%B1%87%E7%BC%96%E8%B0%83%E8%AF%95/)

```

<!-- 常见的寄存器 -->

累加寄存器	AX(16位)	EAX（32位）	RAX（64位）
基址寄存器	BX	EBX RBX
计数寄存器	CX	ECX	RCX
数据寄存器	DX	EDX	RDX
堆栈基指针	BP	EBP	RBP
变址寄存器	SI	ESI RSI
堆栈顶指针	SP	ESP	RSP
指令寄存器	IP	EIP RIP

<!-- 常见的汇编指令 -->

<!-- 一、数据传送指令 -->

<!-- mov – movw（16位）、movl（32位）、movq（64位） -->
寄存器寻址，mov 指令为双操作数指令，两个操作数中必须有一个是寄存器


<!-- push： 入栈指令 -->

入栈时高位字节先入栈，低位字节后入栈


<!-- pop： 出栈指令 -->

<!-- XCHG(eXCHanG)：交换指令: 将两操作数值交换. -->

<!-- XLAT(TRANSLATE)： 换码指令: 把一种代码转换为另一种代码. -->

<!-- PUSHF (PUSH the Flags)： 标志进栈指令，　PUSHF //将标志寄存器的值压入堆栈顶部, 同时栈指针SP值减2 -->

<!-- POPF (POP the Flags)： 标志出栈指令，POPF //与PUSHF相反, 从堆栈的顶部弹出两个字节送到PSW寄存器中, 同时堆栈指针值加2 -->





<!-- 二、算术指令 -->

ADD(ADD DST , SRC)： 加法指令

ADC( ADd with Carry)带进位加法指令

AAA (ASCII Adjust for Addition) 加法的ASCII调整指令

SUB(SUB DST , SRC)减法：不带借位的减法指令

SBB ( SuBtract with Borrow) 带借位减法指令
AAS (ASCII Adjust for Subtraction) 减法的ASCII调整指令
DEC(Decrement)减1，执行操作：OPR = OPR - 1

NEG(Negate)求补，执行操作：opr = 0- opr //将操作数按位求反后末位加1.


CMP(Compare)比较

<!-- 三、逻辑指令 -->
<!-- 四、串处理指令 -->

<!-- 五、控制转移指令 -->
<!-- 1、JMP(jmp) 无条件转移指令 -->


JZ(或JE)(Jump if zero,or equal) 结果为零(或相等)则转移

JNZ(或JNE)(Jump if not zero,or not equal) 结果不为零(或不相等)则转移

JS(Jump if sign) 结果为负则转移

JNS(Jump if not sign) 结果为正则转移

JO(Jump if overflow) 溢出则转移

JNO(Jump if not overflow) 不溢出则转移

JL(或LNGE)(Jump if less,or not greater or equal) 小于,或者不大于或者等于则转移

JNL(或JGE)(Jump if not less,or greater or equal)不小于,或者大于或者等于则转移

JLE(或JNG)(Jump if less or equal,or not greater) 小于或等于,或者不大于则转移

JNLE(或JG)(Jump if not less or equal,or greater) 不小于或等于,或者大于则转移


<!-- 2、LOOP 循环指令 -->

LOOPZ/LOOPE 当为零或相等时循环指令

LOOPNZ/LOOPNE 当不为零或不相等时循环指令

CALL：调用指令，先将过程的返回地址(即CALL的下一条指令的首地址)存入堆栈,然后转移到过程入口地址执行子程序.

RET：返回指令，子程序返回指令RET放在子程序末尾,它使子程序在执行完全部任务后返回主程序继续执行被打断后的程序.返回地址在子程序调用时入栈保存的断点地址-IP或IP和CS.


<!-- ## ARM处理器的16个寄存器 -->

r0-r3：用于存放传递给函数的参数；
r4-r11：用于存放函数的本地参数；
r12：是内部程序调用暂时寄存器。这个寄存器很特别是因为可以通过函数调用来改变它；
r13：栈指针sp(stack pointer)。在计算机科学内栈是非常重要的术语。寄存器存放了一个指向栈顶的指针。看这里了解更多关于栈的信息；
r14：是链接寄存器lr(link register)。它保存了当目前函数返回时下一个函数的地址；
r15：是程序计数器pc(program counter)。它存放了当前执行指令的地址。在每个指令执行完成后会自动增加；


<!-- https://objccn.io/issue-19-2/ #lldb调试  -->


<!-- objc_msgSend函数的定义如下： -->

void objc_msgSend(id self, SEL cmd, ...)

第一个参数是调用方法的对象，第二个参数是选择子selector，后面的参数就是调用方法后面传的参数了





```


- [How iCloud.com works?](https://github.com/prabhu/iCloud)
- [Lets study iTunes for fun and learning](https://github.com/prabhu/iTunes)
```
devzkndeMacBook-Pro:code devzkn$ scp -r  usb2222:/var/mobile/Library/Caches/sharedCaches/com.apple.iTunesStore.NSURLCache  ~/code/com.apple.iTunesStore.NSURLCache
NSFilePath=/var/mobile/Library/Caches/com.apple.storeservices/SSAppImageDatabaseCacheEntry
```
- [https://github.com/zhangkn/KNcom.apple.iTunesStore.NSURLCache.git](https://github.com/zhangkn/KNcom.apple.iTunesStore.NSURLCache.git)
- [deb](https://baike.baidu.com/item/deb)

```
一般有 5 个文件：
1、control，用了记录软件标识，版本号，平台，依赖信息等数据；

2、preinst，在解包data.tar.gz 前运行的脚本；postinst，在解包数据后运行的脚本；

3、prerm，卸载时，在删除文件之前运行的脚本；

4、postrm，在删除文件之后运行的脚本；在 Cydia 系统中，Cydia 的作者 Saurik 另外添加了一个脚本，extrainst_，作用与 postinst 类似。
```
