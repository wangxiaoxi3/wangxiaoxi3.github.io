---
layout: post
title: Python-字符串
date: 2020-09-11 14:30:00 +0900
categories: python
---
# Python - 字符串
#### 一、单引号、双引号、三引号区别

1、Python 中单引号、双引号和三引号的字符串是一模一样的，没有区别。
2、双引号中可内嵌带引号的字符串。
3、三引号则主要应用于多行字符串的情景，如函数注释等。



#### 二、转义符

| 转义字符 | 说明 |
| :----: | :----: |
|\newline | 接下一行|
|\\\ | 表示\|
|\\' | 表示单引号|
|\\" | 表示双引号|
|\\n | 换行|
|\\t | 横向制表符|
|\\b | 退格|
|\\v | 纵向制表符|


#### 三、字符串的操作

1、索引和切片：
字符串的索引同样从 0 开始，index=0 表示第一个元素（字符），[index:index+2] 则表示第 index 个元素到 index+1 个元素组成的子字符串。
```
name = 'jason'
name[0]
'j'
name[1:3]
'as'
```

2、遍历：
遍历字符串同样很简单，相当于遍历字符串中的每个字符。
```
for char in name:
    print(char)
j
a
s
o
n
```

3、字符串拼接
使用加法操作符’+=‘的字符串拼接方法。因为它是一个例外，打破了字符串不可变的特性。
```
str1 += str2  # 表示str1 = str1 + str2
```
除了使用加法操作符，我们还可以使用字符串内置的 join 函数。string.join(iterable)，表示把每个元素都按照指定的格式连接起来。
```
l = []
for n in range(0,10000):
	l.append(str(n))
l = ''.join(l)
```

4、字符串分割
string.split(separator)，表示把字符串按照 separator 分割成子字符串，并返回一个分割后子字符串组合的列表。
```
常见的函数还有：
string.strip(str)，表示去掉首尾的 str 字符串；
string.lstrip(str)，表示只去掉开头的 str 字符串；
string.rstrip(str)，表示只去掉尾部的 str 字符串;
```

#### 四、格式化函数
string.format()是最新的字符串格式函数与规范。

```
print('no data available for person with id: {}, name: {}'.format(id, name))
```
