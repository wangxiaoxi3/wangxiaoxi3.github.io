---
layout: post
title: Monkey命令
date: 2020-09-29 15:20:23 +0900
category: android
tag: Monkey
---
# Monkey相关指令
**1、启动指定的应用程序，并向其发送100个伪随机事件**
* 示例：adb shell monkey -p package_name -v 100

**2、日志级别 Level 1**
* 示例：adb shell monkey -p package_name -v-v 100
说明：提供较为详细的日志，包括每个发送到Activity的事件信息

**3、日志级别 Level 2**
* 示例：adb shell monkey -p package_name -v-v-v 100
说明：最详细的日志，包括了测试中选中/未选中的Activity信息

**4、用于指定伪随机数生成器的seed值，如果seed相同，则两次Monkey测试所产生的事件序列也相同的。**
* 示例：Monkey测试1：adb shell monkey -p package_namer –s 10 100
* 示例：Monkey 测试2：adb shell monkey -p package_name –s 10 100
操作序列虽  然是随机生成的，但是只要我们指定了相同的Seed值，就可以保证两次测试产生的随机操作序列是完全相同的，所以这个操作序列伪随机的；

**5、参数：--throttle <毫秒>**
用于指定用户操作（即事件）间的时延，单位是毫秒；
* 示例：adb shell monkey -p package_name –throttle 3000 100

**6、 参数：--ignore-crashes**
用于指定当应用程序崩溃时（Force& Close错误），Monkey是否停止运行。如果使用此参数，即使应用程序崩溃，Monkey依然会发送事件，直到事件计数完成。
* 示例1：adb shellmonkey -p package_name —ignore-crashes 1000
测试过程中即使Weather程序崩溃，Monkey依然会继续发送事件直到事件数目达到1000为止；
* 示例2：adb shellmonkey -p package_name 1000
测试过程中，如果Weather程序崩溃，Monkey将会停止运行。

**7、参数：--ignore-timeouts**
用于指定当应用程序发生ANR（Application No Responding）错误时，Monkey是否停止运行。如果使用此参数，即使应用程序发生ANR错误，
Monkey依然会发送事件，直到事件计数完成。
* 示例：adb shellmonkey -p package_name —ignore-timeouts 1000

**8、 参数：—ignore-security-exceptions**
用于指定当应用程序发生许可错误时（如证书许可，网络许可等），Monkey是否停止运行。如果使用此参数，即使应用程序发生许可错误，
Monkey依然会发送事件，直到事件计数完成。
* 示例：adb shellmonkey -p package_name —ignore-security-exceptions 1000

**9、参数：—kill-process-after-error**
用于指定当应用程序发生错误时，是否停止其运行。如果指定此参数，当应用程序发生错误时，应用程序停止运行并保持在当前状态（注意：
应用程序仅是静止在发生错误时的状态，系统并不会结束该应用程序的进程）。
* 示例：adb shellmonkey -p package_name —kill-process-after-error 1000

**10、 参数：--monitor-native-crashes**
用于指定是否监视并报告应用程序发生崩溃的本地代码。
* 示例：adb shellmonkey -p package_name —monitor-native-crashes 1000
