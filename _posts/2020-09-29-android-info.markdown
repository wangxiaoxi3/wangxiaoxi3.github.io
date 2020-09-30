---
layout: post
title: 获取App系统资源
date: 2020-09-29 14:20:23 +0900
category: Android
tag: Android专项测试
---
### 1、获取Top信息
`adb shell top  | grep {package_name}`

### 2、获取CPU数据
`adb shell dumpsys cpuinfo`

### 3、获取内存数据
`adb shell dumpsys meminfo 进程消耗内存列表 `

`adb shell dumpsys meminfo pakagename or Pid`

`adb shell getprop dalvik.vm.heapgrowthlimit  查看进程可用的最大内存`

### 4、获取流量信息
`adb shell cat /proc/#pid#/net/dev 需先获取进程id`

```
Receive 表示收包；
Transmit 表示发包；
Bytes 表示收发的字节数；
Packets 表示收发正确的包量；
errs 表示收发错误的包量；
Drop 表示收发丢弃的包量；
```

### 5、获取帧数FPS

打开手机“设置”→“开发者选项”，找到监控一栏点击“GPU更显模式分析”→勾选上“adb shell dumpsys gfxinfo”

`adb shell dumpsys gfxinfo {package_name}`

* Draw 表示在Java中创建显示列表部分中，OnDraw()方法占用的时间；
* Prepare 准备时间；
* Process 表示渲染引擎执行显示列表所花的时间，view越多，时间就越长；
* Eexecute 表示把一帧数据发送到屏幕上排版显示实际花费的时间，其实是实际显示帧数据的后台缓存区与前台缓冲区交换后并将前台缓冲区的内容显示到屏幕上的时间；
将上面的四个时间加起来就是绘制一帧需要的时间，如果超过16ms就要有掉帧了。

### 6、获取电量信息
`adb  shell dumpsys battery`




