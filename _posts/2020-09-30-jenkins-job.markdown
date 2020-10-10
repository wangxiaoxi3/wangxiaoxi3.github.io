---
layout: post
title: Jenkins - job之间传参
date: 2020-09-30 9:30:00 +0900
category: Jenkins
tag: 持续集成
---

前言：
本文介绍插件： [Parameterized Trigger plugin](http://wiki.jenkins-ci.org/display/JENKINS/Parameterized+Trigger+Plugin)的具体使用方法。

---

#### 一、插件介绍
Parameterized Trigger plugin插件可以让你在构建完成时触发新的Job构建，并以各种方式为新Job构建指定参数。
当然也可以添加多个配置：每个配置都有一个要触发的Job，触发时间的条件（基于当前构建的结果）和参数部分。

---

#### 二、使用方法
1、项目Test_A，配置-构建后操作-配置如下信息:
选择【Trigger parameterized build on other projects】
* 【Projects to build】：项目名称
* 【Trigger when build is】：项目执行状态
* 【Add Parameters】：添加需要传的参数

        >> 1)【Predefined parameters】：预定义参数,后续的job的入参与当前job的参数名称不一致，或者有新参数的时候，可以使用该传参方式。局限：后续job的入参的值要么固定，要么由环境变量和入参进行简单的组合获取，无法融入逻辑语句。

        >> 2)【Current build parameters】：后续job的入参和用到当前job的入参的时候，可是使用该传参方式。局限：参数取决于当前的job

        >> 3)【Parameters from properties file】：若后续job的入参的值需要一定逻辑处理才能获取，那么，这种传参方式就特别好用了，比较灵活。
本次传参例子为：JOB_NAME=${JOB_NAME}![job-name.png](/assets/img/cc/job-name.png)

2、项目Test_B，配置-参数化构建过程-配置如下信息：
选择【参数化构建过程】
> > 【添加参数-字符参数】：选择字符参数
> > 【名称】：输入参数名称JOB_NAME
> >   ![image.png](/assets/img/cc/job-name-1.png)

3、项目Test_B，输出传入的参数验证。
选择【构建-Execute shell】
> > 输出传参：echo ${JOB_NAME}
> > ![image.png](/assets/img/cc/job-name-2.png)

4、执行项目Test_A后，自动执行项目Test_B，并传入参数${JOB_NAME}，查看项目Test_B输出结果。
> 项目Test_B输出结果如下：
>![image.png](/assets/img/cc/job-name-3.png)

---
#### 三、预定义参数
我选择的参数为预定义参数Predefined parameters，那分别有哪些预定义参数可以用呢？

在Build模块下选择Execute shell--the list of available environment variables 选项，可以查看预定义参数信息，列出所有可用的预定义参数：

The following variables are available to shell scripts

**BRANCH_NAME**（（正在构建的分支的名称））
> > For a multibranch project, this will be set to the name of the branch being built, for example in case you wish to deploy to production from master but not from feature branches; if corresponding to some kind of change request, the name is generally arbitrary (refer to CHANGE_ID and CHANGE_TARGET).
>
**CHANGE_ID**
> > For a multibranch project corresponding to some kind of change request, this will be set to the change ID, such as a pull request number, if supported; else unset.
>
**CHANGE_URL**
> > For a multibranch project corresponding to some kind of change request, this will be set to the change URL, if supported; else unset.
>
**CHANGE_TITLE**
> > For a multibranch project corresponding to some kind of change request, this will be set to the title of the change, if supported; else unset.
>
**CHANGE_AUTHOR**
> > For a multibranch project corresponding to some kind of change request, this will be set to the username of the author of the proposed change, if supported; else unset.
>
**CHANGE_AUTHOR_DISPLAY_NAME**
> > For a multibranch project corresponding to some kind of change request, this will be set to the human name of the author, if supported; else unset.
>
**CHANGE_AUTHOR_EMAIL**
> > For a multibranch project corresponding to some kind of change request, this will be set to the email address of the author, if supported; else unset.
>
**CHANGE_TARGET**
> > For a multibranch project corresponding to some kind of change request, this will be set to the target or base branch to which the change could be merged, if supported; else unset.
>
**BUILD_NUMBER**（当前构建的编号）
> > The current build number, such as "153"
>
**BUILD_ID**（当前构建的编号，老的构建会增加时间戳）
> > The current build ID, identical to BUILD_NUMBER for builds created in 1.597+, but a YYYY-MM-DD_hh-mm-ss timestamp for older builds
>
**BUILD_DISPLAY_NAME**（当前构建的显示名）
> > The display name of the current build, which is something like "#153" by default.
>
**JOB_NAME**（当前构建项目的名称）
> > Name of the project of this build, such as "foo" or "foo/bar".
>
**JOB_BASE_NAME**（当前构建项目的短名称）
> > Short Name of the project of this build stripping off folder paths, such as "foo" for "bar/foo".
>
**BUILD_TAG**
> > String of "jenkins-${JOB_NAME}-${BUILD_NUMBER}". All forward slashes ("/") in the JOB_NAME are replaced with dashes ("-"). Convenient to put into a resource file, a jar file, etc for easier identification.
>
**EXECUTOR_NUMBER**
> > The unique number that identifies the current executor (among executors of the same machine) that’s carrying out this build. This is the number you see in the "build executor status", except that the number starts from 0, not 1.
>
**NODE_NAME**
> > Name of the agent if the build is on an agent, or "master" if run on master
>
**NODE_LABELS**
> > Whitespace-separated list of labels that the node is assigned.
>
**WORKSPACE**（构建的目录的绝对路径）
> > The absolute path of the directory assigned to the build as a workspace.
>
**JENKINS_HOME**
> > The absolute path of the directory assigned on the master node for Jenkins to store data.
>
**JENKINS_URL**
> > Full URL of Jenkins, like http://server:port/jenkins/ (note: only available if Jenkins URL set in system configuration)
>
**BUILD_URL**
> > Full URL of this build, like http://server:port/jenkins/job/foo/15/ (Jenkins URL must be set)
>
**JOB_URL**
> > Full URL of this job, like http://server:port/jenkins/job/foo/ (Jenkins URL must be set)
>
**GIT_COMMIT**
> > The commit hash being checked out.
>
**GIT_PREVIOUS_COMMIT**
> > The hash of the commit last built on this branch, if any.
>
**GIT_PREVIOUS_SUCCESSFUL_COMMIT**
> > The hash of the commit last successfully built on this branch, if any.
>
**GIT_BRANCH**
> > The remote branch name, if any.
>
**GIT_LOCAL_BRANCH**
> > The local branch name being checked out, if applicable.
>
**GIT_URL**
> > The remote URL. If there are multiple, will be GIT_URL_1, GIT_URL_2, etc.
>
**GIT_COMMITTER_NAME**
> > The configured Git committer name, if any.
>
**GIT_AUTHOR_NAME**
> > The configured Git author name, if any.
>
**GIT_COMMITTER_EMAIL**
> > The configured Git committer email, if any.
>
**GIT_AUTHOR_EMAIL**
> > The configured Git author email, if any.
>
**SVN_REVISION**
> > Subversion revision number that's currently checked out to the workspace, such as "12345"
>
**SVN_URL**
> > Subversion URL that's currently checked out to the workspace.
>


