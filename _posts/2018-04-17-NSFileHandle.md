---
layout: post
title: NSFileHandle
date: 2018-04-17
tag: ios
site: https://zhangkn.github.io
---

### 前言


>* NSFileManager 类主要对文件的操作（删除、修改、移动、复制等等）


>* NSFileHandle 类主要对文件内容进行读取和写入操作





### NSString stringWithContentsOfFile

>* 从txt 读取数据的例子

```

 NSString  *sandboxPath  = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory,NSUserDomainMask, YES) firstObject];
  NSString *realFile = [sandboxPath stringByAppendingPathComponent:@"keyword.txt"];
  NSLog(@"realFile %@",realFile);// realFile /var/mobile/Containers/Data/Application/4D54232E-8358-4E1C-9DDC-C00BC0CA3EC6/Documents/Keyword.txt


	// NSString *realFile = [[contentUserIDURL stringByAppendingPathComponent:@"Documents"] stringByAppendingPathComponent:@"real.txt"];
        // NSString *realFile = @"/var/mobile/Media/";
	NSString *content = [NSString stringWithContentsOfFile:realFile encoding:NSUTF8StringEncoding error:nil];
	NSString *contentUserID =content;

    NSArray *lines; /*将文件转化为一行一行的*/
	lines = [contentUserID componentsSeparatedByString:@"\n"];
	if (contentUserID) {

		NSLog(@"contentUserID is lines：%@",lines);

		NSLog(@"contentUserID is lines.count：%ld",lines.count);
		keylines = lines;

		if (kni > keylines.count )
		{
			/* code */
			return;
		}

		UIPasteboard* pasteboard = [UIPasteboard generalPasteboard];
		[pasteboard setString:keylines[kni]];

		kni++;

		[self startNexTask];

		// [self setuptxt];

	}else{
		NSLog(@"contentUserID is nil");
		return;
	}

```

### NSFileHandle


>* setupsavetxt,写数据的例子

```

// 写数据到
  NSString *contentUserID = tmp;
  // sandboxPath

  NSString  *sandboxPath  = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory,NSUserDomainMask, YES) firstObject];

  NSString *realFile = [sandboxPath stringByAppendingPathComponent:@"real.txt"];
  NSLog(@"realFile");
  if ([[NSFileManager defaultManager] fileExistsAtPath:realFile]) {
                        // NSString *deviceID = GetUniqueDeviceID();
    NSLog(@"fileExistsAtPath fileExistsAtPath");
    if (contentUserID) {
      NSLog(@"contentUserID contentUserID:%@",contentUserID);
                            //将字符串转成NSData类型     
      NSData *data = [contentUserID dataUsingEncoding: NSUTF8StringEncoding]; 

                            // [contentUserID writeToFile:realFile atomically:YES encoding:NSUTF8StringEncoding error:nil];
      // [data writeToFile:realFile atomically:YES];//覆盖

      NSFileHandle *fileHandle = [NSFileHandle fileHandleForUpdatingAtPath:realFile];///打开一个文件准备更新

    [fileHandle seekToEndOfFile]; // 可以操作光标到文件内容的末尾。
    [fileHandle writeData:data]; //追加写入数据////写入数据    
    [fileHandle closeFile];

    }
  }else{

    // [[NSFileManager defaultManager] createFileAtPath:realFile contents:[contentUserID dataUsingEncoding:NSUTF8StringEncoding] attributes:nil];

  	[contentUserID writeToFile:realFile atomically:YES encoding:NSUTF8StringEncoding error:nil];

  }

```


### see also

