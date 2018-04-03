---
layout: post
title: ios-run-application-with-root-privileges
date: 2018-03-29
tag: iOSre
site: https://zhangkn.github.io
---

### 前言


- [code](https://github.com/zhangkn/ios-run-application-with-root-privileges)



### 正文



>* Put your .app bundle in the ./Applications folder of your debian package.

>* Set ownership of your app binary to "root:wheel":

```

after-install::
	# Taokeceshiji1:~ root# uicache
	install.exec "killall \"KNcreateOnDisk\"" || true
	install.exec "uicache "


#Finally, make sure to set the ownership of the script to "root:wheel" and its permissions to "755".

	install.exec "chmod 775 /Applications/KNcreateOnDisk.app/fake"

#要先设置用户属主,再设置文件执行权限
	install.exec "chown root:wheel /Applications/KNcreateOnDisk.app/KNcreateOnDisk"

	#Give your app binary the "set user ID" and "set group ID" flags.
	install.exec "chmod 6775 /Applications/KNcreateOnDisk.app/KNcreateOnDisk"

```

>* Explicitly call "setuid(0)" and "setgid(0)" very early in your code (specifically in "main", somewhere before calling "UIApplicationMain"). Here's a quick example:

```

#import "KNAppDelegate.h"

int main(int argc, char *argv[]) {
	@autoreleasepool {



		        // Set uid and gid
        if (!(setuid(0) == 0 && setgid(0) == 0))
        {
            NSLog(@"Failed to gain root privileges, aborting...");
            exit(EXIT_FAILURE);
        }

        // Launch app

		return UIApplicationMain(argc, argv, nil, NSStringFromClass(KNAppDelegate.class));
	}
}
```



>* add a "launch script" fake to the app bundle, which will be called when you tap on the SpringBoard icon, and will actually launch the app.

```

#!/bin/bash
root=$(dirname "$0")
exec "${root}"/KNcreateOnDisk


<!-- Finally, make sure to set the ownership of the script to "root:wheel" and its permissions to "755".
 -->

可以放在deb 包的postinst ,或者makefile

```












### fake

>* devzkndeMacBook-Pro:Resources devzkn$ touch fake

```

cd /Users/devzkn/code/github/kncreateondisk/Resources

devzkndeMacBook-Pro:Resources devzkn$ chmod +x fake

devzkndeMacBook-Pro:Resources devzkn$ cat fake
#!/bin/bash
root=$(dirname "$0")
exec "${root}"/KNcreateOnDisk

<!-- Taokeceshiji1:/Applications/KNcreateOnDisk.app root# ls -lrt -->

-rwxrwxr-x 1 root   wheel     63 Mar 29 17:40 fake
-rw-r--r-- 1 mobile staff   4313 Mar 29 17:57 Info.plist
-rwsrwsr-x 1 root   wheel 138688 Mar 29 18:00 KNcreateOnDisk

```

>* Info.plist

```
<!-- CFBundleExecutable -->

	<key>CFBundleExecutable</key>
	<string>$(EXECUTABLE_NAME)</string>

修改为 

	<key>CFBundleExecutable</key>
	<string>fake</string>

```



### see also

- [devzkndeMacBook-Pro:github devzkn$ git clone git://git.saurik.com/uikittools.git](https://git.saurik.com/uikittools.git)

```
<!-- https://git.saurik.com/uikittools.git/blob/HEAD:/uicache.mm -->



```

- [ios-run-application-with-root-privileges](http://blog.ib-soft.net/2013/01/ios-run-application-with-root-privileges.html)

```
Its bundle must be located in "/Applications".

Its executable must belong to the root user.

Its executable must be able to set user and group IDs.

It has to explicitly set user and group ID at runtime.

It must not be run directly. Rather, something else should launch it (keep reading to learn how to do this).

```