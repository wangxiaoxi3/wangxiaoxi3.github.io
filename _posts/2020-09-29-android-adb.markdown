---
layout: post
title: Adb命令
date: 2020-09-29 15:20:23 +0900
category: Android
tag: Adb
---
#### 查看设备列表

`adb devices`

#### 进入模拟器的shell模式

`adb shell`

#### 获取管理员权限

`adb root`

#### 安装软件

`adb install [-r] [-s] /Users/wangjuan/Downloads/v6.6.apk`  `电脑本地安装`

`adb shell pm install [-k] /storage/emulated/0/apk/v6.6.apk`  `手机本地安装`

#### 卸载软件

`adb shell pm uninstall [-k] <包名>`

#### 从电脑上发送文件到设备

`adb push /Users/wangjuan/Downloads/v6.6.apk /storage/emulated/0/apk`

#### 从设备上发送文件到电脑本地

`adb pull /storage/emulated/0/apk/v6.6.apk /Users/wangjuan/Downloads/`

#### 启动计算机adb 服务进程

`adb start-server`

#### 关闭计算机adb 服务进程

`adb kill-server`

#### 直接获取具体进程的信息

`adb shell dumpsys meminfo | grep pkg_name or pid`

#### 重启设备- 使用界面：

`adb reboot`

#### 重启设备 - bootloader引导模式：

`adb reboot-bootloader`
`adb reboot bootloader`

#### 重启设备 - recovery刷机模式：

`adb reboot recovery`

#### 返回设备状态

`adb get-state`

#### App信息 - 启动APP应用

`adb shell am start -W -n com.android/.view.Activity -S `

#### 获取App启动时间

`adb shell am start -W [pkg_name]/[activity]`

#### App信息 - 获取任务列表:

`adb shell dumpsys activity activities`

#### App信息 - App入口

`adb logcat |grep -I Displayed`

#### App信息 - 获取当前界面元素:

`adb shell dumpsys activity top `

#### 获取手机错误Log

```
adb shell
cat /data/anr/traces.txt
more /data/anr/traces.txt
```

#### 查看logcat日志信息—按照tag输出

`adb logcat -s tag:I`     `可以指定多个[TAG:LEVEL]`

`adb logcat *:V>log.log`

#### 查看logcat日志信息—按照包名、进程号、关键字查看

`adb logcat | grep tencent`

`adb logcat -c && adb logcat`    `查看当前开始的log，清空之前的缓存`

> 也可直接在adb shell 之后运行。

#### 将内容输送至文本框

`adb shell input text ‘Hello world’`
> 需要输入的文本框获得焦点。

#### 录屏

`adb shell screenrecord /sdcard/tmp.mp4`

#### 截屏

`adb shell screencap -p /storage/emulated/0/DCIM/screenshot/tmp.png`

#### 查看指定包名应用的数据库存储信息

`adb shell dumpsys dbinfo [packageName]`

#### 查看指定包名应用的详细信息

> 相当于输出AndroidManifest.xml的内容

`adb shell dumpsys package [packageName]`

#### 查看当前应用的Activity信息

`adb shell dumpsys activity top`

#### 获取整个系统内存的大致使用情况

`adb shell cat /proc/meminfo`

#### 获取内存使用情况

`adb shell procrank | grep pck_name`    `部分手机不能使用，需要安装`

```
VSS (Virtual Set Size), 虚拟消耗内存，包含共享库占用；
RSS (Residen Set Size), 实际使用物理内存，包含共享库；
PSS (Proportional Set Size), 实际使用的物理内存，比例分配共享库占用的内存；
USS (Unique Set Size), 进程独自占用的物理内存，不包含共享库占用的内存；
一般来说，内存占用大小，VSS >= RSS >= PSS >= USS
```

```
1）将procrank文件push到手机的  /system/xbin目录下；
命令：adb push procrank /system/xbin
将procmem文件push到手机的  /system/xbin目录下；
命令：adb push procmem /system/xbin
将libpagemap.so文件push到手机的  /system/lib目录下；
命令：adb push libpagemap.so /system/lib

2）进入adb shell，获取root权限，分别给procrank、procmem、libpagemap.so三个文件给予777权限，命令如下：
chmod 777 /system/xbin/procrank
chmod 777 /system/xbin/procmem
chmod 777 /system/lib/libpagemap.so

3) 如果push不进三个文件或者修改不了三个文件的权限，那重新挂载一下system，再修改三个文件的权限，如下：
mount   -o  remount,rw    /system

```

#### 获取设备的ID和序列号

`adb get-product`

`adb get-serialno`

> shell环境下:

#### 拨打电话

`am start -a android.intent.action.CALL -d tel:15510700086`

#### 打开网页

`am start -a android.intent.action.VIEW -d http://www.tmall.com `

#### 启动service

`am startservice -n [packageName]`
可以是包名，也可以是[packageName].[serviceName]

#### 获取系统属性，如版本号等

`getprop ro.debuggable`

#### 查看当前应用CPU消耗

```
top -h
Usage: top [ -m max_procs ] [ -n iterations ] [ -d delay ] [ -s sort_column ] [-t ] [ -h ]
    -m num  Maximum number of processes to display. 最多显示多少个进程
    -n num  Updates to show before exiting.  刷新次数
    -d num  Seconds to wait between updates. 刷新间隔时间（默认5秒）
    -s col  Column to sort by (cpu,vss,rss,thr). 按哪列排序
    -t      Show threads instead of processes. 显示线程信息而不是进程
    -h      Display this help screen.  显示帮助文档


￼要监测单个应用，例如针对微博的CPU占用率~
监测一次微博的CPU占用情况：adb shell top -n 1 | grep com.sina.weibo
10秒刷新一次显示CPU占用情况：adb shell top -d 10 | grep com.sina.weibo
实时监测微博的CPU占用情况：adb shell top |grep com.sina.weibo

```

#### 运行dex文件
`dalvikvm -cp [dex文件] [运行主类]`
`dalvikvm -cp /data/demo.dex cn.tencent.main`

#### 查看设备端口信息
`netstat`

#### 查看设备ip
`netcfg`

#### 发送广播（broadcast）
`am broadcast -a [actionName]`

#### 启动service
`am startservice -n [packageName]`
可以是包名，也可以是[packageName].[serviceName]

#### 启动指定应用
`am start -n [packageName]`
可以是包名，也可以是[packageName].[activityName]

#### 卸载应用
`pm uninstall [packageName]`

#### 安装设备中的的apk文件
`pm install /sdcard/demo.apk`

#### 清空数据（指定包名）
`pm clear [packageName]`

#### 查看进程信息
`ps | grep tencent`

`ps -t 11987`
