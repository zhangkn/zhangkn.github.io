---
layout: post
title: SystemIsUnavailable
date: 2018-04-08
tag: iOSre
site: https://zhangkn.github.io
---


### 前言

>* 用peopen替换system

>* 使用posix_spawn替代system


### 正文

>* : 'system' is unavailable: not available on iOS

```

使用FILE	*popen(const char *, const char *) __DARWIN_ALIAS_STARTING(__MAC_10_6, __IPHONE_2_0, __DARWIN_ALIAS(popen)) __swift_unavailable_on("Use posix_spawn APIs or NSTask instead.", "Process spawning is unavailable.");

替代 int	 system(const char *) __DARWIN_ALIAS_C(system);


```


### 替代system的方案

>*  [例子： yalu102 使用Xcode9 进行编译：popen 替代 system](https://github.com/iosjb/KNyalu102/blob/master/yalu102/jailbreak.m)

```
//               system("killall -9 cfprefsd");
                
                popen("killall -9 cfprefsd","r");

```

>* [使用posix_spawn 替代system](https://github.com/iosjb/KNyalu102/blob/master/yalu102/system/utils.c)

```

int run(const char *cmd) {
    char *myenviron[] = {
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/bin/X11:/usr/games",
        "PS1=\\h:\\w \\u\\$ ",
        NULL
    };
    
    pid_t pid;
    char *rawCmd = fixedCmd(cmd);
    char *argv[] = {"sh", "-c", (char*)rawCmd, NULL};
    int status;
    status = posix_spawn(&pid, "/bin/sh", NULL, NULL, argv, (char **)&myenviron);
    if (status == 0) {
        if (waitpid(pid, &status, 0) == -1) {
            perror("waitpid");
        }
    } else {
        printf("posix_spawn: %s\n", strerror(status));
    }
    free(rawCmd);
    return status;
}

```




### see also
