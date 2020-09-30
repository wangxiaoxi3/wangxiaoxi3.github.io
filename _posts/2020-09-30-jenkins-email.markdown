---
layout: post
title: Jenkins - Extended E-mail配置
date: 2020-09-30 10:30:00 +0900
categories: Jenkins
tag: 持续集成
---
前言：
在Jenkins的使用中邮件提醒是一个常用功能，[Extended E-mail Notification](https://wiki.jenkins.io/display/JENKINS/Email-ext+plugin)是一个功能更为齐全，使用也更为复杂的插件，本文即将为大家详细讲解如何配置相关内容，感兴趣的话继续往下看吧！～～

#### 一、全局设置
进入系统管理- 系统设置 - Extended E-mail Notification\
⚠️注意事项：\
1)如果设置QQ邮箱的话，密码必须为授权码，方法为：登录QQ邮箱，在“帐户”里开启“POP3/SMTP”并获取授权码。（否则报错535）\
2)必须勾选【Use SMTP Authentication】【Use SSL】\
3)用户名必须与系统管理员邮件地址保持一致。（否则报错501）\
4)设置接收人（Recipients）,多个接收人时用英文空格分隔。\
1、SMTP基础设置，见下图：
![image.png](/assets/img/cc/jenkins-emali-1.png)
![image.png](/assets/img/cc/jenkins-emali-2.png)
2、拓展设置，见下图：
![image.png](/assets/img/cc/jenkins-emali-3.png)
![image.png](/assets/img/cc/jenkins-emali-4.png)
![image.png](/assets/img/cc/jenkins-emali-5.png)
设置好以上内容后，点击保存。即全局设置完成～～

#### 二、Job中使用Extended E-mail
1、在Job的“构建后操作”中选择“Editable Email Notification”选项即可使用Extended E-mail Notification插件。
![image.png](/assets/img/cc/jenkins-emali-6.png)
![image.png](/assets/img/cc/jenkins-emali-7.png)
![image.png](/assets/img/cc/jenkins-emali-8.png)
设置好以上内容后，点击保存。即Job中使用Extended E-mail设置完成～～

#### 三、Default Content
以下为我的Default Content，仅供参考：
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>${PROJECT_NAME}-第${BUILD_NUMBER}次构建日志</title>
</head>

<body leftmargin="8" marginwidth="0" topmargin="8" marginheight="4"
    offset="0">
    <table width="95%" cellpadding="0" cellspacing="0"
        style="font-size: 11pt; font-family: Tahoma, Arial, Helvetica, sans-serif">
        <tr>
            <td>(本邮件是程序自动下发的，请勿回复！)</td>
        </tr>
        <tr>
            <td><br />
            <b><font color="#0B610B">构建信息($BUILD_STATUS)</font></b>
            <hr size="2" width="100%" align="center" /></td>
        </tr>
        <tr>
            <td>
                <ul>
                    <li>项目名称 ： ${PROJECT_NAME}</li>
                    <li>构建编号 ： 第${BUILD_NUMBER}次构建</li>
                    <li>触发原因 ： ${CAUSE}</li>
                    <li>构建日志 ： <a href="${BUILD_URL}console">${BUILD_URL}console</a></li>
                    <li>工作目录 ： <a href="${PROJECT_URL}ws">${PROJECT_URL}ws</a></li>
                    <li>Allure Report ： <a href="${BUILD_URL}allure">${BUILD_URL}allure</a></li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><b><font color="#0B610B">构建日志(最后100行):</font></b>
            <hr size="2" width="100%" align="center" /></td>
        </tr>
        <tr>
            <td><textarea cols="80" rows="30" readonly="readonly"
                    style="font-family: Courier New">${BUILD_LOG, maxLines=100}</textarea>
            </td>
        </tr>
    </table>
</body>
</html>
```
