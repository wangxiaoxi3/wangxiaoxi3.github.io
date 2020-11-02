---
layout: post
title: Appium-实现手势密码登陆
date: 2020-10-26 15:34:00 +0900
category: Appium
tag: Appium
---
# 前言：
前几天有人问我，手势登陆如何做？于是我找了一个APP试了试，所以本文来总结使用Python+Appium来实现手势密码登陆APP。

----

##### 环境：
MacOS：10.13.4
Appium-desktop：1.6.1
Xcode：9.3.1
APP：众安保险-iOS版
----

#### 一、Appium API -- TouchAction
Appium的辅助类，主要针对手势操作，比如滑动、长按、拖动等。

**1、按压控件**
方法：```press()```
开始按压一个元素或坐标点（x,y）。通过手指按压手机屏幕的某个位置。
举例：
```
TouchAction(driver).press(x=0,y=308).release().perform()

release() 结束的行动取消屏幕上的指针。
Perform() 执行的操作发送到服务器的命令操作。
```
**2、长按控件**
方法：```longPress()```
开始按压一个元素或坐标点（x,y）。 相比press()方法，longPress()多了一个入参，既然长按，得有按的时间吧。duration以毫秒为单位。1000表示按一秒钟。其用法与press()方法相同。
举例：
```
TouchAction(driver).longPress(x=1 ,y=302,duration=1000).perform().release();
```
**3、移动**
方法：```moveTo()```
将指针（光标）从过去指向指定的元素或点。
举例：
```
TouchAction(driver).moveTo(x=0,y=308).perform().release();
```
**4、暂停**
方法：```wait()```
暂停脚本的执行，单位为毫秒。
举例：
```
TouchAction(driver).wait(1000);
```

----

#### 二、通过触摸多点坐标进行解锁
根据上面API解释，我们可以得出按压和移动来实现手势解释，大概思路如下：
```
TouchAction.press(beginX,beginY).moveTo(xStep,yStep).moveTo(xStep,yStep).release().perform();
```
打开Appium-Inspector来查看手势对应的各个点的坐标。
选择[Swipe By Coordinates]，可查看任意点的坐标。可选择手势触摸点的中心位置。如下图所示：作者分别选择左上角的4个点，即可模拟手势来执行登陆操作。
![Appium-Inspector.png](/assets/img/cc/click-1.png)
代码如下：
```
# -*- coding: utf-8 -*-
# @Time    : 2018/5/22 下午10:33
# @Author  : WangJuan
# @File    : appium-ios.py
from time import sleep

from appium import webdriver
from appium.webdriver.common.touch_action import TouchAction

cap = {
  "platformName": "iOS",
  "platformVersion": "11.4",
  "bundleId": "com.zhongan.insurance",
  "automationName": "XCUITest",
  "udid": "3e8325a7c0d*******************a7e",
  "deviceName": "****Iphone"
}

host = "http://0.0.0.0:4728/wd/hub"
driver = webdriver.Remote(host, cap)
sleep(3)
action = TouchAction(driver)
action.press(x=98, y=321).wait(100).move_to(x=208, y=321).wait(100).move_to(x=206, y=432).wait(100).move_to(x=98, y=432).perform().release()
```

---
#### 三、兼容不同分辨率
直接用坐标点找会有一些问题，比如手机屏幕大小不同，找点的位置可能会有偏差，如何解决呢？
由下图可见，先获取第一个触摸点的坐标location及size。分别定义为```start_height、start_width、start_x、start_y```（其中```start_x、start_y```为触摸点左上角的坐标）；
即可计算出第一个触摸点的中心点坐标分别为：
```start_x + start_width/2```,   ```start_y + start_height/2```
然后在计算出第二个触摸点的中心点大致坐标为：
```start_x+start_width*2```,   ```y=start_y+start_height*2```
![Appium-Inspector-2.png](/assets/img/cc/click-2.png)
其他坐标均可按照此计算方式，详情见具体例子。
```
# -*- coding: utf-8 -*-
# @Time    : 2018/5/22 下午10:33
# @Author  : WangJuan
# @File    : appium-ios.py
from time import sleep

from appium import webdriver
from appium.webdriver.common.touch_action import TouchAction

cap = {
  "platformName": "iOS",
  "platformVersion": "11.4",
  "bundleId": "com.zhongan.insurance",
  "automationName": "XCUITest",
  "udid": "3e8325a7c0***************62bd4a7e",
  "deviceName": "My@Iphone"
}

host = "http://0.0.0.0:4728/wd/hub"
driver = webdriver.Remote(host, cap)
sleep(3)
action = TouchAction(driver)
# action.press(x=98, y=321).wait(100).move_to(x=208, y=321).wait(100).move_to(x=206, y=432).wait(100).move_to(x=98, y=432).perform().release()
start = driver.find_element_by_xpath('//XCUIElementTypeApplication[@name="众安保险"]/XCUIElementTypeWindow[1]/XCUIElementTypeOther\
                    /XCUIElementTypeOther/XCUIElementTypeOther/XCUIElementTypeOther/XCUIElementTypeOther/XCUIElementTypeOther/XCUIElementTypeOther[1]')
start_height = start.size['height']
start_width = start.size['width']
start_x = start.location['x']
start_y = start.location['y']
begin_x = start_x + start_width/2
begin_y = start_y + start_height/2

action.press(x=start_x, y=start_y).wait(100).move_to(x=start_x+start_width*2, y=begin_y).wait(100).move_to\
                  (x=start_x+start_width*2, y=start_y+start_height*2).wait(100).move_to(x=begin_x,y=start_y+start_height*2).perform().release()
```

-----
以上
