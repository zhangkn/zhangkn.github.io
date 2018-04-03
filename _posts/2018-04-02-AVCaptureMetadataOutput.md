---
layout: post
title: AVCaptureMetadataOutput
date: 2018-04-02
tag: iOSre
site: https://zhangkn.github.io
---


### 前言


>* [本文主要研究GoogleMobileVision](https://cocoapods.org/pods/GoogleMobileVision)


>* [Creating the Face Detector Pipeline](https://developers.google.com/vision/ios/face-tracker-tutorial)

```

GoogleMVDataOutput contains multiple instances of AVCaptureDataOutput that extend AVCaptureVideoDataOutput to allow you to integrate face tracking with your AVFoundation video pipeline.

```



### 正文


>* [https://developers.google.com/vision/ios/getting-started](https://developers.google.com/vision/ios/getting-started)

```
<!-- Try the sample apps -->

pod try GoogleMobileVision

1) FaceDetectorDemo: This demo demonstrates basic face detection and integration with AVFoundation. The app highlights face, eyes, nose, mouth, cheeks, and ears within detected faces.

2) GooglyEyesDemo: This demo demonstrates how to use GoogleMVDataOutput pod to simplify integration with video pipelines. The app draws cartoon eyes on top of detected faces.

3) MultiDetectorDemo: This demo demonstrates how to use GoogleMVDataOutput pod to run multiple detection types at once. The app draws red rectangles on detected faces and blue rectangles on detected barcodes.


```


>* [To add Mobile Vision API to your existing iOS App](https://developers.google.com/vision/ios/reference/)

```


<!-- add the FaceDetector CocoaPod to your project -->

Add pod 'GoogleMobileVision/FaceDetector' to your Podfile.


pod update



```


>* [Next Steps](https://developers.google.com/vision/face-detection-concepts)

```
<!-- https://developers.google.com/vision/face-detection-concepts -->
Face Detection Concepts Overview

<!-- https://developers.google.com/vision/ios/detect-faces-tutorial -->
detection guides on iOS


```


### Text Recognition API Overview





### Barcode



>* [Track Faces and Barcodes](https://developers.google.com/vision/ios/multi-tracker-tutorial)

```
<!-- MultiDetectorDemo -->

1) Create barcode and face detectors.

2) Track multiple faces and barcodes using a GMVMultiDetectorDataOutput.


3) Create separate graphics for barcodes and faces.







```





### faces


>* [Detect Facial Features in Photos](https://developers.google.com/vision/ios/detect-faces-tutorial)

```
https://developers.google.com/vision/ios/getting-started

<!-- Creating the face detector -->

@import GoogleMobileVision;




```

>* [Add Face Tracking with GoogleMVDataOutput To Your App](https://developers.google.com/vision/ios/face-tracker-tutorial)

```
 <!-- use the Face API with GoogleMVDataOutput with an AVFoundation pipeline to detect eye coordinates within faces in a camera feed.  -->


 GooglyEyesDemo

<!-- This tutorial will show you how to: -->

1) Specify custom camera settings.

2) Track multiple faces.

3) Make performance / feature trade-offs.







```


>* [Release Notes](https://developers.google.com/vision/ios/release-notes)

```
<!-- GoogleMobileVision 1.1.0 -->

Added Barcode Scanner API: Detects 1D and 2D barcodes.

Added 2 sample applications for Barcode Scanner API (GitHub). https://github.com/googlesamples/ios-vision




```




### [GoogleMobileVision](https://developers.google.com/vision)

  "description": "The Mobile vision iOS API provides libraries to locate objects in photos and video. The API is intended to operate offline on the device and doesn't require network connectivity.",



>* [docsets](http://cocoadocs.org/docsets/GoogleMobileVision/1.1.0/)


```
These samples demonstrate the vision API for detecting faces and data output.




```



>* [Stack Overflow](https://stackoverflow.com/questions/tagged/google-ios-vision)


>* [GoogleMobileVision.podspec.json](https://github.com/CocoaPods/Specs/blob/master/Specs/0/4/2/GoogleMobileVision/1.1.0/GoogleMobileVision.podspec.json)

```
Find objects in photos and video, using real-time on-device vision technology.


  "platforms": {
    "ios": "8.0"
  },


   "source": {
    "http": "https://dl.google.com/dl/cpdc/df83c97cbca53eaf/GoogleMobileVision-1.1.0.tar.gz"
  },

    "subspecs": [
    {
      "dependencies": {
        "GoogleMobileVision/Detector": [

        ]
      },
      "frameworks": [
        "Foundation"
      ],
      "name": "MVDataOutput",
      "vendored_frameworks": [
        "MVDataOutput/Frameworks/frameworks/GoogleMVDataOutput.framework"
      ]
    },
    {
      "dependencies": {
        "GoogleInterchangeUtilities": "~> 1.2",
        "GoogleNetworkingUtilities": "~> 1.2",
        "GoogleUtilities": "~> 1.3"
      },
      "frameworks": [
        "Foundation",
        "UIKit",
        "CoreMedia",
        "CoreVideo",
        "CoreGraphics",
        "Accelerate",
        "AVFoundation"
      ],
      "name": "Detector",
      "vendored_frameworks": [
        "Detector/Frameworks/frameworks/GoogleMobileVision.framework"
      ]
    },
    {
      "dependencies": {
        "GoogleMobileVision/Detector": [

        ]
      },
      "frameworks": [
        "Foundation",
        "Accelerate"
      ],
      "libraries": [
        "c++"
      ],
      "name": "FaceDetector",
      "resources": [
        "FaceDetector/Resources/BCLjoy_100.emd",
        "FaceDetector/Resources/BCLlefteyeclosed_200.emd",
        "FaceDetector/Resources/BCLrighteyeclosed_200.emd",
        "FaceDetector/Resources/LMprec_600.emd",
        "FaceDetector/Resources/MFTprec_202.emd",
        "FaceDetector/Resources/PFFprec_702.emd"
      ],
      "vendored_frameworks": [
        "FaceDetector/Frameworks/frameworks/FaceDetector.framework"
      ]
    },
    {
      "dependencies": {
        "GoogleMobileVision/Detector": [

        ]
      },
      "frameworks": [
        "Foundation",
        "Accelerate"
      ],
      "libraries": [
        "c++"
      ],
      "name": "BarcodeDetector",
      "vendored_frameworks": [
        "BarcodeDetector/Frameworks/frameworks/BarcodeDetector.framework"
      ]
    }
  ]
}


```

>* [GoogleMobileVision-1.1.0](https://dl.google.com/dl/cpdc/df83c97cbca53eaf/GoogleMobileVision-1.1.0.tar.gz)

```
https://github.com/GPUImage/GoogleMobileVision-1.1.0

```



### FaceDetectorDemo


>* Podfile

```
target "FaceDetector" do
  pod 'GoogleMobileVision/FaceDetector'
end

```

>* pod update

```

Analyzing dependencies
Downloading dependencies
Installing GoogleInterchangeUtilities (1.2.2)
Installing GoogleMobileVision (1.1.0)
Installing GoogleNetworkingUtilities (1.2.2)
Installing GoogleSymbolUtilities (1.1.2)
Installing GoogleUtilities (1.3.2)
Generating Pods project
Integrating client project

[!] Please close any current Xcode sessions and use `FaceDetector.xcworkspace` for this project from now on.
Sending stats
Pod installation complete! There is 1 dependency from the Podfile and 5 total pods installed.


<!-- FaceDetectorDemo/Pods/GoogleMobileVision/FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector -->

remote: error: File FaceDetectorDemo/Pods/GoogleMobileVision/FaceDetector/Frameworks/frameworks/FaceDetector.framework/FaceDetector is 106.49 MB; this exceeds GitHub's file size limit of 100.00 MB


MFTprec_202.emd 

devzkndeMacBook-Pro:copy devzkn$ ls -lrt /Users/devzkn/code/GPU/GoogleMobileVision-1.1.0/copy/FaceDetectorDemo/Pods/GoogleMobileVision/FaceDetector/Resources 
total 2936
-r-xr-xr-x  1 devzkn  staff     8630 Apr  3 08:39 BCLlefteyeclosed_200.emd
-r-xr-xr-x  1 devzkn  staff    15106 Apr  3 08:39 BCLjoy_100.emd
-r-xr-xr-x  1 devzkn  staff   189154 Apr  3 08:39 MFTprec_202.emd
-r-xr-xr-x  1 devzkn  staff   185703 Apr  3 08:39 PFFprec_702.emd
-r-xr-xr-x  1 devzkn  staff  1078354 Apr  3 08:39 LMprec_600.emd
-r-xr-xr-x  1 devzkn  staff     8655 Apr  3 08:39 BCLrighteyeclosed_200.emd



<!-- devzkndeMacBook-Pro:FaceDetectorDemo devzkn$ cat .gitignore -->
/Pods/


https://github.com/GPUImage/FaceDetectorDemo
```


### see also

- [#import <IJKMediaFramework/IJKMediaFramework.h>](https://github.com/Bilibili/ijkplayer.git)


```
devzkndeMacBook-Pro:github devzkn$ git clone https://github.com/Bilibili/ijkplayer.git ijkplayer-ios

cd ijkplayer-ios

./init-ios.sh

cd ios
./compile-ffmpeg.sh clean
./compile-ffmpeg.sh all


<!-- 6:在项目中使用的时候还需要导入依赖库。 -->

#         AVFoundation.framework
#         CoreGraphics.framework
#         CoreMedia.framework
#         CoreVideo.framework
#         libbz2.tbd
#         libz.tbd
#         MediaPlayer.framework
#         MobileCoreServices.framework
#         OpenGLES.framework
#         QuartzCore.framework
#         UIKit.framework
#         VideoToolbox.framework



<!-- 直播视频拉流: IJKMediaFramework 已编译好, 无需繁琐操作, 拖进项目add相关依赖库就可以使用! -->

https://github.com/xiaohange/IJKMediaFramework

https://download.csdn.net/download/qq_31810357/9911107


<!-- devzkndeMacBook-Pro:ijkplayer-ios devzkn$ /Users/devzkn/code/github/ijkplayer-ios/ios/IJKMediaPlayer  -->


<!-- pod 'ijkplayer', '~> 0.3.2-rc.2' -->

pod "IJKMediaFramework"




```

- [latex3](https://github.com/latex3/latex2e)

```

<!-- https://www.latex-project.org/get/ -->

git clone  https://github.com/latex3/latex2e.git

LaTeX，文字形式写作LaTeX，是一种基于TeX的排版系统，由美国计算机科学家莱斯利·兰伯特在20世纪80年代初期开发，利用这种格式系统的处理，即使用户没有排版和程序设计的知识也可以充分发挥由TeX所提供的强大功能，不必一一亲自去设计或校对，能在几天，甚至几小时内生成很多具有书籍质量的印刷品

<!-- http://www.tug.org/mactex/ -->




```

- [DeviceSupport]（https://download.csdn.net/download/u011018979/10324073）


```
<!-- 补充： 支持iOSV6.0-11.3 https://download.csdn.net/download/u011018979/10324073 -->

devzkndeMacBook-Pro:Platforms devzkn$ /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport 

devzkndeMacBook-Pro:Platforms devzkn$ open  /Applications/Xcode8.3.3.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport


devzkndeMacBook-Pro:Platforms devzkn$ cp -r /Users/devzkn/Downloads/11.3\ \(15E217\)  /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport 


```

- [【如何快速的开发一个完整的iOS直播app】(原理篇)](https://www.jianshu.com/p/bd42bacbe4cc)

```
 <!-- 重点看下视频处理技术 -->


<!-- 3.一个完整直播app实现流程 -->

1.采集、2.滤镜处理、3.编码、4.推流、5.CDN分发、6.拉流、7.解码、8.播放、9.聊天互动

<!-- 2.视频处理（美颜，水印） -->

因为视频最终也是通过GPU，一帧一帧渲染到屏幕上的，所以我们可以利用OpenGL ES，对视频帧进行各种加工，从而视频各种不同的效果，就好像一个水龙头流出的水，经过若干节管道，然后流向不同的目标

<!-- ***** 视频处理框架 ***** -->


1） GPUImage : 

GPUImage是一个基于OpenGL ES的一个强大的图像/视频处理框架,封装好了各种滤镜同时也可以编写自定义的滤镜,

其本身内置了多达120多种常见的滤镜效果。

2） OpenGL:

OpenGL（全写Open Graphics Library）是个定义了一个跨编程语言、跨平台的编程接口的规格，它用于三维图象（二维的亦可）。

OpenGL是个专业的图形程序接口，是一个功能强大，调用方便的底层图形库。

3） OpenGL ES:

OpenGL ES (OpenGL for Embedded Systems) 是 OpenGL三维图形 API 的子集，
针对手机、PDA和游戏主机等嵌入式设备而设计。



```
![](/images/posts/{{page.title}}/live.jpeg)

- [landmarks](https://developer.apple.com/documentation/vision/vnfaceobservation/2867250-landmarks?language=objc)

```
<!-- iOS-11-by-Examples/iOS-11-by-Examples/Vision/Landmarks/LandmarksService.swift -->

https://github.com/artemnovichkov/iOS-11-by-Examples/blob/master/iOS-11-by-Examples/Vision/Landmarks/LandmarksService.swift

```


- [【如何快速的开发一个完整的iOS直播app】(播放篇)](https://www.jianshu.com/p/7b2f1df74420)


```
<!-- bash: bash是一种shell解释器版本 -->

<!-- shell:通常我们说的shell,指的是shell脚本语言 -->

第一行一定要指明系统需要哪种shell解释器解释你的shell脚本，如：#! /bin/bash，使用bash解析脚本语言

<!-- https://github.com/Bilibili/ijkplayer -->


```

- [【如何快速的开发一个完整的iOS直播app】(采集篇)](https://www.jianshu.com/p/c71bfda055fa)

```


<!-- 基本知识介绍 -->

1)AVFoundation: 音视频数据采集需要用AVFoundation框架.

2)AVCaptureDevice：硬件设备，包括麦克风、摄像头，通过该对象可以设置物理设备的一些属性（例如相机聚焦、白平衡等）

3)AVCaptureDeviceInput：硬件输入对象，

可以根据AVCaptureDevice创建对应的AVCaptureDeviceInput对象，用于管理硬件输入数据。

4)AVCaptureOutput：硬件输出对象，用于接收各类输出数据，通常使用对应的子类AVCaptureAudioDataOutput（声音数据输出对象）、AVCaptureVideoDataOutput（视频数据输出对象）


5)AVCaptionConnection:
当把一个输入和输出添加到AVCaptureSession之后，AVCaptureSession就会在输入、输出设备之间建立连接,而且通过AVCaptureOutput可以获取这个连接对象。


6) AVCaptureVideoPreviewLayer:相机拍摄预览图层，

能实时查看拍照或视频录制效果，创建该对象需要指定对应的AVCaptureSession对象，因为AVCaptureSession包含视频输入数据，有视频数据才能展示。



7)AVCaptureSession:  协调输入与输出之间传输数据


<!-- 捕获音视频步骤:官方文档 -->

https://link.jianshu.com/?t=https://developer.apple.com/library/ios/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/04_MediaCapture.html


1.创建AVCaptureSession对象

2.获取AVCaptureDevicel录像设备（摄像头），录音设备（麦克风），注意不具备输入数据功能,只是用来调节硬件设备的配置。

3.根据音频/视频硬件设备(AVCaptureDevice)创建音频/视频硬件输入数据对象(AVCaptureDeviceInput)，专门管理数据输入。

4.创建视频输出数据管理对象（AVCaptureVideoDataOutput），并且设置样品缓存代理(setSampleBufferDelegate)就可以通过它拿到采集到的视频数据

5.创建音频输出数据管理对象（AVCaptureAudioDataOutput），并且设置样品缓存代理(setSampleBufferDelegate)就可以通过它拿到采集到的音频数据

6.将数据输入对象AVCaptureDeviceInput、数据输出对象AVCaptureOutput添加到媒体会话管理对象AVCaptureSession中,就会自动让音频输入与输出和视频输入与输出产生连接.

7.创建视频预览图层AVCaptureVideoPreviewLayer并指定媒体会话，添加图层到显示容器layer中

8.启动AVCaptureSession，只有开启，才会开始输入到输出数据流传输。


<!-- 视频采集额外功能一（切换摄像头） -->

1.获取当前视频设备输入对象

2.判断当前视频设备是前置还是后置

3.确定切换摄像头的方向

4.根据摄像头方向获取对应的摄像头设备

5.创建对应的摄像头输入对象

6.从会话中移除之前的视频输入对象

7.添加新的视频输入对象到会话中



<!-- https://link.jianshu.com/?t=https://github.com/iThinkerYZ/YZLiveAppDemo.git -->

Analyzing dependencies
Downloading dependencies
Installing AFNetworking (3.1.0)
Using MJExtension (3.0.13)
Using SDWebImage (3.7.6)
Generating Pods project
Integrating client project
Sending stats
Pod installation complete! There are 3 dependencies from the Podfile and 3 total pods installed.




```




- [在直播应用中添加Faceu效果](https://www.jianshu.com/p/ba1f79f8f6fa)

```

<!-- 2.水印 -->

要理解它的实现原理，需要搞懂GPUImageUIElement和GPUImageAlphaBlendFilter。

1)GPUImageUIElement的作用是把一个视图的layer通过CALayer的renderInContext:方法把layer转化为image，然后作为OpenGL的纹理传给GPUImageAlphaBlendFilter。

2) 而 GPUImageAlphaBlendFilter 则是一个两输入的blend filter,
 它的第一个输入是摄像头数据，第二个输入则是刚刚提到的GPUImageUIElement的数据，
 GPUImageAlphaBlendFilter将这两个输入做alpha blend，可以简单的理解为将第二个输入叠加到第一个的上面，

 3)更多关于[alpha blend](https://en.wikipedia.org/wiki/Alpha_compositing#Alpha_blending)在维基百科上有介绍。


<!-- 3.人脸检测 -->

   然后通过将摄像头数据CMSampleBufferRef转化为CIImage，对CIImage用CIDetector进行人脸检测:

<!--  -->

```

- [/usr/local/bin/pod: /System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/bin/ruby: bad interpreter: No such file or directory]

```
devzkndeMacBook-Pro:restore-symbol devzkn$ sudo gem install -n /usr/local/bin cocoapods


<!-- devzkndeMacBook-Pro:YLFaceuDemo devzkn$ pod update-->

Update all pods
Re-creating CocoaPods due to major version update.
Updating local specs repositories
  $ /usr/bin/git -C /Users/devzkn/.cocoapods/repos/master fetch origin --progress

```

- [iOS黑科技之(AVFoundation)动态人脸识别(二)](https://www.jianshu.com/p/5e624dc68a64)

```
<!-- 一. CoreImage静态人脸识别, 可识别照片, 图像等 -->

<!-- 二. AVFoundation -->

<!-- 1. AVCaptureDevice:代表硬件设备 -->

从这个类中获取手机硬件的照相机、声音传感器

当我们在应用程序中需要改变一些硬件设备的属性（例如：切换摄像头、闪光模式改变、相机聚焦改变）的时候必须要先为设备加锁，修改完成后解锁。


//4. 移除旧输入，添加新输入
//4.1 设备加锁
session.beginConfiguration()
//4.2. 移除旧设备
session.removeInput(deviceIn)
//4.3 添加新设备
session.addInput(newVideoInput)
//4.4 设备解锁
session.commitConfiguration()


<!-- 2. AVCaptureDeviceInput:设备输入数据管理对象 -->

该对象将会被添加到AVCaptureSession中管理,代表输入设备，它配置抽象硬件设备的ports。通常的输入设备有（麦克风，相机等）

<!-- 3. AVCaptureOutput: 代表输出数据 -->

输出的可以是图片（AVCaptureStillImageOutput）或者视频（AVCaptureMovieFileOutput）

<!-- 4. AVCaptureSession: 媒体（音、视频）捕捉会话 -->

负责把捕捉的音频视频数据输出到输出设备中。


是连接AVCaptureInput和AVCaptureOutput的桥梁，它协调input到output之间传输数据。

它有startRunning和stopRunning两种方法来开启会话和结束会话。

<!-- 5. AVCaptureVideoPreviewLayer: 图片预览层 -->

我们的照片以及视频是如何显示在手机上的呢？那就是通过把这个对象添加到UIView的layer上的


<!-- 三. 添加扫描设备 -->


//3.创建原数据的输出对象
let metadataOutput = AVCaptureMetadataOutput()
        
//4.设置代理监听输出对象输出的数据，在主线程中刷新
metadataOutput.setMetadataObjectsDelegate(self, queue: DispatchQueue.main)

//7.告诉输出对象要输出什么样的数据,识别人脸, 最多可识别10张人脸
metadataOutput.metadataObjectTypes = [.face]

<!-- 主要代码如下: -->

fileprivate func addScaningVideo(){
    //1.获取输入设备（摄像头）
    guard let device = AVCaptureDevice.default(for: .video) else { return }
    
    //2.根据输入设备创建输入对象
    guard let deviceIn = try? AVCaptureDeviceInput(device: device) else { return }
    deviceInput = deviceIn
    
    //3.创建原数据的输出对象
    let metadataOutput = AVCaptureMetadataOutput()
    
    //4.设置代理监听输出对象输出的数据，在主线程中刷新
    metadataOutput.setMetadataObjectsDelegate(self, queue: DispatchQueue.main)
    //4.2 设置输出代理
    faceDelegate = previewView
    
    //5.设置输出质量(高像素输出)
    session.sessionPreset = .high
    
    //6.添加输入和输出到会话
    if session.canAddInput(deviceInput!) {
        session.addInput(deviceInput!)
    }
    if session.canAddOutput(metadataOutput) {
        session.addOutput(metadataOutput)
    }
    
    //7.告诉输出对象要输出什么样的数据,识别人脸, 最多可识别10张人脸
    metadataOutput.metadataObjectTypes = [.face]
    
    //8.创建预览图层
    previewLayer = AVCaptureVideoPreviewLayer(session: session)
    previewLayer.videoGravity = .resizeAspectFill
    previewLayer.frame = view.bounds
    previewView.layer.insertSublayer(previewLayer, at: 0)
    
    //9.设置有效扫描区域(默认整个屏幕区域)（每个取值0~1, 以屏幕右上角为坐标原点）
    metadataOutput.rectOfInterest = previewView.bounds
    
    //10. 开始扫描
    if !session.isRunning {
        DispatchQueue.global().async {
            self.session.startRunning()
        }
    }
}



<!-- 4. AVMetadataFaceObject介绍 -->

faceID: 人脸的唯一标识
扫描出来的每一个人, 有不同的faceID
同一个人, 不同的状态下(摇头, 歪头, 抬头等), 都会有不同faceID

hasYawAngle: 是否有偏转角(左右摇头)




<!-- https://link.jianshu.com/?t=https://github.com/coderQuanjun/JunFaceRecognition -->




```

- [https://cocoapods.org/pods/GoogleMobileVision](https://cocoapods.org/pods/GoogleMobileVision)

```
iOS Vision API Samples
These samples demonstrate the vision API for detecting faces and data output.

```

- [googlesamples](https://github.com/googlesamples/ios-vision)

```

<!-- Add Face Tracking with GoogleMVDataOutput To Your App -->
https://developers.google.com/vision/ios/face-tracker-tutorial


```

- [https://cocoapods.org/?q=GoogleMobileVision](https://cocoapods.org/?q=GoogleMobileVision)

```

Face Verification  https://github.com/voiceittech/VoiceItApi2IosSDK


```

- [LANDMARK](https://cocoapods.org/?q=lang%3Aobjc%20on%3Aios%20landmark)

```

pod 'FaceDetectSDK', '~> 0.0'



```

- [interface_g_m_v_face_feature](https://developers.google.com/vision/ios/reference/interface_g_m_v_face_feature)

```

Describes a face detected in a still image frame.Its properties provide face landmark information.



<!-- Detect Facial Features in Photos -->


<!-- Get Started with the Mobile Vision iOS API -->
https://developers.google.com/vision/ios/getting-started
The Google Mobile Vision iOS SDK and related samples are distributed through CocoaPods.

<!-- face-tracker-tutorial -->
https://developers.google.com/vision/ios/face-tracker-tutorial

<!-- A face detect SDK to detect face acts, poses, landmarks. -->

https://cocoapods.org/?q=lang%3Aobjc%20on%3Aios%20landmark




```

- [AVCaptureSession 自定义摄像头](https://developer.apple.com/documentation/avfoundation/avcapturesession?language=objc)


- [人脸识别: using the system face detection via AVCaptureMetadataOutput](https://github.com/zweigraf/face-landmarking-ios)

- [Permissions issue when linking python3](https://github.com/Homebrew/homebrew-core/issues/19286)

```

<!-- Error: An unexpected error occurred during the `brew link` step
The formula built, but is not symlinked into /usr/local
Permission denied @ dir_s_mkdir - /usr/local/Frameworks
Error: Permission denied @ dir_s_mkdir - /usr/local/Frameworks
 -->
devzkndeMacBook-Pro:swift devzkn$ sudo mkdir  /usr/local/Frameworks

devzkndeMacBook-Pro:swift devzkn$ sudo chown $(whoami):admin /usr/local/Frameworks
```