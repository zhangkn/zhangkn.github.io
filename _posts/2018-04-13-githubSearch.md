---
layout: post
title: githubSearch
date: 2018-04-13
tag: Search
site: https://zhangkn.github.io
---


### 前言


分为两大部分：1. Search 、2.Trending、3.快捷键

#### 1. Search

>* [githubSearch 的高级搜索语法其实百度、谷歌类似](https://github.com/search?utf8=%E2%9C%93&q=+user%3Azhangkn+user%3Aantire+repo%3Azhangkn%2Fzhangkn.github.io+repo%3Aantire%2Fzhangkn.github.io+created%3A%3E2017-01-01+created%3A2017-01-02+stars%3A0..1+stars%3A1+stars%3A%3E1+forks%3A0..1+forks%3A1+forks%3A%3C10+size%3A1+pushed%3A%3C2018-09-09+extension%3Ajpg+extension%3Ahtml+size%3A1..1000+size%3A%3E1+path%3A%2F_site+language%3ACSS%09&type=Repositories)


```
user:zhangkn user:antire repo:zhangkn/zhangkn.github.io repo:antire/zhangkn.github.io created:>2017-01-01 created:2017-01-02 stars:0..1 stars:1 stars:>1 forks:0..1 forks:1 forks:<10 size:1 pushed:<2018-09-09 extension:jpg extension:html size:1..1000 size:>1 path:/_site language:CSS	

```

>* [search](https://github.com/search)

>* [advanced search page.](https://github.com/search/advanced)

>* [searching-code](https://help.github.com/articles/searching-code)

>* [searching-in-forks](https://help.github.com/articles/searching-in-forks)


#### 2.Trending

>* [trending](https://github.com/trending)

#### 3.快捷键

>* [shift + / ]

>* [t]


### 1. Search

>* [Repository search ](https://help.github.com/articles/searching-repositories)

```
<!-- cat stars:>100	 -->
Find cat repositories with greater than 100 stars.
<!-- user:defunkt -->
	Get all repositories from the user defunkt.
<!-- pugs pushed:>2013-01-28	 -->

Pugs repositories pushed to since Jan 28, 2013.

<!-- node.js forks:<200	 -->
Find all node.js repositories with less than 200 forks.
<!-- jquery size:1024..4089	 -->
Find jquery repositories between the sizes 1024 and 4089 kB.
<!-- gitx fork:true -->
	Repository search includes forks of gitx.
<!-- gitx fork:only -->
	Repository search returns only forks of gitx.

```

>* [Basic search]

```


<!--  cat stars:>100: -->

	Find cat repositories with greater than 100 stars.

<!--  user:defunkt	: -->

Get all repositories from the user defunkt.

<!--  tom location:"San Francisco, CA"	: -->

Find all tom users in "San Francisco, CA".

<!--  join extension:coffee -->

	Find all instances of join in code with coffee extension.


<!--  NOT cat	 -->

Excludes all results containing cat
```


>* [Advanced search](https://github.com/search?utf8=%E2%9C%93&q=user%3Azhangkn+user%3Aantire+repo%3Azhangkn%2Fzhangkn.github.io+repo%3Aantire%2Fzhangkn.github.io+created%3A%3E2017-01-01+created%3A2017-01-02+stars%3A0..1+stars%3A1+stars%3A%3E1+forks%3A0..1+forks%3A1+forks%3A%3C10+size%3A1+pushed%3A%3C2018-09-09+extension%3Ajpg+extension%3Ahtml+size%3A1..1000+size%3A%3E1+path%3A%2F_site+comments%3A0..1+comments%3A%3E0+label%3Abug+author%3Azhangkn+mentions%3Azhangkn+assignee%3Azhangkn+updated%3A%3C2018-09-09+fullname%3Azhangkn+location%3ACA+followers%3A0..1000+followers%3A%3E1+followers%3A%3C10000+repos%3A0+repos%3A%3C90+repos%3A%3E5+updated%3A%3C2018-09-09+language%3ACSS+language%3ACSS&type=Repositories&ref=advsearch&l=CSS&l=CSS)

```

user:zhangkn user:antire repo:zhangkn/zhangkn.github.io repo:antire/zhangkn.github.io created:>2017-01-01 created:2017-01-02 stars:0..1 stars:1 stars:>1 forks:0..1 forks:1 forks:<10 size:1 pushed:<2018-09-09 extension:jpg extension:html size:1..1000 size:>1 path:/_site comments:0..1 comments:>0 label:bug author:zhangkn mentions:zhangkn assignee:zhangkn updated:<2018-09-09 fullname:zhangkn location:CA followers:0..1000 followers:>1 followers:<10000 repos:0 repos:<90 repos:>5 updated:<2018-09-09 language:CSS language:CSS


```


>* [searching-code](https://help.github.com/articles/searching-code)

```
<!-- install repo:charles/privaterepo -->

	Find all instances of install in code from the repository charles/privaterepo.
<!-- shogun user:heroku	 -->

Find references to shogun from all public heroku repositories.
<!-- join extension:coffee	 -->
Find all instances of join in code with coffee extension.
<!-- system size:>1000	 -->
Find all instances of system in code of file size greater than 1000kbs.
<!-- examples path:/docs/ -->
	Find all examples in the path /docs/.
<!-- replace fork:true -->
	Search replace in the source code of forks.
```

- [Issue search ](https://help.github.com/articles/searching-issues)

```
<!-- encoding user:heroku -->
	Encoding issues across the Heroku organization.
<!-- cat is:open	 -->
Find cat issues that are open.
<!-- strange comments:>42	 -->

Issues with more than 42 comments.

<!-- hard label:bug -->
	Hard issues labeled as a bug.

<!-- author:mojombo -->
	All issues authored by mojombo.
<!-- mentions:tpope	 -->
All issues mentioning tpope.
<!-- assignee:rtomayko	 -->
All issues assigned to rtomayko.
<!-- exception created:>2012-12-31 -->
	Created since the beginning of 2013.
<!-- exception updated:<2013-01-01' -->
	Last updated before 2013.
```


>* [searching-users](https://help.github.com/articles/searching-users)

```
<!-- fullname:"Linus Torvalds"	 -->
Find users with the full name "Linus Torvalds".
<!-- tom location:"San Francisco, CA" -->
	Find all tom users in "San Francisco, CA".
<!-- chris followers:100..200	 -->
Find all chris users with followers between 100 and 200.
<!-- ryan repos:>10	 -->
Find all ryan users with more than 10 repositories.
```


### 2.Trending

>* [trending](https://github.com/trending)


### 3、快捷键

	

>* [github上按“shift + /”]

```

支持的快捷键：Site wide shortcuts、Repositories、Source code browsing

<!-- Site wide shortcuts -->

s or /	Focus search bar
g n	Go to Notifications
g d	Go to Dashboard
?	Bring up this help dialog
j	Move selection down
k	Move selection up
x	Toggle selection
o or enter	Open selection
h	Open hovercard
<!-- Repositories -->
g c	Go to Code
g i	Go to Issues
g p	Go to Pull Requests
g b	Go to Projects
g w	Go to Wiki
<!-- Source code browsing -->
t	Activates the file finder
l	Jump to line
w	Switch branch/tag
y	Expand URL to its canonical form
i	Show/hide all inline notes

```

>* [在特定的页面（Repositories），搜索文件按"t"，可以快速搜索](https://github.com/zhangkn/zhangkn.github.io/find/master)

```
You’ve activated the file finder. Start typing to filter the file list.
 1) Use ↑ and ↓ to navigate,
 2)enter to view files,
 3)esc to exit.

```


### see also 

>* [advanced search page](https://github.com/search/advanced)

```

Advanced search

<!-- 例子 -->


user:zhangkn user:antire repo:zhangkn/zhangkn.github.io repo:antire/zhangkn.github.io created:>2017-01-01 created:2017-01-02 stars:0..1 stars:1 stars:>1 forks:0..1 forks:1 forks:<10 size:1 pushed:<2018-09-09 extension:jpg extension:html size:1..1000 size:>1 path:/_site language:CSS	




<!-- Advanced options -->

From these owners :zhangkn,antire


In these repositories: zhangkn/zhangkn.github.io,antire/zhangkn.github.io


Created on the dates: >YYYY-MM-DD, YYYY-MM-DD


Written in this language:css


<!--  Repositories options -->

With this many stars: 0..100, 200, >1000


With this many forks: 0..1,1,<10

Of this size: Repository size in KB


Pushed to: <YYYY-MM-DD


With this license: 

<!--  Code options -->


With this extension : rb,py,gpg

Of this file size : 100..80000000 ,>10000

In this path: /user/zhangkn/github/io

<!-- Issues options -->

In the state: open/closed

With this many comments

 
<!-- Issues options -->

In the state

With this many comments: 0..100, >442

With the labels : bug, ie6

Opened by the author:hubot, octocat

Mentioning the users:tpope, mattt

Assigned to the users: twp, jim

Updated before the date: <YYYY-MM-DD




<!-- Users options -->

With this full name: Grace Hopper

From this location:San Francisco, CA

With this many followers: 20..50, >200, <2

With this many public repositories:0, <42, >5

Working in this language:C 


<!-- Wiki options -->

Updated before the date: <YYYY-MM-DD
```



- [about-searching-on-github/](https://help.github.com/articles/about-searching-on-github/)

```

<!-- types -->

Repositories

Topics

Issues

Code

Commits

Users

Wikis

```

- [GoogleHacking](https://zhangkn.github.io/2018/02/GoogleHacking/)

```
filetype:doc site:(https://zhangkn.github.io) inurl: (iOS "lldb" (hopper) -(   --- 百度也有对应的高级搜索
```

- [Github 搜索技巧](https://www.jianshu.com/p/7321caea2a08)

```
<!-- 查看一门语言的 Repository 排行榜（ -->

>*  language:Objective-C stars:>0      

zhangkn  OR zhangkn.github.io

```

>* [删除QuickLook缓存文件](http://ifatima.lofter.com/post/151688_784eed)

```
四：删除QuickLook缓存文件

<!-- sudo rm -rf /private/var/folders/ -->

<!-- /private/var/folders/8s/t119mw8d4lsdztx8h9q8113m0000gn/c/com.apple.DeveloperTools/All/Xcode/EmbeddedAppDeltas -->

153G  Users
 27G  Applications
 10G  private
6.4G  usr



删除就可以。这个文件夹内的文件是你在真机测试时安装程序的详情。 


devzkndeMacBook-Pro:Users devzkn$ sudo du -sh * |sort -h

<!-- /Users/devzkn/Pictures/照片图库.photoslibrary -->

<!-- /Users/devzkn/Library/Safari -->



<!-- tail获取iOS模拟器日志 -->

日志文件的路径:/Users/$UserName/Library/Logs/CoreSimulator/$simulator_hash/system.log



simulator_hash：模拟器的identifier


/Users/devzkn/Library/Containers/com.tencent.qq/Data/Library/Caches/Images
/Users/devzkn/Library/Containers/com.tencent.xinWeChat/Data/Library/Application Support/com.tencent.xinWeChat/2.0b4.0.9/168382edfbf20fc8e6340f8590a006c0/Message/MessageTemp

<!-- https://apple.stackexchange.com/questions/44141/where-does-garageband-put-its-downloaded-instruments-and-loop-libraries -->

The loops for GarageBand are stored in ~/Library/Audio/Apple Loops/, and my installation is 629.9MB. However I do not believe that I have downloaded the whole set.

At an Apple retailer where I have worked, their installation in

~/Library/Application Support/GarageBand is 14.57GB,
and their installation in

~/Library/Audio/Apple Loops is 32GB.


<!-- https://www.tekrevue.com/tip/delete-garageband/ -->

macintosh hd/applications/garageband.app (1.16gb)
macintosh hd/library/application support/garageband (995mb)
macintosh hd/library/application support/logic (880mb)
macintosh hd/library/audio/apple loops (up to 10gb)*

/library/application support/logic/


649M	Logic
843M	GarageBand

<!-- /Users/devzkn/Library/Developer/CoreSimulator/Devices -->

Try to run xcrun simctl delete unavailable in your terminal.

can use the simctl tool (comes with Xcode 6+) to list all of the actual simulator devices you have, along with the ids so that you can figure out what folders to delete.

xcrun simctl list devices

== Devices ==
-- iOS 11.1 --
    iPhone 5s (6A5C0A08-F5EC-40C0-B98F-708B5803F8E6) (Shutdown)
    iPhone 6 (1C3A19F2-790F-486F-B4C6-A8B83D687239) (Shutdown)
    iPhone 6 Plus (E46FFA5C-B3ED-4DD7-B900-741E189EB78D) (Shutdown)
    iPhone 6s (57811378-5D20-4640-885E-C067056D4D6E) (Shutdown)
    iPhone 6s Plus (91612885-F49D-4BE1-ADDA-C3B45FFE4DFB) (Shutdown)
    iPhone 7 (E561318B-E59B-4B50-8708-1CB44FF1D8E6) (Shutdown)
    iPhone 7 Plus (E2A96E26-F7E0-44F1-B7CF-7BB1BA1D9AA7) (Shutdown)
    iPhone 8 (13E866D9-9AE3-44C3-8BC9-453E7453E065) (Shutdown)
    iPhone 8 Plus (7638500D-A75E-4AB3-83BF-04CD819AE961) (Shutdown)
    iPhone SE (035651F6-014A-45E9-B2F6-878DB7A722C2) (Shutdown)
    iPhone X (CA8B361E-C25A-484B-8EB9-288753B2BB92) (Shutdown)
    iPad Air (C4400BC1-2B38-476E-B644-3BC471A6A1C8) (Shutdown)
    iPad Air 2 (A7695764-A0FF-4021-B932-714DF3D335C5) (Shutdown)
    iPad (5th generation) (A7BAA0F6-893B-4325-8035-486E20A1F3DD) (Shutdown)
    iPad Pro (9.7-inch) (58580719-AE8D-4423-8760-50DA606F4F5C) (Shutdown)
    iPad Pro (12.9-inch) (9E2948ED-ABD3-4000-8A98-C3E3E06FE0A4) (Shutdown)
    iPad Pro (12.9-inch) (2nd generation) (92100EF4-1744-4A99-B4BF-BAB23517B5CE) (Shutdown)
    iPad Pro (10.5-inch) (07183112-F213-44EE-86CB-8AC09543EC9C) (Shutdown)
-- tvOS 11.1 --
    Apple TV (FC9866EE-9CC9-4C19-A2AE-688391F0178F) (Shutdown)
    Apple TV 4K (F3EAC680-CE06-4AFE-8DC6-CE83ADC3B750) (Shutdown)
    Apple TV 4K (at 1080p) (E04AF63B-CA48-4FC5-A4ED-C6841B496A8A) (Shutdown)
-- watchOS 4.1 --
    Apple Watch - 38mm (AD443BF5-6884-470F-B962-E89B9C1F1267) (Shutdown)
    Apple Watch - 42mm (142E49C5-68AD-4543-AD00-DB6EBE6AF172) (Shutdown)
    Apple Watch Series 2 - 38mm (9DCB54A3-E3F2-4259-8D2B-5E755C6C740D) (Shutdown)
    Apple Watch Series 2 - 42mm (23A8224B-58E3-4621-99E0-C1282DD02170) (Shutdown)
    Apple Watch Series 3 - 38mm (AF3AC564-D350-4BD8-B0AB-4BA3DE7A3B16) (Shutdown)
    Apple Watch Series 3 - 42mm (6DE994DA-C442-44C6-B2B7-FF9DFAB218D3) (Shutdown)
-- Unavailable: com.apple.CoreSimulator.SimRuntime.iOS-10-3 --
    iPhone 5 (E0A84296-A6F0-47BC-BB3F-198D7350DBF4) (Shutdown) (unavailable, runtime profile not found)
    iPhone 5s (ABCA4986-229C-4149-898D-E12681F81489) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6 (2499648A-9622-42D9-8931-C342DB0208CC) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6 Plus (02FE1D98-311A-4121-90DD-112E6ACD00AE) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6s (C7BBB881-D268-41C9-933A-DFE2CB1690F3) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6s Plus (D9C47E65-61E9-48AA-A5D4-123FFAD320CF) (Shutdown) (unavailable, runtime profile not found)
    iPhone 7 (99DD08F2-0ABD-41B3-A3D6-DEF89FEC5A86) (Shutdown) (unavailable, runtime profile not found)
    iPhone 7 Plus (03FD7AC9-0B1A-41C1-8790-9CF914D33B68) (Shutdown) (unavailable, runtime profile not found)
    iPhone SE (8E605B0A-5C3D-46C9-8F47-D2F46413FE93) (Shutdown) (unavailable, runtime profile not found)
    iPad Air (9F4F828B-E8BF-415E-A403-C89C4523C9CF) (Shutdown) (unavailable, runtime profile not found)
    iPad Air 2 (77CD3990-757C-4EC8-8EC8-CD6641642A19) (Shutdown) (unavailable, runtime profile not found)
    iPad (5th generation) (CF06258E-60E4-4112-AE6E-504467B2995B) (Shutdown) (unavailable, runtime profile not found)
    iPad Pro (9.7 inch) (B3E52D35-01E6-421F-A1D7-969EDA80D244) (Shutdown) (unavailable, runtime profile not found)
    iPad Pro (12.9 inch) (5463C0EF-EC36-413D-9AAA-AA7289BE6327) (Shutdown) (unavailable, runtime profile not found)
    iPad Pro (12.9-inch) (2nd generation) (C9B39AF7-D0A0-45A0-ADC8-841280CE9975) (Shutdown) (unavailable, runtime profile not found)
    iPad Pro (10.5-inch) (00A33D8E-9A8D-41FF-8727-DA282A778C3B) (Shutdown) (unavailable, runtime profile not found)
-- Unavailable: com.apple.CoreSimulator.SimRuntime.iOS-9-2 --
    iPhone 4s (D777F1BE-5349-4A6A-8E56-0B2EE127D5A7) (Shutdown) (unavailable, runtime profile not found)
    iPhone 5 (CD749BB5-604B-4F5B-842F-58B955B03BCC) (Shutdown) (unavailable, runtime profile not found)
    iPhone 5s (6BD7375F-E847-43FA-921F-EEA5E0D5E664) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6 (BAE2759A-94D7-4993-8873-14AEB7B43B1B) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6 Plus (33CF73B6-B30C-4E62-94F6-7C66014ECF53) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6s (4CDA8F10-AD17-479C-A5EF-1BF4D0D90864) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6s Plus (F98ECCDC-B0C7-4236-9B80-350340F9B0FC) (Shutdown) (unavailable, runtime profile not found)
    iPad 2 (C6718F23-DE60-4AB9-88D9-5EE733D497CC) (Shutdown) (unavailable, runtime profile not found)
    iPad Retina (498249BD-FACB-465E-9C2A-1A233DFA0E92) (Shutdown) (unavailable, runtime profile not found)
    iPad Air (8FDDB01B-91D7-479C-A79A-59C60CCE326A) (Shutdown) (unavailable, runtime profile not found)
    iPad Air 2 (17C45F34-D377-4A32-8DF9-54B4E7A384D8) (Shutdown) (unavailable, runtime profile not found)
    iPad Pro (EBE9B7AF-5730-48C2-8994-CA89C38CA827) (Shutdown) (unavailable, runtime profile not found)
-- Unavailable: com.apple.CoreSimulator.SimRuntime.iOS-9-3 --
    iPhone 4s (107AFBA3-D232-4B71-BF7B-DE6B9AF36615) (Shutdown) (unavailable, runtime profile not found)
    iPhone 5 (6ECA6D7D-2C76-4896-81FA-A2A8787D6AFF) (Shutdown) (unavailable, runtime profile not found)
    iPhone 5s (6D2F470D-E7D1-4B0D-8E85-A481C9B37706) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6 (2A9F4ADF-BF7D-49FB-B4D3-5B8896B35027) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6 Plus (EB5481B3-4367-4B65-9804-D19BA7798F19) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6s (2B3278F2-6097-497E-93A5-F23B74ABD83F) (Shutdown) (unavailable, runtime profile not found)
    iPhone 6s Plus (23AFEE14-2F20-4A7F-A765-075ABCBF065A) (Shutdown) (unavailable, runtime profile not found)
    iPad 2 (625AD3CF-ABA7-44BE-A2CF-C505D329B2ED) (Shutdown) (unavailable, runtime profile not found)
    iPad Retina (8AF972F7-0362-4BE0-A245-370E81696E5D) (Shutdown) (unavailable, runtime profile not found)
    iPad Air (A4C542FD-8587-4EDC-91C4-5A95EF80B365) (Shutdown) (unavailable, runtime profile not found)
    iPad Air 2 (3137E899-ACDB-4FA1-A0BA-432CA983CF03) (Shutdown) (unavailable, runtime profile not found)
    iPad Pro (4BF4B214-1283-4A27-B4B7-4712AF71A6F3) (Shutdown) (unavailable, runtime profile not found)
-- Unavailable: com.apple.CoreSimulator.SimRuntime.tvOS-10-2 --
    Apple TV 1080p (95572697-E738-444C-A9EE-8C9ADA1FD39D) (Shutdown) (unavailable, runtime profile not found)
-- Unavailable: com.apple.CoreSimulator.SimRuntime.tvOS-9-1 --
    Apple TV 1080p (7F65F5AB-93D1-4EC9-A3FB-DDCC3FEF5D07) (Shutdown) (unavailable, runtime profile not found)
-- Unavailable: com.apple.CoreSimulator.SimRuntime.tvOS-9-2 --
    Apple TV 1080p (F252C2B2-55B3-4DEF-956B-5750DE17FE5B) (Shutdown) (unavailable, runtime profile not found)
-- Unavailable: com.apple.CoreSimulator.SimRuntime.watchOS-2-1 --
    Apple Watch - 38mm (545804A7-803D-4714-A914-3111785F36F4) (Shutdown) (unavailable, runtime profile not found)
    Apple Watch - 42mm (75175B70-A1D3-42B9-B976-C3C70AED9E9B) (Shutdown) (unavailable, runtime profile not found)
-- Unavailable: com.apple.CoreSimulator.SimRuntime.watchOS-2-2 --
    Apple Watch - 38mm (1AD6D822-3A52-49C9-B223-48973D022812) (Shutdown) (unavailable, runtime profile not found)
    Apple Watch - 42mm (59C3C345-4F01-4B06-98CC-9323D4BC8FC6) (Shutdown) (unavailable, runtime profile not found)
-- Unavailable: com.apple.CoreSimulator.SimRuntime.watchOS-3-2 --
    Apple Watch - 38mm (A523E96F-5F27-43DA-8ECF-B6040B79B494) (Shutdown) (unavailable, runtime profile not found)
    Apple Watch - 42mm (339C470A-EF45-4E94-B5FC-ED6D12FF4DBC) (Shutdown) (unavailable, runtime profile not found)
    Apple Watch Series 2 - 38mm (5DCD5B99-D787-4EB1-AA64-E87C97CEB4C3) (Shutdown) (unavailable, runtime profile not found)
    Apple Watch Series 2 - 42mm (C827CECB-0550-49ED-A895-A3DF8CDD6306) (Shutdown) (unavailable, runtime profile not found)


<!-- xcrun simctl help delete -->
So I can either delete the individual device(s):

xcrun simctl delete D24C18BC-268C-4F0B-9CD8-8EFFDE6619E3

or I can bulk delete all of the unavailable ones with:

xcrun simctl delete unavailable


xcrun simctl help


<!-- /Users/devzkn/Library/Developer/Xcode -->

 39M	Products
126M	DocumentationCache
191M	UserData
414M	iOS Device Logs
823M	Archives
3.0G	DerivedData
 62G	iOS DeviceSupport





```
