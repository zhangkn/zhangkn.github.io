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


- [iOS短视频使用到的AVFoundation组件时遇到的问题，在 金山云多媒体SDK中硬编直接使用VideoToolBox做编码](https://www.jianshu.com/p/9eaf39f45265)

```

<!-- 3. 问题及解决方案 -->

移动直播的延时控制--基于ijkplayer的实践


<!-- ijkplayer框架深入剖析 -->
https://www.jianshu.com/p/daf0a61cc1e0





```


- [IOS视频直播:高仿 腾讯旗下【NOW】直播 ](https://github.com/ChinaArJun/Tencent-NOW)


```

编码: 重难点在于要在分辨率，帧率，码率，GOP等参数设计上找到最佳平衡点。
 <!-- iOS8之后,Apple开放了VideoToolbox.framework, 可以直接进行硬编解码 -->

 https://github.com/GPUImage/Tencent-NOW

<!-- ijkplayer -->

Android/iOS video player based on FFmpeg n3.4, with MediaCodec, VideoToolbox support.






```

- [IJKMediaFramework.framework]

```

<!-- 链接: https://pan.baidu.com/s/1Yfs-gs5a3euD0MOhv4nuBA 密码: p8cm -->

<!-- /Users/devzkn/code/github/YZLiveAppDemo/YZLiveApp/Pods/ijkplayer -->

<!-- devzkndeMacBook-Pro:YZLiveApp devzkn$ cat Podfile -->
# Uncomment this line to define a global platform for your project
platform :ios, '9.0'
#use_frameworks!

target 'YZLiveApp' do
  pod 'AFNetworking', '~> 3.1.0'
  pod 'SDWebImage', '~> 3.7.4’
  pod 'MJExtension', '~> 3.0.13'
#  pod 'ijkplayer', '~> 0.3.2-rc.2'
#pod 'IJKMediaFramework'
pod 'ijkplayer', '~> 1.1.0'

end


```

- [黑咔相机](https://itunes.apple.com/cn/app/id1146696085)

```
https://github.com/tensorflow/tensorflow 
https://github.com/google/protobuf/blob/master/src/google/protobuf/io/coded_stream.cc

```

- [20种ios滤镜都是基于GPUImage的，有3种滤镜是GPUImage库中包含的，还有17种是Instagram中的经典滤镜](https://github.com/forrestwoo/openApp)

```

https://github.com/GPUImage/openApp/tree/master/FWMeituApp/FWMeituApp/Custom%20Filters


<!-- FWNashvilleFilter -->

https://github.com/forrestwoo/openApp/blob/6104814d070f6d7fd63bb01a06d20d0046a8a05c/FWLifeApp/FWLifeApp/Custom%20Filters/FWNashvilleFilter.h


#import "GPUImageTwoInputFilter.h"

//创建滤镜效果 ，该类主要实现滤镜的效果，包含一个片段着色程序。它是滤镜效果的具体实现

@interface FWFilter1 : GPUImageTwoInputFilter


@end


//所有滤镜类都继承自GPUImageFilterGroup类.它允许我们所创建的类混合其他滤镜。

//它其实是向FWFilter1类中添加需要的输入纹理图片
@interface FWNashvilleFilter : GPUImageFilterGroup
{
    GPUImagePicture *imageSource ;
}

@end


<!-- https://github.com/forrestwoo/openApp/blob/6104814d070f6d7fd63bb01a06d20d0046a8a05c/FWLifeApp/FWMeituApp/FWMeituApp/Custom%20Filters/FWNashvilleFilter.m -->




<!-- 应用例子：https://github.com/forrestwoo/openApp/blob/6104814d070f6d7fd63bb01a06d20d0046a8a05c/FWMeituApp/FWMeituApp/Utilies/FWApplyFilter.m -->


+ (UIImage *)applyNashvilleFilter:(UIImage *)image
{
    FWNashvilleFilter *filter = [[FWNashvilleFilter alloc] init];//自定义滤镜

    [filter forceProcessingAtSize:image.size];

    GPUImagePicture *pic = [[GPUImagePicture alloc] initWithImage:image];

    [pic addTarget:filter];// 往GPUImagePicture 添加GPUImageFilterGroup的子类
    
    [pic processImage];// -[GPUImagePicture processImage]:

    [filter useNextFrameForImageCapture];//-[GPUImageFilter useNextFrameForImageCapture]:

    return [filter imageFromCurrentFramebuffer];
}

<!--  -->


```

- [BeautifyFaceDemo](https://github.com/Guikunzhi/BeautifyFaceDemo/blob/master/README-CN.md)

```

<!-- http://m.blog.csdn.net/article/details?id=50496969 -->
图像算法---磨皮算法研究汇总


<!-- 图像算法---表面模糊算法 -->

https://blog.csdn.net/trent1985/article/details/49864397



<!-- 大家对于美颜比较常见的需求就是磨皮、美白 -->

<!-- 3.磨皮 -->

磨皮的本质实际上是模糊：就是将像素点的取值与周边的像素点取值相关联

1）：高斯模糊 它的像素点取值则是由周边像素点求加权平均所得，而权重系数则是像素间的距离的高斯函数，大致关系是距离越小、权重系数越大

https://link.jianshu.com/?t=https://zh.wikipedia.org/wiki/%E9%AB%98%E6%96%AF%E6%A8%A1%E7%B3%8A


2）：双边滤波(Bilateral Filter)  GPUImageBilateralFilter

考虑到了颜色的差异，它的像素点取值也是周边像素点的加权平均，而且权重也是高斯函数。这个权重不仅与像素间距离有关，还与像素值本身的差异有关；

https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/Bilateral_filter


3）   Combination  Filter：

通过肤色检测和边缘检测，只对皮肤和非边缘部分进行处理








```


- [App Installation failed, No code signature found.]

```

devzkndeMacBook-Pro:YZLiveApp devzkn$  plutil -p  /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/SDKSettings.plist


<!-- devzkndeMacBook-Pro:YZLiveApp devzkn$ sudo chmod +x /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/SDKSettings.plist -->

devzkndeMacBook-Pro:YZLiveApp devzkn$ sudo chmod -R 777 /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk 


    "CODE_SIGNING_REQUIRED" => "NO"

    修改为yes


最后重启xcode，搞定。

```

- [iOS:抖音短视频生成webp动图客户端解决方案](https://www.jianshu.com/p/c75938791cdf)



- [【如何快速的开发一个完整的iOS直播app】(美颜篇)](https://www.jianshu.com/p/4646894245ba)

```

<!-- https://link.jianshu.com/?t=https://github.com/iThinkerYZ/GPUImgeDemo -->


<!-- 利用GPUImage处理直播过程中美颜的流程 -->

采集视频 => 获取每一帧图片 => 滤镜处理 => GPUImageView展示

<!-- 美颜基本概念 -->


1) GPU：（Graphic Processor Unit图形处理单元）手机或者电脑用于图像处理和渲染的硬件



2) GPU工作原理：采集数据-> 存入主内存(RAM) -> CPU(计算处理) -> 存入显存(VRAM) -> GPU(完成图像渲染) -> 帧缓冲区 -> 显示器


3) OpenGL ES：（Open Graphics Library For Embedded(嵌入的) Systems 开源嵌入式系统图形处理框架），一套图形与硬件接口，用于把处理好的图片显示到屏幕上。


4) GPUImage:是一个基于OpenGL ES 2.0图像和视频处理的开源iOS框架，提供各种各样的图像处理滤镜，并且支持照相机和摄像机的实时滤镜，内置120多种滤镜效果，并且能够自定义图像滤镜。



5) 滤镜处理的原理:就是把静态图片或者视频的每一帧进行图形变换再显示出来。它的本质就是像素点的坐标和颜色变化




<!-- GPUImage处理画面原理 -->

GPUImage采用链式方式来处理画面,通过addTarget:方法为链条添加每个环节的对象，处理完一个target,就会把上一个环节处理好的图像数据传递下一个target去处理，称为GPUImage处理链。


一般的target可分为两类: 中间环节的target(GPUImageFilter)、最终环节的target（GPUImageView、GPUImageMovieWriter）


<!-- GPUImage处理主要分为3个环节 -->



source(视频、图片源) -> filter（滤镜） -> final target (处理后视频、图片)


1）GPUImaged的Source:都继承GPUImageOutput

GPUImageVideoCamera：用于实时拍摄视频

GPUImageStillCamera： 用于实时拍摄照片

GPUImagePicture： 用于处理已经拍摄好的图片，比如png,jpg图片

GPUImageMovie： 用于处理已经拍摄好的视频,比如mp4文件

2） GPUImage的filter:GPUimageFilter类

这个类继承自GPUImageOutput,并且遵守GPUImageInput协议，这样既能流进，又能流出


3） GPUImage的final target:GPUImageView,GPUImageMovieWriter


<!-- 美颜原理 -->

1） 磨皮GPUImageBilateralFilter ： 让像素点模糊，可以使用高斯模糊；双边滤波(Bilateral Filter) ，有针对性的模糊像素点，能保证边缘不被模糊。

2） 美白(GPUImageBrightnessFilter)：本质就是提高亮度。

<!-- GPUImage实战 -->


<!-- GPUImage原生美颜 -->

步骤一：使用Cocoapods导入GPUImage

步骤二：创建视频源GPUImageVideoCamera

步骤三：创建最终目的源：GPUImageView

步骤四：创建滤镜组(GPUImageFilterGroup)，需要组合亮度(GPUImageBrightnessFilter)和双边滤波(GPUImageBilateralFilter)这两个滤镜达到美颜效果.

步骤五：设置滤镜组链

步骤六：设置GPUImage处理链，从数据源 => 滤镜 => 最终界面效果

步骤七：开始采集视频

<!-- 注意点： -->

1）SessionPreset最好使用AVCaptureSessionPresetHigh，会自动识别，

2） GPUImageVideoCamera必须要强引用，否则会被销毁，不能持续采集视频.

3） 必须调用startCameraCapture，底层才会把采集到的视频源，渲染到GPUImageView中，就能显示了。

4） GPUImageBilateralFilter的distanceNormalizationFactor值越小，磨皮效果越好,distanceNormalizationFactor取值范围: 大于1。


<!-- 利用美颜滤镜实现 -->


步骤一：使用Cocoapods导入GPUImage

步骤二：导入GPUImageBeautifyFilter文件夹

步骤三：创建视频源GPUImageVideoCamera

步骤四：创建最终目的源：GPUImageView

步骤五：创建最终美颜滤镜：GPUImageBeautifyFilter

步骤六：设置GPUImage处理链，从数据源 => 滤镜 => 最终界面效果

<!-- 注意： -->

切换美颜效果原理：移除之前所有处理链，重新设置处理链





```

- [GPUImage所有滤镜介绍](http://www.360doc.com/content/15/0907/10/19175681_497418716.shtml)

```

GPUImage是Brad Larson在github托管的一个开源项目，项目实现了图片滤镜、摄像头实时滤镜，

该项目的优点不但在于滤镜很多，而且处理效果是基于GPU的，比使用CPU性能更高。


<!-- 下载地址是：https://github.com/BradLarson/GPUImage -->


<!-- 已有的一些filter介绍： -->

#import "GPUImageBrightnessFilter.h"                //亮度

#import "GPUImageExposureFilter.h"                  //曝光

#import "GPUImageContrastFilter.h"                  //对比度

#import "GPUImageSaturationFilter.h"                //饱和度

#import "GPUImageGammaFilter.h"                     //伽马线

#import "GPUImageColorInvertFilter.h"               //反色

#import "GPUImageSepiaFilter.h"                     //褐色（怀旧）

#import "GPUImageLevelsFilter.h"                    //色阶

#import "GPUImageGrayscaleFilter.h"                 //灰度

#import "GPUImageHistogramFilter.h"                 //色彩直方图，显示在图片上

#import "GPUImageHistogramGenerator.h"              //色彩直方图

#import "GPUImageRGBFilter.h"                       //RGB

#import "GPUImageToneCurveFilter.h"                 //色调曲线

#import "GPUImageMonochromeFilter.h"                //单色

#import "GPUImageOpacityFilter.h"                   //不透明度

#import "GPUImageHighlightShadowFilter.h"           //提亮阴影

#import "GPUImageFalseColorFilter.h"                //色彩替换（替换亮部和暗部色彩）

#import "GPUImageHueFilter.h"                       //色度

#import "GPUImageChromaKeyFilter.h"                 //色度键

#import "GPUImageWhiteBalanceFilter.h"              //白平横

#import "GPUImageAverageColor.h"                    //像素平均色值

#import "GPUImageSolidColorGenerator.h"             //纯色

#import "GPUImageLuminosity.h"                      //亮度平均

#import "GPUImageAverageLuminanceThresholdFilter.h" //像素色值亮度平均，图像黑白（有类似漫画效果）


#import "GPUImageLookupFilter.h"                    //lookup 色彩调整

#import "GPUImageAmatorkaFilter.h"                  //Amatorka lookup

#import "GPUImageMissEtikateFilter.h"               //MissEtikate lookup

#import "GPUImageSoftEleganceFilter.h"              //SoftElegance lookup



<!-- #pragma mark - 图像处理 Handle Image -->


#import "GPUImageCrosshairGenerator.h"              //十字

#import "GPUImageLineGenerator.h"                   //线条



#import "GPUImageTransformFilter.h"                 //形状变化

#import "GPUImageCropFilter.h"                      //剪裁

#import "GPUImageSharpenFilter.h"                   //锐化

#import "GPUImageUnsharpMaskFilter.h"               //反遮罩锐化


#import "GPUImageFastBlurFilter.h"                  //模糊

#import "GPUImageGaussianBlurFilter.h"              //高斯模糊

#import "GPUImageGaussianSelectiveBlurFilter.h"     //高斯模糊，选择部分清晰

#import "GPUImageBoxBlurFilter.h"                   //盒状模糊

#import "GPUImageTiltShiftFilter.h"                 //条纹模糊，中间清晰，上下两端模糊

#import "GPUImageMedianFilter.h"                    //中间值，有种稍微模糊边缘的效果

#import "GPUImageBilateralFilter.h"                 //双边模糊

#import "GPUImageErosionFilter.h"                   //侵蚀边缘模糊，变黑白

#import "GPUImageRGBErosionFilter.h"                //RGB侵蚀边缘模糊，有色彩

#import "GPUImageDilationFilter.h"                  //扩展边缘模糊，变黑白

#import "GPUImageRGBDilationFilter.h"               //RGB扩展边缘模糊，有色彩

#import "GPUImageOpeningFilter.h"                   //黑白色调模糊

#import "GPUImageRGBOpeningFilter.h"                //彩色模糊

#import "GPUImageClosingFilter.h"                   //黑白色调模糊，暗色会被提亮

#import "GPUImageRGBClosingFilter.h"                //彩色模糊，暗色会被提亮

#import "GPUImageLanczosResamplingFilter.h"         //Lanczos重取样，模糊效果

#import "GPUImageNonMaximumSuppressionFilter.h"     //非最大抑制，只显示亮度最高的像素，其他为黑

#import "GPUImageThresholdedNonMaximumSuppressionFilter.h" //与上相比，像素丢失更多


#import "GPUImageSobelEdgeDetectionFilter.h"        //Sobel边缘检测算法(白边，黑内容，有点漫画的反色效果)

#import "GPUImageCannyEdgeDetectionFilter.h"        //Canny边缘检测算法（比上更强烈的黑白对比度）

#import "GPUImageThresholdEdgeDetectionFilter.h"    //阈值边缘检测（效果与上差别不大）

#import "GPUImagePrewittEdgeDetectionFilter.h"      //普瑞维特(Prewitt)边缘检测(效果与Sobel差不多，貌似更平滑)

#import "GPUImageXYDerivativeFilter.h"              //XYDerivative边缘检测，画面以蓝色为主，绿色为边缘，带彩色

#import "GPUImageHarrisCornerDetectionFilter.h"     //Harris角点检测，会有绿色小十字显示在图片角点处

#import "GPUImageNobleCornerDetectionFilter.h"      //Noble角点检测，检测点更多

#import "GPUImageShiTomasiFeatureDetectionFilter.h" //ShiTomasi角点检测，与上差别不大

#import "GPUImageMotionDetector.h"                  //动作检测

#import "GPUImageHoughTransformLineDetector.h"      //线条检测

#import "GPUImageParallelCoordinateLineTransformFilter.h" //平行线检测


#import "GPUImageLocalBinaryPatternFilter.h"        //图像黑白化，并有大量噪点


#import "GPUImageLowPassFilter.h"                   //用于图像加亮

#import "GPUImageHighPassFilter.h"                  //图像低于某值时显示为黑




<!-- #pragma mark - 视觉效果 Visual Effect -->


#import "GPUImageSketchFilter.h"                    //素描

#import "GPUImageThresholdSketchFilter.h"           //阀值素描，形成有噪点的素描

#import "GPUImageToonFilter.h"                      //卡通效果（黑色粗线描边）

#import "GPUImageSmoothToonFilter.h"                //相比上面的效果更细腻，上面是粗旷的画风

#import "GPUImageKuwaharaFilter.h"                  //桑原(Kuwahara)滤波,水粉画的模糊效果；处理时间比较长，慎用


#import "GPUImageMosaicFilter.h"                    //黑白马赛克

#import "GPUImagePixellateFilter.h"                 //像素化

#import "GPUImagePolarPixellateFilter.h"            //同心圆像素化

#import "GPUImageCrosshatchFilter.h"                //交叉线阴影，形成黑白网状画面

#import "GPUImageColorPackingFilter.h"              //色彩丢失，模糊（类似监控摄像效果）


#import "GPUImageVignetteFilter.h"                  //晕影，形成黑色圆形边缘，突出中间图像的效果

#import "GPUImageSwirlFilter.h"                     //漩涡，中间形成卷曲的画面

#import "GPUImageBulgeDistortionFilter.h"           //凸起失真，鱼眼效果

#import "GPUImagePinchDistortionFilter.h"           //收缩失真，凹面镜

#import "GPUImageStretchDistortionFilter.h"         //伸展失真，哈哈镜

#import "GPUImageGlassSphereFilter.h"               //水晶球效果

#import "GPUImageSphereRefractionFilter.h"          //球形折射，图形倒立
    
#import "GPUImagePosterizeFilter.h"                 //色调分离，形成噪点效果

#import "GPUImageCGAColorspaceFilter.h"             //CGA色彩滤镜，形成黑、浅蓝、紫色块的画面

#import "GPUImagePerlinNoiseFilter.h"               //柏林噪点，花边噪点

#import "GPUImage3x3ConvolutionFilter.h"            //3x3卷积，高亮大色块变黑，加亮边缘、线条等

#import "GPUImageEmbossFilter.h"                    //浮雕效果，带有点3d的感觉

#import "GPUImagePolkaDotFilter.h"                  //像素圆点花样

#import "GPUImageHalftoneFilter.h"                  //点染,图像黑白化，由黑点构成原图的大致图形




<!-- #pragma mark - 混合模式 Blend -->


#import "GPUImageMultiplyBlendFilter.h"             //通常用于创建阴影和深度效果

#import "GPUImageNormalBlendFilter.h"               //正常

#import "GPUImageAlphaBlendFilter.h"                //透明混合,通常用于在背景上应用前景的透明度

#import "GPUImageDissolveBlendFilter.h"             //溶解

#import "GPUImageOverlayBlendFilter.h"              //叠加,通常用于创建阴影效果

#import "GPUImageDarkenBlendFilter.h"               //加深混合,通常用于重叠类型

#import "GPUImageLightenBlendFilter.h"              //减淡混合,通常用于重叠类型

#import "GPUImageSourceOverBlendFilter.h"           //源混合

#import "GPUImageColorBurnBlendFilter.h"            //色彩加深混合

#import "GPUImageColorDodgeBlendFilter.h"           //色彩减淡混合

#import "GPUImageScreenBlendFilter.h"               //屏幕包裹,通常用于创建亮点和镜头眩光

#import "GPUImageExclusionBlendFilter.h"            //排除混合

#import "GPUImageDifferenceBlendFilter.h"           //差异混合,通常用于创建更多变动的颜色

#import "GPUImageSubtractBlendFilter.h"             //差值混合,通常用于创建两个图像之间的动画变暗模糊效果

#import "GPUImageHardLightBlendFilter.h"            //强光混合,通常用于创建阴影效果

#import "GPUImageSoftLightBlendFilter.h"            //柔光混合

#import "GPUImageChromaKeyBlendFilter.h"            //色度键混合

#import "GPUImageMaskFilter.h"                      //遮罩混合

#import "GPUImageHazeFilter.h"                      //朦胧加暗

#import "GPUImageLuminanceThresholdFilter.h"        //亮度阈

#import "GPUImageAdaptiveThresholdFilter.h"         //自适应阈值

#import "GPUImageAddBlendFilter.h"                  //通常用于创建两个图像之间的动画变亮模糊效果

#import "GPUImageDivideBlendFilter.h"               //通常用于创建两个图像之间的动画变暗模糊效果

```


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

<!-- pod "IJKMediaFramework" -->

<!-- pod 'ijkplayer', '~> 1.1.0' -->

For more information, see https://blog.cocoapods.org and the CHANGELOG for this version at https://github.com/CocoaPods/CocoaPods/releases/tag/1.5.0.beta.1

Analyzing dependencies
Downloading dependencies
Using AFNetworking (3.1.0)
Using MJExtension (3.0.13)
Using SDWebImage (3.7.6)
Installing ijkplayer (1.1.2)
Generating Pods project
Integrating client project
Sending stats
Pod installation complete! There are 4 dependencies from the Podfile and 4 total pods installed.


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