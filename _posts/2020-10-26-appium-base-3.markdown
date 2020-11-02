---
layout: post
title: Appium-处理系统弹窗
date: 2020-10-26 15:32:00 +0900
category: Appium
tag: Appium
---
# 前言：
最近在搞appium自动化，iOS的系统弹窗是大家都会遇到的，本文来总结处理这种弹窗的用法。

----

##### 环境：
MacOS：10.13.4
Appium-desktop：1.6.1
Xcode：9.3.1
----

#### 一、使用switch_to.alert处理弹窗
```
#所有弹窗默认允许
self.driver.switch_to.alert.accept()
```
首先推荐这种方式，几乎不会失败。解决这个问题之后，作者默默的高兴了一中午。

---

#### 二、使用App Inspector定位弹窗元素
```
#弹窗中允许按键，xpath为：//XCUIElementTypeApplication[1]/XCUIElementTypeWindow[6]/XCUIElementTypeOther[2]/XCUIElementTypeAlert[1]/XCUIElementTypeOther[1]
self.driver.find_element_by_xpath(locator).click()
```
这种情况有时候会识别不到元素，从而失败，导致自动化用例无法继续运行，建议使用第一种方法。

---

#### 三、错误方法：'autoAcceptAlerts': True
Appium更新后，改为使用XCUITest后，该参数：autoAcceptAlerts 已经废弃，[官网](https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/caps.md)
已经详细说明，请大家及时更新信息，不要被这个问题所困扰。

----
以上
