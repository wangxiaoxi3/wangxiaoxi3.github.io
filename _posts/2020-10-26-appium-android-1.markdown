---
layout: post
title: Appium+Python3+Android入门
date: 2020-10-26 14:30:00 +0900
category: Appium
tag: Android
---
# 前言：

Appium 是一个自动化测试开源工具，支持 iOS 平台和 Android 平台上的原生应用，web 应用和混合应用。

* * *

#### 一、环境配置

1、安装Node.js

>[https://nodejs.org/](https://nodejs.org/)

2、安装Appium

>http://appium.io/ 

3、安装Android SDK

>[http://tools.android-studio.org/index.php/sdk](http://tools.android-studio.org/index.php/sdk)

4、安装Python-client

>pip3 install Appium-Python-Client

5、安装Appium-client

>npm install wd

最后，打开命令行，输入“appium-doctor”命令，提示Appium所需要的各项环境准备情况。

* * *

#### 二、服务关键字

Desired Capabilities在启动session的时候是必须提供的。

Desired Capabilities本质上是以key value字典的方式存放，客户端将这些键值对发给服务端，告诉服务端我们想要怎么测试。
```
desired_caps = {}

desired_caps['platformName'] ='Android'

desired_caps['platformVersion'] ='6.0.1'

desired_caps['deviceName'] ='e0bbc8b7'

desired_caps['appPackage'] ='com.ximalaya.ting.android'

desired_caps['appActivity'] ='com.ximalaya.ting.android.host.activity.WelComeActivity'

desired_caps["unicodeKeyboard"] ="True"

desired_caps["resetKeyboard"] ="True"

self.driver = webdriver.Remote('http://127.0.0.1:4723/wd/hub', desired_caps)
```
它告诉appium Server这样一些事情：

>deviceName：启动哪种设备，是真机还是模拟器？iPhone Simulator，iPad Simulator，iPhone Retina 4-inch，Android Emulator，Galaxy S4…

>automationName：使用哪种自动化引擎。appium（默认）还是Selendroid。

>platformName：使用哪种移动平台。iOS, Android, orFirefoxOS。

>platformVersion：指定平台的系统版本。例如指的Android平台，版本为5.1。

>appActivity：待测试的app的Activity名字。比如MainActivity、.Settings。注意，原生app的话要在activity前加个”.“。

>appPackage：待测试的app的Java package。比如com.example.android.myApp, com.android.settings。
>>注⚠️：aapt工具可获取appActivity，appPackage
aapt dump badging <file.apk>
即可获取appActivity，appPackage等～～



* * *

### 三、定位控件

1、ID定位

使用方法：```driver.find_element_by_id(‘com.android.calculator2:id/formula’)```

![image](http://upload-images.jianshu.io/upload_images/7116457-c94cd4b1cd7ba5ef?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、name定位

使用方法：```driver.find_element_by_name("9"))```

![image](http://upload-images.jianshu.io/upload_images/7116457-cec76b778ea18828?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、class name定位

使用方法：```driver.find_element_by_class_name(“android.widget.Button"))```

![image](http://upload-images.jianshu.io/upload_images/7116457-332c077f8a76ce23?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、XPath定位

使用方法：用class的属性来替代做标签的名字。

```driver.find_element_by.xpath(“//android.view.ViewGroup/android.widget.Button”)```

![image](http://upload-images.jianshu.io/upload_images/7116457-6a4c2aba2e46fd8a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当果如果出现class相同的情况下可以用控件的属性值进行区分。
```
driver.find_element_by_xpath("//android.widget.Button[contains(@text,'7')]").click();

driver.find_element_by_xpath(”//android.widget.Button[contains(@content-desc,’times')]").click();

driver.find_element_by_xpath("//android.widget.Button[contains(@text,'7')]").click();

driver.ffind_element_by_xpath("//android.widget.Button[contains(@content-desc,'equals')]").click();
```
XPath在Appium上的用法依然很强大，有时需要写更臭更长的定位语法，因为APP上元素的class命令本来就长，再加上多层级，结果可想而知。

5、Accessibility ID定位

使用方法：其实，我们的核心是要找到元素的contentDescription属性。它就是元素的content-desc。

```driver.find_element_by_accessibility_id("plus").click();```

![image](http://upload-images.jianshu.io/upload_images/7116457-db1ab87f3cd52500?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6、android uiautomator定位

使用方法：

一个元素的任意属性都可以通过android uiautomator方法来进行定位，但要保证这种定位方式的唯一性。

```driver.find_element_by_android_uiautomator("new UiSelector().text(\”8\")").click();```

```driver.find_element_by_android_uiautomator("new UiSelector().description(\”plus\")").click();```

![image](http://upload-images.jianshu.io/upload_images/7116457-e3c441a0b172cbe1?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* * *

#### 四、应用操作

**1、安装应用**

```install_app()```

安装应用到设备中去。需要apk包的路径。

**2、卸载应用**

```remove_app()```

从设备中删除一个应用。

**3、关闭应用**

```close_app()```

关闭打开的应用，默认关闭当前打开的应用，所以不需要入参。这个方法并非真正的关闭应用，相当于按home键将应用置于后台，可以通过launchApp()再次启动。

**4、启动应用**

```launch_app()```

启动应用。你一定很迷惑，不是在初始化的配置信息已经指定了应用，脚本运行的时候就需要启动应用，为什么还要有这个方法去启动应用呢？重新启动应用也是一个测试点，该方法需要配合closeApp()使用的。

**5、检查应用是否安装**

```is_app_installed()```

检查应用是否已经安装。需要传参应用包的名字。返回结果为Ture或False。

**6、将应用置于后台**

```background_app()```

将当前活跃的应用程序发送到后台。这个方法需要入参，需要指定应用置于后台的时长。

**7、应用重置**

```reset()```

重置当前被测程序到出始化状态。该方法不需要入参。

* * *

#### 五、键盘操作

1、SendKeys()方法

```driver.find_element_by_name(“Name”).send_keys("jack");```

2、PressKeyCode()方法

除此之外，Appium扩展提供了pressKeyCode()方法。该方法Android特有。

发送一个键码的操作。（键码对照表请自行百度，此处不展示了。）

```driver.press_keycode(3)//点击Android的HOME键```

```driver.press_keycode(27)//点击拍照键```

3、输入法问题：

必须使用appium自带键盘，并添加：

```desired_caps["unicodeKeyboard"] = "True"```

```desired_caps["resetKeyboard"] = "True"```

* * *

#### 六、TouchAction操作

Appium的辅助类，主要针对手势操作，比如滑动、长按、拖动等。

1、按压控件press()

开始按压一个元素或坐标点（x,y）。通过手指按压手机屏幕的某个位置。

```press(WebElement el, int x, int y)```

2、长按控件longPress()

开始按压一个元素或坐标点（x,y）。相比press()方法，longPress()多了一个入参，既然长按，得有按的时间吧。duration以毫秒为单位。1000表示按一秒钟。其用法与press()方法相同。

```longPress(WebElement el, int x, int y, Duration duration)```

3、点击控件tap()

对一个元素或控件执行点击操作。用法参考press()。

```tap(WebElement el, int x, int y)```

4、移动moveTo()

将指针（光标）从过去指向指定的元素或点。

```moveTo(WebElement el, int x, int y)```

5、暂停wait()

暂停脚本的执行，单位为毫秒。

```action.wait(1000);```

* * *

#### 七、其他操作

其它操作针对移动设备上特有的一些操作。

**1、熄屏**

方法：```lock()```

点击电源键熄灭屏幕。[iOS专有,可以设置熄屏时间]

```driver.lock(1000);// iOS```


**2、收起键盘**

方法：```hide_keyboard()```

收起键盘，这个方法很有用，当我们对一个输入框输入完成后，需要将键盘收起，再切换到一下输入框进行输入。

```driver.hide_keyboard();//收起键盘```

**3、滑动**

方法：```swipe()```

模拟用户滑动。将控件或元素从一个位置（x,y）拖动到另一个位置（x,y）。
```
swipe(int startx, int starty, int endx, int endy, int duration)

* start_x：开始滑动的x坐标。* start_y：开始滑动的y坐标。

* end_x：结束滑动的x坐标。* end_y：结束滑动的y坐标。

* duration：持续时间。

例：driver.swipe(75, 500, 75, 0, 800);
```
**4、截屏**

方法：```get_screenshot_as_file()```

用法：```driver.get_screenshot_as_file('../screenshot/foo.png')```,参数为保存的图片路径和名称

5、获取控件各种属性

方法：```get_attribute()```

用法:
```
driver.find_element_by_id().get_attribute(name)，

name即是左侧的标志(class，package，checkable，checked....)，

可获取的字符串类型：

name(返回content-desc或text)

text(返回text)

className(返回class，只有API=>18才能支持)

resourceId(返回resource-id，只有API=>18才能支持)
```

* * *

#### 八、unittest之断言

**在unittest单元测试框架中，TestCase类提供了一些方法来检查并报告故障：**

>assertEqual(first, second, msg=None)

判断first和second的值是否相等，如果不相等则测试失败，msg用于定义失败后所抛出的异常信息。

>assertNotEqual(first, second, msg=None)

测试first和second不相等，如果相等，则测试失败。

>assertTure(expr,msg=None)

>assertFalse(expr,msg=None)

测试expr为Ture（或为False）

>>assertIs(first, second, msg=None)

>>assertIsNot(first, second, msg=None)

测试的first和second是（或不是）相同的对象。

>>assertIsNone(expr, msg=None)

>>assertIsNotNone(expr, msg=None)

测试expr是（或不是）为None

>>assertIn(first, second, msg=None)

>>assertNotIn(first, second, msg=None)

测试first是（或不是）在second中。second包含是否包含first。

* * *

#### 九、实例代码（例子app：喜马拉雅FM）
```
import os

import unittest

from appium import webdriver

from time import sleep

# Returns abs path relative to this file and not cwd

PATH =lambdap: os.path.abspath(

os.path.join(os.path.dirname(__file__), p)

)

class Contacts Android Tests(unittest.TestCase):

def setUp(self): 

desired_caps = {}

desired_caps['platformName'] ='Android'

desired_caps['platformVersion'] ='6.0.1'

desired_caps['deviceName'] ='e0bbc8b7'

desired_caps['appPackage'] ='com.ximalaya.ting.android'

desired_caps['appActivity'] ='com.ximalaya.ting.android.host.activity.WelComeActivity'

desired_caps["unicodeKeyboard"] ="True"

desired_caps["resetKeyboard"] ="True"

self.driver = webdriver.Remote('http://127.0.0.1:4723/wd/hub', desired_caps)

sleep(6)

def tearDown(self):

self.driver.close_app()

self.driver.quit()

deftest_Login(self):

self.driver.find_element_by_id('com.ximalaya.ting.android:id/tab_myspace').click()

sleep(2)

self.driver.find_element_by_accessibility_id('设置').click()

sleep(2)

width =self.driver.get_window_size()['width']

height =self.driver.get_window_size()['height']

self.driver.swipe(width /2, height *7/8, width /2, height *4/8,1000)

# 不同的手机分辨率不同，所以一个坐标如果用另一个分辨率不同的手机可能位置就有所变化了，为了让apppium 更好的兼容不同分辨率的设备，

# 在执行滑动前先获取屏幕的分辨率。

self.driver.find_element_by_id('com.ximalaya.ting.android:id/main_tv_login').click()

self.driver.find_element_by_id('com.ximalaya.ting.android:id/main_username').send_keys('18500425217')

self.driver.find_element_by_id('com.ximalaya.ting.android:id/main_password').send_keys('wj1234')

self.driver.find_element_by_id('com.ximalaya.ting.android:id/main_login').click()

sleep(4)

text =self.driver.find_element_by_id('com.ximalaya.ting.android:id/tab_myspace').text

self.assertEqual(text,'我的')

deftest_Search(self):

sleep(3)

message =self.driver.find_element_by_id('com.ximalaya.ting.android:id/main_item_finding_title').text

self.assertEqual(message,'天天好书')

self.driver.find_element_by_accessibility_id("搜索").click()

self.driver.find_element_by_id('com.ximalaya.ting.android:id/main_search_et').send_keys('段子')

self.driver.find_element_by_id('com.ximalaya.ting.android:id/main_search_button').click()

self.driver.get_screenshot_as_file('/Users/wangjuan/截图图库／1.jpg')

self.driver.press_keycode(4)

self.assertEqual(message,'天天好书')

if__name__ =='__main__':

suite = unittest.TestLoader().loadTestsFromTestCase(ContactsAndroidTests)

unittest.TextTestRunner(verbosity=2).run(suite)
```

* * *

以上，希望对你有所帮助～～
