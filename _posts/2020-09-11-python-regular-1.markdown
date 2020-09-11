---
layout: post
title: Python-正则表达式一
date: 2020-09-11 14:30:00 +0900
categories: python
---
## 一、简单介绍
[在线正则表达式测试工具](http://tool.oschina.net/regex/)

>通配符(.)
正则表达式可以匹配多于一个的字符串，通配符（.）可以匹配任何除换行符“\n”外的字符。
例：待匹配文本：`school`
正则表达式：`s…l`
匹配结果：`school`

>转义符(\)
转义字符，使用后使一个字符改变原来的意思，如果字符串中有字符需要匹配，可以使用（\字符）来表达。
例：待匹配文本：`W3.school`
正则表达式：`W3\.school`
匹配结果：`W3.school`

>字符集（[ ]）
我们可以使用中括号（[ ]）括住字符串来创建字符集。可以使用范围，比如‘[a-z]’能够匹配a到z的任意一个字符；还可以通过一个接一个的方式将范围联合起来使用，比如‘[a-zA-Z0-9]’能够匹配任意大小写字母和数字。
例：待匹配文本：`W3school`
正则表达式：`[a-z]`
匹配结果：`school`
正则表达式：`[0-9]`
匹配结果：`3`
正则表达式：`[A-Z]`
匹配结果：`W`

>反转字符集（[^ ]）
可以在开头使用^字符，比如‘[^abc]’可以匹配任何除了a、b、c之外的字符。
例：待匹配文本：`W3school`
正则表达式：`[^a-z]`
匹配结果：`W3`

>数字（\d）：匹配一个数字字符。等价于 [0-9]。
例：待匹配文本：`W3school`
正则表达式：`\d`
匹配结果：`3`

>非数字（\D）： 匹配一个非数字字符。等价于 [^0-9]。
例：待匹配文本：`W3school`
正则表达式：`\D`
匹配结果：`Wschool`

>空白字符（\s）：  匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。
例：待匹配文本：`W3 school`
正则表达式：`W3\sschool`
匹配结果：`W3 school`

>非空白字符（\S）：  匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。
例：待匹配文本：`W3 school`
正则表达式：`\S`
匹配结果：`W3school`

>任何单词字符（\w）：  匹配包括下划线的任何单词字符。等价于’[A-Za-z0-9_]’。
例：待匹配文本：`W3_school`
正则表达式：`\w`
匹配结果：`W3_school`

>任何非单词字符（\W）：  匹配任何非单词字符。等价于 ‘[^A-Za-z0-9_]’。
例：待匹配文本：`!@#W3school`
正则表达式：`\W`
匹配结果：`!@#`

>(pattern)* :   允许模式重复0次或多次
例：待匹配文本：“www.jianshu.com”
正则表达式：“w*\.jianshu\.com”
匹配结果：”www.jianshu.com”

>(pattern)+ :   允许模式重复1次或多次
例：待匹配文本：`www.jianshu.com`
正则表达式：`w+\.jianshu\.com`
匹配结果: `www.jianshu.com`

>(pattern){m,n} :  允许模式重复m~ n次
例：待匹配文本：`wwwww.jianshu.com`
正则表达式：`w{1,3}\.jianshu\.com`
匹配结果：`www.jianshu.com`

>匹配字符串开头（^）: 在多行模式中匹配每一行的开头
例：待匹配文本：`W3school`
正则表达式：`^W3`
匹配结果：`W3`

>匹配字符串末尾（$）: 在多行模式中匹配每一行的末尾
例：待匹配文本：`W3school`
正则表达式：`ool$`
匹配结果：`ool`

>仅匹配字符串开头（\A）
例：待匹配文本：`W3school`
正则表达式：`\AW3`
匹配结果：`W3`

>仅匹配字符串末尾（\Z）
例：待匹配文本：`W3school`
正则表达式：`ool\Z`
匹配结果：`ool`

>管道符号（|）: 左右表达式任意匹配一个。如果|没有被包括在（）中，则它的范围是整个正则表达式。
例：待匹配文本：`W3school`
正则表达式：`W3|ol`
匹配结果：`W3ol`

>分组（）：被括起来的表达式将作为分组，从表达式左边开始每遇到一个分组的左括号’(‘，编号+1 。另外，分组表达式作为一个整体，可以后接数量词。表达式中的|仅在该组中有效。
例：待匹配文本：`W3schoolW3school`
正则表达式：`(W3school){2}`
匹配结果：`W3schoolW3school`
正则表达式：`W3(sch|ol)`
匹配结果：`W3sch`

>>(?P<name>…) ： 分组，除了原有的编号外在指定一个额外的别名。

>>(?P=<name>)： 引用别名为<name>的分组匹配到的字符串。

## 二、re模块
Python 自1.5版本起增加了re 模块，它提供 Perl 风格的正则表达式模式。re 模块使 Python 语言拥有全部的正则表达式功能。re模块中一些重要的函数，如下：

**1、re.compile(strPattern[, flag])**
这个方法是Pattern类的工厂方法，用于将字符串形式的正则表达式编译为Pattern对象。 第二个参数flag是匹配模式，取值可以使用按位或运算符’|’表示同时生效，
比如`re.I | re.M`。

**2、re.search(pattern, string[, flags]**
这个方法用于查找字符串中可以匹配成功的子串。从string的pos下标处起尝试匹配pattern，如果pattern结束时仍可匹配，则返回一个Match对象；若无法匹配，则将pos加1后重新尝试匹配；直到pos=endpos时仍无法匹配则返回None。pos和endpos的默认值分别为0和len(string))；re.search()无法指定这两个参数，参数flags用于编译pattern时指定匹配模式。
```
import re
pattern = 'I want world peace'
re.search('[a-z]',pattern)
```

**3、re.split(pattern, string[, maxsplit])**
按照能够匹配的子串将string分割后返回列表。maxsplit用于指定最大分割次数，不指定将全部分割。
```
import re
pattern = 'I want world peace'
re.split('\s',pattern)
结果： ['I','want','world','peace']
```

**4、re. findall(pattern, string[, flags])**
搜索string，以列表形式返回全部能匹配的子串。
```
import re
pattern = 'I want world peace'
re.findall('[a-z]',pattern)
结果： ['w','a','n','t','w','o','r','l','d','p','e','a','c','e']
```

**5、re.sub(pattern, repl, string[, count])**
使用给定的替换内容将匹配模式的子符串（最左端并且重叠子字符串）替换掉。
```
import re
pattern = '{name}'
text = '{name} want world peace'
re.sub(pattern,'xiaoxi',text)
结果： 'xiaoxi want world peace'
```

**6、re.escape（string）**
可以对字符串中所有可能被解释为正则运算符的字符进行转义的应用函数。如果字符串很长且包含很多特殊字符，而你又不想输入一大堆反斜线，可以使用这个函数。
```
re.escape('www.jianshu.com')
结果： ‘www\\.jianshu\\.com’
```



