---
layout: post
title:  Pytest+Allure定制报告
date: 2020-09-29 17:30:00 +0900
categories: python
tag: Allure
---

### 前言：
最近在研究接口自动化的框架，好的测试报告在整个测试框架起到至关重要的部分。终于被我发现一个超好用的报告框架,不仅报告美观,而且方便CI集成。
就是它，就是它：[Allure Test Report](http://allure.qatools.ru/)！！！

>先上一张报告效果图：![Allure Test Report.png](/assets/img/cc/AllureTestReport.png)

----

##### python版本及必要库-(2020-4-30)更新

`pytest==5.3.1`\
`allure-pytest==2.8.6`\
`allure-python-commons==2.8.6`\
⚠️注：pytest-allure-adaptor已弃用，改为allure-pytest；\
安装allure-pytest时，需将pytest-allure-adaptor卸载

----

### 一、环境配置
安装Python依赖库：

`pip3 install pytest`
`pip3 install pytest-allure-adaptor`

安装 Command Tool：

`brew tap qatools/formulas`
`brew install allure-commandline`

官方参考文档：[https://pypi.org/project/pytest-allure-adaptor](https://pypi.org/project/pytest-allure-adaptor/)

---

### 二、生成html报告命令
1、pytest命令基础上加--alluredir，生成xml报告。
```
pytest -s -q --alluredir [xml_report_path]
//[xml_report_path]根据自己需要定义文件夹，作者定义为：/report/xml
```
>用例执行完成之后会在[xml_report_path]目录下生成了一堆xml的report文件，当然这不是我们最终想要的美观报告。
![xml.png](/assets/img/cc/report_xml.png)

2、需要使用 Command Tool 来生成我们需要的美观报告。
```
allure generate [xml_report_path] -o [html_report_path]
//[html_report_path]根据自己需要定义文件夹
作者定义为：/report/html
```
>打开index.html,之前写的case报告就会呈现在你面前
![html.png](/assets/img/cc/report_html.png)

注⚠️：直接用chrome浏览器打开报告，报告可能会是空白页面。\
解决办法：\
1、在pycharm中右击index.html选择打开方式Open in Browser就可以了。\
2、使用Firefox直接打开index.html。

----

### 三、定制报告

* Feature: 标注主要功能模块
* Story: 标注Features功能模块下的分支功能
* Severity: 标注测试用例的重要级别
* Step: 标注测试用例的重要步骤
* Issue和TestCase: 标注Issue、Case，可加入URL
* attach: 标注增加附件
* Environment: 标注环境Environment字段

##### 1、Features定制详解
```
# -*- coding: utf-8 -*-
# @Time    : 2018/8/17 上午10:10
# @Author  : WangJuan
# @File    : test_case.py
import allure
import pytest


@allure.feature('test_module_01')
def test_case_01():
    """
    用例描述：Test case 01
    """
    assert 0

@allure.feature('test_module_02')
def test_case_02():
    """
    用例描述：Test case 02
    """
    assert 0 == 0


if __name__ == '__main__':
    pytest.main(['-s', '-q', '--alluredir', './report/xml'])
```
>添加feature，Report展示见下图。![feature.png](/assets/img/cc/feature.png)

##### 2、Story定制详解
```
# -*- coding: utf-8 -*-
# @Time    : 2018/8/17 上午10:10
# @Author  : WangJuan
# @File    : test_case.py
import allure
import pytest


@allure.feature('test_module_01')
@allure.story('test_story_01')
def test_case_01():
    """
    用例描述：Test case 01
    """
    assert 0

@allure.feature('test_module_01')
@allure.story('test_story_02')
def test_case_02():
    """
    用例描述：Test case 02
    """
    assert 0 == 0


if __name__ == '__main__':
    pytest.main(['-s', '-q', '--alluredir', './report/xml'])
```
>添加story，Report展示见下图。![story.png](/assets/img/cc/story.png)

##### 3、用例标题和用例描述定制详解
```
# -*- coding: utf-8 -*-
# @Time    : 2018/8/17 上午10:10
# @Author  : WangJuan
# @File    : test_case.py
import allure
import pytest

@allure.feature('test_module_01')
@allure.story('test_story_01')
#test_case_01为用例title
def test_case_01():
    """
    用例描述：这是用例描述，Test case 01，描述本人
    """
    #注释为用例描述
    assert 0

if __name__ == '__main__':
    pytest.main(['-s', '-q', '--alluredir', './report/xml'])
```
>添加用例标题和用例描述，Report展示见下图。![case.png](/assets/img/cc/case.png)

##### 4 、Severity定制详解
Allure中对严重级别的定义：
* 1、 Blocker级别：中断缺陷（客户端程序无响应，无法执行下一步操作）
* 2、 Critical级别：临界缺陷（ 功能点缺失）
* 3、 Normal级别：普通缺陷（数值计算错误）
* 4、 Minor级别：次要缺陷（界面错误与UI需求不符）
* 5、 Trivial级别：轻微缺陷（必输项无提示，或者提示不规范）

```
# -*- coding: utf-8 -*-
# @Time    : 2018/8/17 上午10:10
# @Author  : WangJuan
# @File    : test_case.py
import allure
import pytest


@allure.feature('test_module_01')
@allure.story('test_story_01')
@allure.severity('blocker')
def test_case_01():
    """
    用例描述：Test case 01
    """
    assert 0

@allure.feature('test_module_01')
@allure.story('test_story_01')
@allure.severity('critical')
def test_case_02():
    """
    用例描述：Test case 02
    """
    assert 0 == 0

@allure.feature('test_module_01')
@allure.story('test_story_02')
@allure.severity('normal')
def test_case_03():
    """
    用例描述：Test case 03
    """
    assert 0

@allure.feature('test_module_01')
@allure.story('test_story_02')
@allure.severity('minor')
def test_case_04():
    """
    用例描述：Test case 04
    """
    assert 0 == 0


if __name__ == '__main__':
    pytest.main(['-s', '-q', '--alluredir', './report/xml'])
```
>添加Severity，Report展示见下图![severity.png](/assets/img/cc/severity.png)

##### 5、Step定制详解
```
# -*- coding: utf-8 -*-
# @Time    : 2018/8/17 上午10:10
# @Author  : WangJuan
# @File    : test_case.py
import allure
import pytest

@allure.step("字符串相加：{0}，{1}")
# 测试步骤，可通过format机制自动获取函数参数
def str_add(str1, str2):
    if not isinstance(str1, str):
        return "%s is not a string" % str1
    if not isinstance(str2, str):
        return "%s is not a string" % str2
    return str1 + str2

@allure.feature('test_module_01')
@allure.story('test_story_01')
@allure.severity('blocker')
def test_case():
    str1 = 'hello'
    str2 = 'world'
    assert str_add(str1, str2) == 'helloworld'


if __name__ == '__main__':
    pytest.main(['-s', '-q', '--alluredir', './report/xml'])

```
>添加Step，Report展示见下图![step.png](/assets/img/cc/step.png)

##### 6、Issue和TestCase定制详解
```
# -*- coding: utf-8 -*-
# @Time    : 2018/8/17 上午10:10
# @Author  : WangJuan
# @File    : test_case.py
import allure
import pytest


@allure.step("字符串相加：{0}，{1}")     # 测试步骤，可通过format机制自动获取函数参数
def str_add(str1, str2):
    print('hello')
    if not isinstance(str1, str):
        return "%s is not a string" % str1
    if not isinstance(str2, str):
        return "%s is not a string" % str2
    return str1 + str2

@allure.feature('test_module_01')
@allure.story('test_story_01')
@allure.severity('blocker')
@allure.issue("http://www.baidu.com")
@allure.testcase("http://www.testlink.com")
def test_case():
    str1 = 'hello'
    str2 = 'world'
    assert str_add(str1, str2) == 'helloworld'


if __name__ == '__main__':
    pytest.main(['-s', '-q', '--alluredir', './report/xml'])
```
>添加Issue和TestCase，Report展示见下图。![issue.png](/assets/img/cc/issue.png)

##### 7、Environment定制详解
```
# 具体Environment参数可自行设置
allure.environment(app_package='com.mobile.fm')
allure.environment(app_activity='com.mobile.fm.activity')
allure.environment(device_name='aad464')
allure.environment(platform_name='Android')
```
>添加Environment参数，Report展示见下图![environment.png](/assets/img/cc/environment.png)

##### 8、attach定制详解
```
 file = open('../test.png', 'rb').read()
 allure.attach('test_img', file, allure.attach_type.PNG)
```

>在报告中增加附件：`allure.attach(’arg1’,’arg2’,’arg3’)`

`arg1`：是在报告中显示的附件名称\
`arg2`：表示添加附件的内容\
`arg3`：表示添加的类型(支持:`HTML,JPG,PNG,JSON,OTHER,TEXTXML`)\

>添加attach参数，Report展示见下图![attach.png](/assets/img/cc/attach.png)

----

此外，Allure还支持Jenkins Plugin～
