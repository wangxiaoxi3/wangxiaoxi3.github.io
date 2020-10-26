---
layout: post
title: Appium-超过60s的应用场景如何处理
date: 2020-10-26 15:30:00 +0900
category: Appium
tag: Appium
---
## 前言：
最近在搞appium自动化项目，遇到超过60s的应用场景时，总是报错报错。如何解决呢？见下文。
报错信息：
```
2018-05-21 14:03:42:253 - [HTTP] <-- POST /wd/hub/session/6b55742d-aa16-413c-aedd-ba69a89ced41/element/14/click 200 135 ms - 76
2018-05-21 14:03:42:253 - [HTTP]
2018-05-21 14:04:42:252 - [BaseDriver] Shutting down because we waited 60 seconds for a command
2018-05-21 14:04:42:253 - [debug] [AndroidDriver] Shutting down Android driver
2018-05-21 14:04:42:253 - [Appium] Closing session, cause was 'New Command Timeout of 60 seconds expired. Try customizing the timeout using the 'newCommandTimeout' desired capability'
2018-05-21 14:04:42:254 - [Appium] Removing session 6b55742d-aa16-413c-aedd-ba69a89ced41 from our master session list
2018-05-21 14:04:42:254 - [debug] [AndroidDriver] Resetting IME to io.appium.android.ime/.UnicodeIME
```
---
## 解决方案：
Appium在没有收到下一个命令时，默认超时时间是60s，超时后应用将会自动关闭session，所以你接下来的所有操作都将失败。
```
capabilities = {
                    'automationName': 'UIAutomator2',
                    #可以通过newcommandtimeout将超时时间改长，这样就解决了该问题！！
                    #超时时间可按照实际情况自定义！
                    'newCommandTimeout': "2000",
                    'unicodeKeyboard': True,
                    'resetKeyboard': True,
                    'noSign': True
                    }
    host = "http://localhost:4723/wd/hub"
    driver = webdriver.Remote(host, capabilities)
```
