---
layout: post
title: launchApplicationWithIdentifier
date: 2018-04-17
tag: iOSre
site: https://zhangkn.github.io
---

###  前言


>* [code:自动解锁以及打开特定的app](https://github.com/jbtewaks/launchappandautounlock)

>* [lua代码实现解锁和打开特定app的代码 ](https://github.com/zhangkn/knlua/blob/master/popen.lua)

### 正文



>* launchApplicationWithIdentifier

```
NSTimer *timer ;

%hook SpringBoard
//applicationDidFinishLaunching
-(void)applicationDidFinishLaunching: (id)application
{
        %orig;
//创建一个定时器，定时检测   
	timer = [NSTimer scheduledTimerWithTimeInterval:60*2 target:self selector:@selector(checkHeart) userInfo:nil repeats:YES];
}

%new
- (void)checkHeart
{
	//定时检测微信是否开启
    [[UIApplication sharedApplication] launchApplicationWithIdentifier:@"com.kntencent.xin" suspended:0];
}

%end
```


### 与其他的tweak 结合起来使用，合并到同一个deb 包中


>* 放到layout对应的目录

```
 5 files changed, 3 insertions(+), 1 deletion(-)
 create mode 100755 Layout/Library/MobileSubstrate/DynamicLibraries/SBLockScreenViewController.dylib
 create mode 100644 Layout/Library/MobileSubstrate/DynamicLibraries/SBLockScreenViewController.plist
```

>* 方式二：在%ctor 中根据 processName 进行%init group


```

// # 必须放置于最后，否则找不到wxHook
%ctor
{
	if ([[[NSProcessInfo processInfo] processName] isEqualToString:@"knWeChat"])
		%init(wxHook);

	if ([[[NSProcessInfo processInfo] processName] isEqualToString:@"SpringBoard"])
		%init(sbHook);
}


<!-- tweak plist -->

{ Filter = { Bundles = ( "com.kntencent.xin","com.apple.springboard"  ); }; }

```

>* [基础的logos 语法复习](http://iphonedevwiki.net/index.php/Logos)

```

<!-- %ctor -->

1) tweak的constructor,完成初始化工作；如果不显示定义，Theos会自动生成一个%ctor,并在其中调用%init(_ungrouped)。

2) %ctor一般可以用来初始化%group,以及进行MSHookFunction等操作

<!-- %group -->

1) 该指令用于将%hook分组，便于代码管理及按条件初始化分组，必须以%end结尾。

2) 一个％group可以包含多个%hook,所有不属于某个自定义group的％hook会被隐式归类到％group_ungrouped中。

<!-- %init -->

1) 该指令用于初始化某个％group，必须在%hook或％ctor内调用；如果带参数，则初始化指定的group，如果不带参数，则初始化_ungrouped.

2) 注：切记，只有调用了％init,对应的%group才能起作用！


<!-- %property -->

这个属性是时候尝试应用下了，否则使用全局变量不太方便

1） Add a property to a %subclass just like you would with @property to a normal Objective-C subclass as well as adding new properties to existing classes within %hook.

2） %property (nonatomic|assign|retain|copy|weak|strong|getter|setter) Type name;


<!-- %subclass -->
1） Subclass block - the class is created at runtime and populated with methods. ivars are not yet supported (use associated objects).

2）  The %new specifier is needed for a method that doesn't exist in the superclass. To instantiate an object of the new class, you can use the %c operator.

3） Can be inside a %group block.
```

### see also

>* [AutoUnlock/](https://zhangkn.github.io/2018/04/AutoUnlock/)
