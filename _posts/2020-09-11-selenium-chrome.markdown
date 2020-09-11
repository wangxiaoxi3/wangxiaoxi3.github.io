---
layout: post
title: Mac搭建Selenium+ChromeDriver
date: 2020-09-11 14:20:23 +0900
category: selenium
---
### 一、安装Selenium
selenium可以直接用pip安装:   `pip3 install selenium`

### 二、安装ChromeDriver
chromedriver的版本一定要与Chrome的版本一致，不然就不起作用。

下载地址：

1、*[http://chromedriver.storage.googleapis.com/index.html](http://chromedriver.storage.googleapis.com/index.html)*

2、*[https://npm.taobao.org/mirrors/chromedriver/](https://npm.taobao.org/mirrors/chromedriver/)*

下载后，将安装包加入到环境变量。以mac系统为例，将chromedriver移至/usr/bin目录下即可。
```
sudo mv ~/Downloads/chromedriver /usr/bin
```
验证安装:
```
chromedriver
Starting ChromeDriver 80.0.3987.16 (320f6526c1632ad4f205ebce69b99a062ed78647-refs/branch-heads/3987@{#185}) on port 9515
Only local connections are allowed.
Please protect ports used by ChromeDriver and related test frameworks to prevent access by malicious code.
```
代码测试能否调用chrome：
```
from selenium import webdriver
browser = webdriver.Chrome()
browser.get('https://www.baidu.com')
print browser.title
browser.quit()
```
如果能调用chrome浏览器，即表示安装成功![chromedriver](/image/chromedriver.png)


### 三、遇到问题
1、将chromedriver移至/usr/bin目录下报错：`Operation not permitted`

解决方案：

>1.关闭mac的安全机制，首先可以在正常模式下，输入 csrutil status 命令，查看mac安全机制是否开启。

>2.如果 Protection status： enabled 则要进入安全模式进行关闭。

>3.进行安全模式操作: 点击屏幕左上角苹果图标，点击重新启动按钮，屏幕暗下后立马按住command + R键，直到出现屏幕中央出现苹果图标停手。

>4.进入安全模式界面后会看到安全界面操作，屏幕最上面一排，找到实用工具菜单，再在里面找到终端，点击后输入 csrutil disable 回车后会出现一串英文，大致意思是安全模式已经关闭，重启后生效进行操作。然后输入 reboot 重启即可。

>5.重启后在terminal终端中输入 csrutil status 会看到
Protection status：disable .意思是安全模式的状态：是关闭的。


----
以上，喜欢的话请点赞❤️吧～
