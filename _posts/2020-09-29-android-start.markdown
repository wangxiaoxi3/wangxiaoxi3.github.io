---
layout: post
title: APP启动时间
date: 2020-09-29 14:20:23 +0900
category: Android
tag: Android专项测试
---
### 一、adb shell 命令查看

`adb shell am force-stop com.qiyi.video`

`adb shell am start  -S -W com.qiyi.video/.WelcomeActivity`

`adb logcat |grep -I activitymanager.*Displayed`

`lazy load：只能埋点`

```
thisTime:  最后一个activity时间
totalTime：包括间接启动的activity时间
waitTime：总体消耗时间
```

### 二、录屏分帧

录屏（通过录制整个加载过程得到加载过程视频）：
`adb shell screenrecord  —time-limit 10 /sdcard/video.mp4`

`adb pull /sdcard/video.mp4  /Users/wangjuan/tmp/ffmpeg`

分帧（通过ffmpeg 分帧）：
`ffmpeg -I /Users/wangjuan/tmp/ffmpeg/video.mp4  -r 100 -threads 2 /Users/wangjuan/tmp/ffmpeg/Android-Capture-%05d.png`

