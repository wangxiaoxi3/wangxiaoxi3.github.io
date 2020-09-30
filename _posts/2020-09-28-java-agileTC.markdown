---
layout: post
title: Mac下安装AgileTC
date: 2020-09-28 14:20:23 +0900
category: Java
tag: 用例管理平台
---

### 简介
[AgileTC](https://github.com/didi/AgileTC)是一套敏捷的测试用例管理平台，支持测试用例管理、执行计划管理、进度计算、多人实时协同等能力，方便测试人员对用例进行管理和沉淀。产品以脑图方式编辑可快速上手，用例关联需求形成流程闭环，并支持组件化引用，可在各个平台嵌入使用，是测试人员的贴心助手！

### 环境依赖
* Mac/Linux/Windows 
* Java 1.8 
* Mysql 服务端 
* Maven

### 安装使用
1、下载源码
git clone https://github.com/didi/AgileTC.git 

2、创建依赖数据库，默认数据库名为：case_manager 
执行sql:  `create database case_manager `

3、创建对应数据表，执行sql文件：`/AgileTC/case-server/sql/case-server.sql `

4、修改`/AgileTC/case-server/src/main/resourcesapplication-dev.properties中spring.datasource`的配置
```
#修改对应数据库中端口，数据库名称
spring.datasource.url=jdbc:mysql://127.0.0.1:33306/case_manager?useSSL=false&useUnicode=true&characterEncoding=utf8
#修改对应数据库中用户名
spring.datasource.username=root
#修改对应数据库中密码
spring.datasource.password=123456
```

5、安装xmind.jar包,进入文件夹`/AgileTC/case-server`,执行mvn安装命令
```
 mvn install:install-file -Dfile=org.xmind.core_3.5.2.201505201101.jar
-DgroupId=com.xmind -DartifactId=sdk-Java -Dversion=201505201101
-Dpackaging=jar
```

6、进入文件夹`/AgileTC/case-server`，执行运行命令：`mvn spring-boot:run `

7、运行成功，进入页面：[http://localhost:8094/case/caseList/1](http://localhost:8094/case/caseList/1)
