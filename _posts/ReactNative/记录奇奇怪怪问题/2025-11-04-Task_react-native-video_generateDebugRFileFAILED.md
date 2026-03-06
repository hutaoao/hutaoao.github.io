---
title: Task_react-native-video_generateDebugRFileFAILED
date: 2025-11-04
description: Task :react-native-video:generateDebugRFile FAILED
tags: [ReactNative, 记录奇奇怪怪问题, react, native]
categories: [ReactNative, 记录奇奇怪怪问题]
---
# Task :react-native-video:generateDebugRFile FAILED

具体截图：

![1728886022560-c7c71ebc-2f4f-4176-afea-ec0ab83d55b6.png](/assets/img/posts/ReactNative/1728886022560-c7c71ebc-2f4f-4176-afea-ec0ab83d55b6-353351.png)

`react-native-video`版本：

![1728886335446-83b9d4e3-caf1-4b2d-85f0-1712f57be9f6.png](/assets/img/posts/ReactNative/1728886335446-83b9d4e3-caf1-4b2d-85f0-1712f57be9f6-210719.png)

有一天莫名其妙就抱这个错

### 解决

修改一下：/node\_modules/react-native-video/android/build.gradle

```plain
dependencies {
     //noinspection GradleDynamicVersion
     implementation "com.facebook.react:react-native:${safeExtGet('reactNativeVersion', '+')}"
-    implementation 'com.yqritc:android-scalablevideoview:1.0.4'
+    implementation 'com.github.MatrixFrog:Android-ScalableVideoView:v1.0.4-jitpack'
 }
```

![1728886543265-414fc04d-8b47-46b8-8f88-dc703de99200.png](/assets/img/posts/ReactNative/1728886543265-414fc04d-8b47-46b8-8f88-dc703de99200-331525.png)

> 据说应该是我这个\*\*版本\*\*的问题，更新版本就能解决问题


