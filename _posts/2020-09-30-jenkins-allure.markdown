---
layout: post
title: Jenkins - 构建Allure Report
date: 2020-09-30 10:30:00 +0900
category: Jenkins
tag: Jenkins
---

### 前言
本文为[Pytest+Allure定制报告](https://wangxiaoxi.cn/posts/pytest-allure/)进阶篇，集成Jenkins，在Jenkins中直接生成报告，更方便测试人员查看。

#### 一、安装插件
插件官方地址：[allure-jenkins-plugin](https://plugins.jenkins.io/allure-jenkins-plugin)
> 1、进入系统管理 - 管理插件\
> 2、搜索Allure，并进行安装，重启Jenkins\
> 3、进入系统管理 - 全局工具配置 - Allure Commandline\
> 4、点击 Allure Commandline安装，如下图：其中name可随便定义，作为标识用。


#### 二、构建配置
> 1、General - 参数化构建过程 处增加参数ALLURE_HOME，参数值填写存放allure results的默认路径。

> 2、构建 - Execute shell 处增加命令 --alluredir ${ALLURE_HOME}

> 3、构建后操作 - Allure Report ，Results Path处设置${ALLURE_HOME}

#### 三、查看报告
构建已配置好的工程，即可查看Allure Report，有多处入口，点击任意入口即可查看Report
