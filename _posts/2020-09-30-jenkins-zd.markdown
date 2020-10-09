---
layout: post
title: Jenkins - 构建自动化任务
date: 2020-09-30 10:30:00 +0900
category: Jenkins
tag: 持续集成
---
## 前言：
Jenkins是一个开源软件项目，是基于Java开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

---
#### 一、环境配置
1、切换到jenkins.jar存放的目录，输入如下命令：\
$ java -jar jenkins.war\
如果需要修改端口可以使用如下命令：\
$ java -jar jenkins.war --httpPort=8081\
然后在浏览器中输入[localhost:8080](localhost:8080)，localhost可以是本机的ip，也可以是计算机名。就可以打开jenkins。

2、用tomcat打开\
解压tomcat到某个目录,如/usr/local，进入tomcat下的/bin目录，启动tomcat\
将jenkins.war文件放入tomcat下的webapps目录下，启动tomcat时，会自动在webapps目录下建立jenkins目录，在地址栏上需要输入[localhost:8080/jenkins](localhost:8080/jenkins)

---
#### 二、新建任务
登入Jenkins后，左侧视图功能列表中，点击新建任务：![image.png](/assets/img/cc/jenkins.png)

进入创建页面，输入任务名称，选择【构建一个自由风格的软件项目】，点击确定按钮。![image.png](/assets/img/cc/jenkins-1.png)

---
#### 三、项目配置
1、General部分可配置【丢弃旧的构建】，也可忽略，本次设置为保持构建的天数7天，保持构建的最大个数20。![image.png](/assets/img/cc/jenkins-2.png)
2、源码管理选择【Git】，这个时候添加Repository URL之后，下边会报错，显示让去认证，认证即可（其他的安装中又遇到这个问题），
如果认证失败，请下载认证Github Authentication plugin插件，这个在插件管理的可选插件中搜索安装。
![image.png](/assets/img/cc/jenkins-3.png)![image.png](/assets/img/cc/jenkins-4.png)
3、构建触发器，选择Build periodically，即配置项目的定时执行。\
本次设置为【H/15 * * * *】，即为每15分钟执行一次。输入框下方会显示本次执行时间和下一次执行时间。![image.png](/assets/img/cc/jenkins-5.png)
此处定时任务的格式遵循 cron 的语法（可以与 cron 的语法有轻微的差异）\
具体格式，每行包含五个字段，通过 Tab 或空格分隔。![image.png](/assets/img/cc/jenkins-6.png)
若要指定一个字段的多个值，可以使用以下运算符，按先后顺序。
* 指定所有值
* M-N 指定范围值
* M-N/X 或 */X 在指定范围或整个有效范围内按 X 间隔的步骤
* A,B,...,Z 列举了多个值

```
每两小时一次，每个工作日上午9点到下午5点
H H(9-16)/2 * * 1-5
```
```
除12月外，每月1号和15号每天一次
H H 1,15 1-11 *
```

4、构建，选择增加构建步骤【Execute shell】，输入需要执行的shell语句。![image.png](/assets/img/cc/jenkins-7.png)
5、构建后操作，选择邮件通知【E-mail Notification】，配置收件人的邮箱。![image.png](/assets/img/cc/jenkins-8.png)

---
#### 四、立即构建
成功创建项目后，进入该项目详情页，点击立即构建。在Build History列表中可看到构建历史。![image.png](/assets/img/cc/jenkins-9.png)

---
#### 五、邮件配置
进入系统管理-系统设置-邮件通知部分
![image.png](/assets/img/cc/jenkins-10.png)
![image.png](/assets/img/cc/jenkins-11.png)

1、如果设置QQ邮箱的话，密码必须为授权码，方法为：登录QQ邮箱，在“帐户”里开启“POP3/SMTP”并获取授权码。（否则报错535）\
2、必须勾选【使用SMTP认证】\
3、用户名必须与系统管理员邮件地址保持一致。（否则报错501）\
4、设置接收人（Recipients）,多个接收人时用英文空格分隔。\
5、勾选【通过发送测试邮件测试配置】，可验证邮箱配置。


