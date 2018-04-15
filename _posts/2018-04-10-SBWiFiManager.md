---
layout: post
title: SBWiFiManager
date: 2018-04-10
tag: iOSre
site: https://zhangkn.github.io
---

### 前言


>*  [采用MobileWiFi.framework实现 的背景](https://zhangkn.github.io/2018/04/SBWiFiManager/)

```
 实现Associate to wifi的，在iOS 版本10 之后，就比较困难，因为苹果将SBWiFiManager 的joinNetwork：password： 移除掉； 且在iOS10 中SBWiFiManager 的t_manager、t_device、t_currentNetwork 均采用 struct存储，提高了安全性。

 因此要实现自动 Associate to wifi 的话，我从hopper 中看到sb 中使用WiFiManagerClientCreate() 实现连接Wi-Fi,因此就才想到使用MobileWiFi.framework 实现是最佳的捷径
```


>* [code](https://github.com/sbtweaks/wifiutil)



### 正文


>* [SBWiFiManager.h](https://github.com/nanotech/iphoneheaders/blob/master/SpringBoard/SBWiFiManager.h)


```
@interface SBWiFiManager : NSObject {


	WiFiManagerClient* _manager;
	WiFiDeviceClient* _device;
	WiFiNetwork* _currentNetwork;
	BOOL _currentNetworkHasBeenSet;
	BOOL _powered;
	BOOL _poweredHasBeenSet;
	int _rssiThreshold;
	BOOL _joining;
	int _signalStrengthBars;
	BOOL _signalStrengthHasBeenSet;
	NSTimer* _signalStrengthTimer;
	int _shouldPollSignalStrength;
	BOOL _canPollSignalStrength;
#if __IPHONE_OS_VERSION_MAX_ALLOWED < __IPHONE_3_2
	NSMutableDictionary* _securityDict;
#endif
	id _delegate;
	unsigned _notificationID;
	double _lastSignalStrengthUpdateTime;


}
+(id)sharedInstance;
-(id)init;
-(void)scan;
-(BOOL)joining;
-(BOOL)wiFiEnabled;
-(void)setWiFiEnabled:(BOOL)enabled;
-(int)signalStrengthBars;
-(int)signalStrengthRSSI;
-(void)updateSignalStrength;
-(void)_updateSignalStrengthTimer;
-(void)cancelTrust:(BOOL)trust;
-(void)acceptTrust:(id)trust;
-(void)cancelPicker:(BOOL)picker;
-(void)userChoseNetwork:(id)network;
-(id)knownNetworks;
-(void)resetSettings;
-(void)_scanComplete:(CFArrayRef)complete;
-(void)joinNetwork:(id)network password:(id)password;
-(void)_askToJoinWithID:(unsigned)anId;
@end


<!--  -->

-(void)_joinComplete:(int)complete network:(WiFiNetwork*)network;

-(BOOL)networkRequiresPassword:(id)password;

-(void)_askToJoinWithID:(unsigned)anId;

-(void)joinNetwork:(id)network password:(id)password;

```






### 打开和关闭Wi-Fi的开关控制


>* setPowered


```
<!--  在sb 中执行 -->
            [[objc_getClass("SBWiFiManager") sharedInstance] setPowered:YES];
            [[objc_getClass("SBWiFiManager") sharedInstance] setWiFiEnabled:YES];

```



### cy分析 iOS10

>* 重要的东西采用struct，Apple的安全意思越来越高了

```

cy# [#0x1706e9f00 _ivarDescription].toString()
`<SBWiFiManager: 0x1706e9f00>:
in SBWiFiManager:
\t_lock (NSRecursiveLock*): <NSRecursiveLock: 0x1704c9ed0>
\t_callbackRunLoop (struct __CFRunLoop*): 0x1706e9f10 -> 0x17016f6c0
\t_manager (struct __WiFiManagerClient*): 0x1706e9f18 -> 0x1701a9e60
\t_device (struct __WiFiDeviceClient*): 0x1706e9f20 -> 0x100ed4b80
\t_deviceInterfaceName (NSString*): @"en0"
\t_mainThread_devicePresent (BOOL): YES
\t_currentNetwork (struct __WiFiNetwork*): 0x1706e9f38 -> 0x174626b20
\t_previousNetwork (struct __WiFiNetwork*): 0x1706e9f40 -> 0x0
\t_currentNetworkHasBeenSet (BOOL): YES
\t_currentNetworkIsIOSHotspot (BOOL): NO
\t_currentNetworkIsIOSHotspotHasBeenSet (BOOL): YES
\t_powered (BOOL): YES
\t_poweredHasBeenSet (BOOL): YES
\t_mainThread_signalStrengthBars (int): 3
\t_mainThread_signalStrengthRSSI (int): -61
\t_mainThread_signalStrengthHasBeenSet (BOOL): YES
\t_systemPathMonitor (NWSystemPathMonitor*): <NWSystemPathMonitor: 0x170c71400>
\t_primaryInterfaceUpdateTimeoutSource (NSObject<OS_dispatch_source>*): nil
\t_primaryInterface (struct __WiFiNetwork*): 0x1706e9f70 -> 0x174626b20
\t_primaryInterfaceHasBeenSet (BOOL): YES
\t_isPrimaryInterface (BOOL): YES
\t_isPrimaryInterfaceChanging (BOOL): NO
in NSObject:
\tisa (Class): SBWiFiManager (isa, 0x41a100a61c25)`

```

### [Reverse engineered C headers from various iOS frameworks.](https://github.com/sbtweaks/ios-reversed-headers)

### [Tool to generate the UDID like Apple does.](https://github.com/davidmurray/UDIDGenerator)

Tool to generate the UDID like Apple does.




### [MobileWiFi.framework](http://iphonedevwiki.net/index.php/MobileWiFi.framework)

MobileWiFi is the framework that manages WiFi functionality on iOS. It powers WiFiPicker.servicebundle, the Wi-Fi settings page and more. Its main purpose is to be a front-end to wifid




>* [Example Code](https://github.com/Cykey/wifi)

```

<!-- WiFi: https://github.com/Cykey/wifi -->

WiFi manager app that uses MobileWiFi.framework.

https://github.com/sbtweaks/wifi





<!-- Airscan: https://github.com/Cykey/airscan -->

iOS command-line WiFi stumbler.

TOOL_NAME = airscan
airscan_FILES = main.c


airscan_PRIVATE_FRAMEWORKS = MobileWiFi

airscan_CODESIGN_FLAGS = -Sentitlements.plist

include $(THEOS_MAKE_PATH)/tool.mk




<!-- airscan/entitlements.plist -->


<dict>
	<key>com.apple.wifi.manager-access</key>
	<true/>
</dict>




```



>* Retrieving a list of known networks


```

#include <MobileWiFi.h>

WiFiManagerRef manager = WiFiManagerClientCreate(kCFAllocatorDefault, 0);

CFArrayRef networks = WiFiManagerClientCopyNetworks(manager);

NSLog(@"networks: %@", networks);

CFRelease(manager);
CFRelease(networks);

```


>* Getting the WiFi signal strength

```
#include <math.h>
#include <MobileWiFi.h>

WiFiManagerRef manager = WiFiManagerClientCreate(kCFAllocatorDefault, 0);
CFArrayRef devices = WiFiManagerClientCopyDevices(manager);

WiFiDeviceClientRef client = (WiFiDeviceClientRef)CFArrayGetValueAtIndex(devices, 0);
CFDictionaryRef data = (CFDictionaryRef)WiFiDeviceClientCopyProperty(client, CFSTR("RSSI"));
CFNumberRef scaled = (CFNumberRef)WiFiDeviceClientCopyProperty(client, kWiFiScaledRSSIKey);

CFNumberRef RSSI = (CFNumberRef)CFDictionaryGetValue(data, CFSTR("RSSI_CTL_AGR"));

int raw;
CFNumberGetValue(RSSI, kCFNumberIntType, &raw);

float strength;
CFNumberGetValue(scaled, kCFNumberFloatType, &strength);
CFRelease(scaled);

strength *= -1;

// Apple uses -3.0.
int bars = (int)ceilf(strength * -3.0f);
bars = MAX(1, MIN(bars, 3));


printf("WiFi signal strength: %d dBm\n\t Bars: %d\n", raw,  bars);

CFRelease(data);
CFRelease(scaled);
CFRelease(devices);
CFRelease(manager);

```



### The missing iOS WiFi manager.






>* Makefile

```


WiFi_PRIVATE_FRAMEWORKS = MobileWiFi
WiFi_CODESIGN_FLAGS = -Ssrc/entitlements.xml


ADDITIONAL_CFLAGS = -I$(THEOS_PROJECT_DIR)/ios-reversed-headers/ -include src/DMConstants.h





include $(THEOS_MAKE_PATH)/application.mk


```


### airscan


>* setup_for_scan


```

static void setup_for_scan()


	manager = WiFiManagerClientCreate(kCFAllocatorDefault, 0);

	CFArrayRef devices = WiFiManagerClientCopyDevices(manager);



	WiFiManagerClientScheduleWithRunLoop(manager, CFRunLoopGetCurrent(), kCFRunLoopDefaultMode);





```

>* airscan/main.c

```

	const char *SSID = CFStringGetCStringPtr(WiFiNetworkGetSSID(network), kCFStringEncodingMacRoman);
	const char *BSSID = CFStringGetCStringPtr(WiFiNetworkGetProperty(network, CFSTR("BSSID")), kCFStringEncodingMacRoman);

	// RSSI.
	CFNumberRef RSSI = (CFNumberRef)WiFiNetworkGetProperty(network, CFSTR("RSSI"));


```




### [https://github.com/davidmurray/ios-reversed-headers](https://github.com/davidmurray/ios-reversed-headers)



>* [ios-reversed-headers/MobileWiFi/WiFiManager.h](https://github.com/davidmurray/ios-reversed-headers/blob/c613e45f3ee5ad9f85ec7d43906cf69ee812ec6a/MobileWiFi/WiFiManager.h)

```
#pragma mark - API

	WiFiManagerRef WiFiManagerClientCreate(CFAllocatorRef allocator, int flags);

	CFArrayRef WiFiManagerClientCopyDevices(WiFiManagerRef manager);
	CFArrayRef WiFiManagerClientCopyNetworks(WiFiManagerRef manager);

```

>* [ios-reversed-headers/SpringBoardServices/SpringBoardServices.h](https://github.com/davidmurray/ios-reversed-headers/blob/c613e45f3ee5ad9f85ec7d43906cf69ee812ec6a/SpringBoardServices/SpringBoardServices.h)

```


    // The following need com.apple.backboardd.launchapplications entitlement
    int SBSLaunchApplicationWithIdentifier(CFStringRef identifier, Boolean suspended);
    int SBSLaunchApplicationWithIdentifierAndLaunchOptions(CFStringRef identifier, CFDictionaryRef launchOptions, BOOL suspended);
    CFStringRef SBSApplicationLaunchingErrorString(int error);

    int SBSOpenSensitiveURLAndUnlock(CFURLRef url, char flags); // need com.apple.springboard.opensensitiveurl entitlement

```


>* [wifi/src/DMNetworksManager.m](https://github.com/davidmurray/wifi/blob/master/src/DMNetworksManager.m)

```
- (void)setWiFiEnabled:(BOOL)enabled
{
	// XXX: What.

	_manager = WiFiManagerClientCreate(kCFAllocatorDefault, 0);
	CFBooleanRef value = (enabled ? kCFBooleanTrue : kCFBooleanFalse);

	WiFiManagerClientSetProperty(_manager, CFSTR("AllowEnable"), value);
}



- (void)_addNetwork:(DMNetwork *)network
{
	[_networks addObject:network];
}

- (WiFiNetworkRef)_currentNetwork
{
	return _currentNetwork;
}


```

>* wifid

```

iPhone:/System/Library/CoreServices/SpringBoard.app root# ps -e |grep wifid
  249 ??         0:04.31 /usr/sbin/wifid

```

### kndump


>*  frida-ps -Ua

```

devzkndeMacBook-Pro:bin devzkn$ frida-ps -Ua
Failed to enumerate applications: unable to connect to remote frida-server: Unable to connect (connection refused)

<!-- install frida on device and mac -->



https://zhangkn.github.io/2017/12/frida/#gsc.tab=0


Start Cydia and add Frida’s repository by navigating to Manage -> Sources -> Edit -> Add and entering https://build.frida.re

devzkndeMacBook-Pro:bin devzkn$ frida-ps -Ua
 PID  Name   Identifier          
----  -----  --------------------
1130  Cydia  com.saurik.Cydia    
1125  邮件     com.apple.mobilemail


<!-- sb 直接copy即可  -->

devzkndeMacBook-Pro:bin devzkn$ scp usb2222:/System/Library/CoreServices/SpringBoard.app/SpringBoard /Users/devzkn/decrypted/jb/sb

```



>* WiFiNetworkGetSSID

```
void WiFiNetworkGetSSID() {
    _WiFiNetworkGetSSID();
    return;
}
```

>* WiFiManagerClientCreate

```

void WiFiManagerClientCreate() {
    _WiFiManagerClientCreate();
    return;
}

```


>* SBControlCenterSystemAgent


```
-[SBControlCenterSystemAgent setWifiPowered:]
```

>* SBExternalWifiDefaults

```
void * -[SBExternalWifiDefaults init](void * self, void * _cmd) {
    r0 = [self _initWithDomain:@"com.apple.preferences.network"];
    return r0;
}
```

>* SpringBoard

```

iPhone:~ root#  ps -e |grep SpringBoard
 1105 ??         0:08.29 /System/Library/CoreServices/SpringBoard.app/SpringBoard


```

>* [SBDashBoardSetupViewController]

```

@property(retain, nonatomic, getter=_wifiScanner, setter=_setWifiScanner:) SBSetupWiFiScanner *wifiScanner; // @synthesize wifiScanner=_wifiScanner;


```

>* [WiFiDataMigrator]

```

@interface WiFiDataMigrator : DataClassMigrator

-(void)removeNetworkChannelList:(WiFiManagerClientRef)arg1 ;

-(void)disableAutoJoinAdhocNetworks:(WiFiManagerClientRef)arg1 ;

-(void)turnWiFiOn:(WiFiManagerClientRef)arg1 ;

-(void)disableAskToJoin:(WiFiManagerClientRef)arg1 ;


```

### SBClientAlertItemTestRecipe


>* SBClientAlertItemTestRecipe


```

- (id)_wifiTrustAlert;
- (id)_wifiPasswordAlert;
- (id)_wifiIsEnterpriseAlert;
- (id)_wifiErrorAlert;
- (id)_wifiDontAskAlert;
- (id)_wifiBTSSPAlert;

```



### https://github.com/sbtweaks/wifiutil


iOS command-line tool for WiFi-related operation.


>*  最简单的使用方式

```
此工具很好用，如果不修改代码的话，直接写一个tweak 直接调用wifiutil 即可
```


>* Usage

```

To associate with wifi hotspot named {ssid} and with password {passwd}: wifiutil associate {ssid} -p {passwd} --最好封装一个出来，实现自动登录特定Wi-Fi的，便于自动化管理机器


```


>* wifiutil/Makefile

```

include $(THEOS)/makefiles/common.mk

TOOL_NAME = wifiutil
wifiutil_FILES = $(wildcard src/*.m*)
wifiutil_PRIVATE_FRAMEWORKS = MobileWiFi
wifiutil_CODESIGN_FLAGS = -Ssrc/entitlements.xml

ADDITIONAL_CFLAGS = -I$(THEOS_PROJECT_DIR)/headers/

include $(THEOS_MAKE_PATH)/tool.mk

```


>* https://github.com/qpiu/wifiutil/blob/master/src/entitlements.xml

```

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
	<dict>
		<key>com.apple.wifi.manager-access</key>
		<true/>
		<key>keychain-access-groups</key>
		<array>
			<string>apple</string>
			<string>com.apple.preferences</string>
		</array>
	</dict>
</plist>
```


>* wifiutil/src/UtilNetworksManager.h

```

#import <UIKit/UIKit.h>
#import "MobileWiFi/MobileWiFi.h"

@class UtilNetwork;

@interface UtilNetworksManager : NSObject {
	WiFiManagerRef      _manager;
	WiFiDeviceClientRef _client;
	WiFiNetworkRef      _currentNetwork;
	BOOL                _scanning;
	BOOL                _associating;
	NSMutableArray      *_networks;
	int 				_statusCode;
}


- (void)associateWithEncNetwork:(UtilNetwork *)network Password:(NSString *)passwd;


```


>* - (void)associateWithEncNetwork:(UtilNetwork *)network Password:(NSString *)passwd


```

	WiFiNetworkSetPassword(net, (__bridge CFStringRef)passwd);

//    int WiFiDeviceClientAssociateAsync(WiFiDeviceClientRef client, WiFiNetworkRef network, WiFiDeviceAssociateCallback callback, CFDictionaryRef dict);

		WiFiDeviceClientAssociateAsync(_client, net, (WiFiDeviceAssociateCallback)UtilAssociationCallback, 0);


```

>* wifiutil/headers/MobileWiFi/WiFiDeviceClient.h

```

    /*
     * Used to connect to a network. Example usage:
     *    (Get a WiFiNetworkRef instance from the scan results or something.)
     *    WiFiNetworkSetPassword(network, CFSTR("Password1"));
     *    WiFiDeviceClientAssociateAsync(client, network, MyCallbackFunction, NULL);
     */
    int WiFiDeviceClientAssociateAsync(WiFiDeviceClientRef client, WiFiNetworkRef network, WiFiDeviceAssociateCallback callback, CFDictionaryRef dict);


```


>* wifiutil/headers/MobileWiFi/WiFiNetwork.h

```

    void WiFiNetworkSetPassword(WiFiNetworkRef network, CFStringRef password);

```

>* control

```

==> Error: /Applications/Xcode.app/Contents/Developer/usr/bin/make package requires you to have a layout/ directory in the project root, containing the basic package structure, or a control file in the project root describing the package.

```

>* wifiutil

```

iPhone:/System/Library/CoreServices/SpringBoard.app root# which wifiutil
/usr/bin/wifiutil
iPhone:/System/Library/CoreServices/SpringBoard.app root# wifiutil
2018-04-10 16:25:08.298 wifiutil[1260:30599] [Output]  Hello, wifiutil! Type wifiutil help to see usage.
2018-04-10 16:25:08.300 wifiutil[1260:30599] [Error]  Specify arguments to use wifiutil.



2018-04-10 16:25:33.849 wifiutil[1261:30631] [Debug]  wifiutil: help
2018-04-10 16:25:33.850 wifiutil[1261:30631] 
##########   WifiUtil Usage   ##########
Scan available wifi hotspots:
		wifiutil scan
Enable wifi on iPhone:
		wifiutil enable-wifi
Disable wifi on iPhone:
		wifiutil disable-wifi
Associate to wifi:
		wifiutil associate <ssid>  --> For wifi networks without password
		wifiutil associate <ssid> -p <passwd> --> For wifi networks with password
Disassociate with current wifi network:
		wifiutil disassociate


```

>* wifiutil scan

```

iPhone:/System/Library/CoreServices/SpringBoard.app root# wifiutil scan
2018-04-10 16:26:11.091 wifiutil[1293:31008] [Output]  Hello, wifiutil! Type wifiutil help to see usage.
2018-04-10 16:26:11.094 wifiutil[1293:31008] [Debug]  wifiutil: scan
2018-04-10 16:26:13.982 wifiutil[1293:31008] [Output]  Finished scanning! 16 networks: 
                           H01A	|     c8:3a:35:5a:9:10	| channel 8	

```

>* wifiutil enable-wifi

```

iPhone:/System/Library/CoreServices/SpringBoard.app root# wifiutil enable-wifi
2018-04-10 16:27:25.651 wifiutil[1294:31385] [Output]  Hello, wifiutil! Type wifiutil help to see usage.
2018-04-10 16:27:25.653 wifiutil[1294:31385] [Debug]  wifiutil: enable-wifi
2018-04-10 16:27:25.654 wifiutil[1294:31385] [Debug]  Enable WiFi on iPhone.

```

>* wifiutil disable-wifi

```
iPhone:/System/Library/CoreServices/SpringBoard.app root# wifiutil disable-wifi
2018-04-10 16:27:57.803 wifiutil[1295:31472] [Output]  Hello, wifiutil! Type wifiutil help to see usage.
2018-04-10 16:27:57.805 wifiutil[1295:31472] [Debug]  wifiutil: disable-wifi
2018-04-10 16:27:57.806 wifiutil[1295:31472] [Debug]  Disable WiFi on iPhone.

```

>* 断开当前的网络

```
iPhone:/System/Library/CoreServices/SpringBoard.app root# wifiutil disassociate
2018-04-10 16:29:28.797 wifiutil[1314:32176] [Output]  Hello, wifiutil! Type wifiutil help to see usage.
2018-04-10 16:29:28.798 wifiutil[1314:32176] [Debug]  wifiutil: disassociate

```

>* 

```

iPhone:/System/Library/CoreServices/SpringBoard.app root# wifiutil associate @zhangkn.github.io -p zhangkn.github.io
2018-04-10 16:31:28.488 wifiutil[1321:32673] [Output]  Hello, wifiutil! Type wifiutil help to see usage.
2018-04-10 16:31:28.491 wifiutil[1321:32673] [Debug]  wifiutil: associate
2018-04-10 16:31:28.492 wifiutil[1321:32673] [Debug]  Prepare to associate with network @zhangkn.github.io, passwd: zhangkn.github.io
2018-04-10 16:31:31.275 wifiutil[1321:32673] [Output]  Finished scanning! 29 networks: 
                     liuzhikobe	|     8:10:79:e6:ab:d8	| channel 3	

2018-04-10 16:31:31.278 wifiutil[1321:32673] [Output]  Scanning is successful :) 
2018-04-10 16:31:31.279 wifiutil[1321:32673] [Output]  Found network @zhangkn.github.io :)
2018-04-10 16:31:31.452 wifiutil[1321:32673] [Debug]  Start associating
2018-04-10 16:31:32.341 wifiutil[1321:32673] [Output]  Association is successful :) 

```

### see also


- [associate ssid](https://github.com/qpiu/wifiutil/blob/f4da379420e94ace926ee283519cd9d82b85b7f0/src/main.m)

```


// 密码连接Wi-Fi

    usage = [NSString stringWithFormat:@"%@\n\t\t\x1B[36mwifiutil associate <ssid> -p <passwd> \x1B[0m--> For wifi networks with password\x1B[0m", usage];



```


- [wifichannelbar](https://github.com/janbures12/wifichannelbar/blob/5cde468d5fa0fa29cd3aec9c97b13aa1703de9da/Tweak.xm)


```

WifiChannelBar is a tweak for iOS 9.0 - 10.2 that adds current wifi channel number to the status bar.



#include <MobileWiFi/MobileWiFi.h>


%ctor
{
    HBLogDebug(@"Tweak loaded");
    wifiLibHandle = dlopen("/System/Library/PrivateFrameworks/MobileWiFi.framework/MobileWiFi", RTLD_NOW);
    libstatusbar = dlopen("/Library/MobileSubstrate/DynamicLibraries/libstatusbar.dylib", RTLD_NOW);
    if(!libstatusbar)
    {
        errorString = [NSString stringWithFormat:@"Error loading libstatusbar: %s", dlerror()];
        HBLogError(@"%@", errorString);
        error = YES;
    }

    *(void **) (&wifiManagerClientCreate) = dlsym(wifiLibHandle, "WiFiManagerClientCreate");
    *(void **) (&wifiManagerClientCopyDevices) = dlsym(wifiLibHandle, "WiFiManagerClientCopyDevices");
    *(void **) (&wifiDeviceClientCopyCurrentNetwork) = dlsym(wifiLibHandle, "WiFiDeviceClientCopyCurrentNetwork");
    *(void **) (&wifiNetworkGetProperty) = dlsym(wifiLibHandle, "WiFiNetworkGetProperty");

    manager = (*wifiManagerClientCreate)(kCFAllocatorDefault, 0);
}




<!-- wifichannelbar/entitlements.xml -->


<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
	<dict>
		<key>com.apple.wifi.manager-access</key>
		<true/>
		<key>keychain-access-groups</key>
		<array>
			<string>apple</string>
			<string>com.apple.preferences</string>
		</array>
	</dict>
</plist>


<!-- wifichannelbar/Makefile -->


include $(THEOS)/makefiles/common.mk

#export TARGET = simulator:clang
#export ARCH = x86_64

TWEAK_NAME = WifiChannelBar
WifiChannelBar_FILES = Reachability.m Tweak.xm
WifiChannelBar_FRAMEWORKS = CoreFoundation UIKit SystemConfiguration
WifiChannelBar_CODESIGN_FLAGS = -Sentitlements.xml

include $(THEOS_MAKE_PATH)/tweak.mk

after-install::
	install.exec "killall -9 SpringBoard"
include $(THEOS_MAKE_PATH)/aggregate.mk


```

- [iPhone Wi-Fi Manager SDK](https://stackoverflow.com/questions/2053114/iphone-wi-fi-manager-sdk)

```
<!-- https://github.com/davidmurray/wifi/issues/16 -->

    [[objc_getClass("SBWiFiManager") sharedInstance] setWiFiEnabled:NO];
```

- [http://iphonedevwiki.net/]

```
<!-- http://iphonedevwiki.net/index.php/User:Iosre -->


What is the class representing a mutable array in Cocoa?

NSMutableDictionary


```

- [SBAssistantController]

```

- (_Bool)uiPlugin:(id)arg1 launchApplicationWithBundleID:(id)arg2 openURL:(id)arg3 allowDismissal:(_Bool)arg4;


```


- [SBClientAlertItemTestRecipe]

```

@interface SBClientAlertItemTestRecipe : NSObject <SBTestRecipe>
{
    SBAlertItem *_item;
}

+ (id)title;
- (void).cxx_destruct;
- (id)_nextAlertItemToTest;
- (id)_mapsApp;
- (id)_anySUDescriptor;
- (void)handleVolumeDecrease;
- (void)handleVolumeIncrease;
- (void)_dismissCurrentItem;
- (id)_currentAssistantLanguage;
- (void)educationAlertWasDeactivated:(id)arg1;
- (id)_chatMMSInformationMissingAlert;
- (id)_chatMMSDelayedDownloadAlert;
- (id)_chatCMSBAlert;
- (id)_chatMessageAlert;
- (id)_chatCarrierSMSAlert;
- (id)_mapsThermalAlert;
- (id)_wifiTrustAlert;
- (id)_wifiPasswordAlert;
- (id)_wifiIsEnterpriseAlert;
- (id)_wifiErrorAlert;
- (id)_wifiDontAskAlert;
- (id)_wifiBTSSPAlert;

// Remaining properties
@property(readonly, copy) NSString *debugDescription;
@property(readonly, copy) NSString *description;
@property(readonly) unsigned long long hash;
@property(readonly) Class superclass;

@end

```

- [SBPasscodeController]



- [WiFiManagerRef](https://github.com/janbures12/wifichannelbar/blob/5cde468d5fa0fa29cd3aec9c97b13aa1703de9da/Tweak.xm)


```
WiFiManagerRef (*wifiManagerClientCreate)(CFAllocatorRef, int);
CFArrayRef (*wifiManagerClientCopyDevices)(WiFiManagerRef);
WiFiNetworkRef (*wifiDeviceClientCopyCurrentNetwork)(WiFiDeviceClientRef);
WiFiNetworkRef (*wifiNetworkGetProperty)(WiFiNetworkRef, CFStringRef);
```

- [UISettings](https://github.com/0wnTeam/UISettings/blob/39b01b1d661f4353b48a12d48a1e5d3d15f71c75/uitoggles/UIToggles.mm)

```
OpenSource Toggle System - w/ SBSettings Loader (not in source)



<!-- _askToJoinWithID -->

-(void)wifi
{
	Class SBWiFiManager = objc_getClass("SBWiFiManager");
	BOOL wistatus=![[SBWiFiManager sharedInstance] wiFiEnabled];
	[[SBWiFiManager sharedInstance]setWiFiEnabled:wistatus];
	refresh();
	[[SBWiFiManager sharedInstance] _askToJoinWithID:0];
}


<!-- SpringBoard -->

@interface SpringBoard {}
+(id)sharedBoard;
@end



-(void)shut
{
	[[objc_getClass("SpringBoard") sharedBoard] powerDown];
}
-(void)reboot
{
        [[objc_getClass("SpringBoard") sharedBoard] reboot];
}




```

- [SBServer.xm](https://github.com/bored-engineer/iOS-sbutils/blob/53cedffa0b4e4273ae3ea0c5edd2abedc1d6d151/SBServer.xm)

```

<!-- iOS utilities - replaces some broken Erica utilities and adds a bunch of new ones -->



//用于进程通信
#import <AppSupport/CPDistributedMessagingCenter.h>

/*
#import <SpringBoard/SBMediaController.h>
#import <SpringBoard/SBWiFiManager.h>
#import <SpringBoard/SBTelephonyManager.h>
#import <BluetoothManager/BluetoothManager.h> 
#import <CoreLocation/CLLocationManager.h> 
*/


%hook SpringBoard 
- (id)init {
	if ((self = %orig)) {
		CPDistributedMessagingCenter *messagingCenter = [CPDistributedMessagingCenter centerNamed:@"com.innoying.sbserver"];
		[messagingCenter runServerOnCurrentThread];
		[messagingCenter registerForMessageName:@"sbmedia" target:self selector:@selector(handleSbmediaCommand:withUserInfo:)];
		[messagingCenter registerForMessageName:@"sbtoggle" target:self selector:@selector(handleSbtoggleCommand:withUserInfo:)];

	}
	return self;
}


https://github.com/bored-engineer/iOS-sbutils/blob/master/sbtoggle.mm
<!-- 发送消息 -->

    	NSString *identifier = [[[NSString alloc] initWithUTF8String:argv[1]] autorelease];
		NSDictionary *userInfo = [NSDictionary dictionaryWithObject:identifier forKey:@"options"];

		CPDistributedMessagingCenter *messagingCenter = [CPDistributedMessagingCenter centerNamed:@"com.innoying.sbserver"];	
    	NSDictionary *status = [messagingCenter sendMessageAndReceiveReplyName:@"sbtoggle" userInfo:userInfo];


```


- [WiPi:Show a WiFi picker using various activation methods](https://github.com/ichitaso/WiPi/blob/master/Tweak.xm)


```

http://bensge.com/blog/


Cydia Substrate tweak for iOS 6, 7, 8, 9 and 10. Activates the system WiFi Picker with an Activator gesture, or by long-pressing the Control Center or FlipSwitch WiFi toggle. Automatically enables Wi-Fi if not enabled.


<!-- WiPi can be downloaded from the Cydia -->

http://cydia.saurik.com/package/com.bensge.wipi/



<!-- code -->


[[objc_getClass("SBUserAgent") sharedUserAgent] deviceIsLocked]


%hook WFWiFiAlertItem

if ([cell isKindOfClass:objc_getClass("WFWiFiCell")])

NSString *currentNetworkBSSID = (NSString *)[[objc_getClass("WFWiFiManager") sharedInstance] currentNetworkBSSID];





```


- [libtogglekit](https://github.com/ichitaso/libtogglekit/blob/48ddbffdc3a82d8bd7e77649bc4ec7c59734bdef/Tweak.xm)

```
<!-- SBTelephonyManager -->
- (void)toggleAirplaneMode
{
	if(![self airplaneModeEnabled]) {
		[[%c(SBTelephonyManager) sharedTelephonyManager] setIsInAirplaneMode:YES];
	}
	else {
		[[%c(SBTelephonyManager) sharedTelephonyManager] setIsInAirplaneMode:NO];
	}
}


<!--BluetoothManager  -->
- (void)toggleBluetooth
{
	if(![self bluetoothEnabled]) {
        [[%c(BluetoothManager) sharedInstance] setEnabled:YES];
        [[%c(BluetoothManager) sharedInstance] setPowered:YES];
	}
	else {
        [[%c(BluetoothManager) sharedInstance] setEnabled:NO];
        [[%c(BluetoothManager) sharedInstance] setPowered:NO];
	}
}


<!-- CLLocationManager -->


- (void)toggleLocationServices
{
	if(![self locationServicesEnabled]) {
		[%c(CLLocationManager) setLocationServicesEnabled:YES];
	}
	else {
		[%c(CLLocationManager) setLocationServicesEnabled:NO];
	}
}


<!-- SBOrientationLockManager -->
- (void)toggleRotation
{	
	if(![self rotationEnabled]) {
		[[%c(SBOrientationLockManager) sharedInstance] lock];
	}
	else {
		[[%c(SBOrientationLockManager) sharedInstance] unlock];
	}
}

```


- [OLWiFiToggle](https://github.com/coolstar/Olympus/blob/master/Classes/OLWiFiToggle.m)

```


	NSString *body = [NSString stringWithFormat:@"WiFi %@", (([[objc_getClass("SBWiFiManager") sharedInstance] wiFiEnabled]) ? @"enabled" : @"disabled")];

	Class bulletinBannerController = objc_getClass("SBBulletinBannerController");
	Class bulletinRequest = objc_getClass("BBBulletinRequest");
		
	BBBulletinRequest *request = [[[bulletinRequest alloc] init] autorelease];
	request.title = @"Olympus";
	request.message = body;
	request.sectionID = @"com.apple.Preferences";

	[(SBBulletinBannerController *)[bulletinBannerController sharedInstance] observer:nil addBulletin:request forFeed:2];

```



