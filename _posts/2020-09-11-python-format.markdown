---
layout: post
title: Python-format格式化函数
date: 2020-09-11 14:30:00 +0900
categories: python
---
1、format 函数可以接受不限个参数，位置可以不按顺序。
```
# 不设置指定位置，按默认顺序
"{} {}".format("hello", "world")
输出结果：
> 'hello world'

# 设置指定位置
"{0} {1}".format("hello", "world")
输出结果：
> 'hello world'

# 设置指定位置
"{1} {0} {1}".format("hello", "world")
输出结果：
> 'world hello world'
```

2、format 函数可以设置参数，还可以使用 *元组 和 **字典 的形式传参，两者可以混合使用。
```
print("网站名：{name}, 地址 {url}".format(name="菜鸟教程", url="www.runoob.com"))

# 通过字典设置参数
site = {"name": "菜鸟教程", "url": "www.runoob.com"}
print("网站名：{name}, 地址 {url}".format(**site))

# 使用元组传参
infos = ['钢铁侠', 66]
print('我是{}，身价{}亿。'.format(*infos)

# 通过列表索引设置参数
my_list = ['菜鸟教程', 'www.runoob.com']
print("网站名：{0[0]}, 地址 {0[1]}".format(my_list))  # "0" 是必须的

# 方括号用法：用列表传递位置参数
infos = ['阿星', 9527]
food = ['霸王花', '爆米花']
print('我叫{0[0]}，警号{0[1]}，爱吃{1[0]}。'.format(infos, food))

输出结果：
> 我叫阿星，警号9527，爱吃霸王花。

# 同时使用元组和字典传参
hulk = '绿巨人', '拳头'
captain = {'name': '美国队长', 'weapon': '盾'}
print('我是{}, 我怕{weapon}。'.format(*hulk, **captain))

输出结果：
> 我是绿巨人, 我怕盾。

```

3、str.format() 格式化数字的多种方法：
```
^, <, > 分别是居中、左对齐、右对齐,后面带宽度;
: 号后面带填充的字符,只能是一个字符,不指定则默认是用空格填充;
+ 表示在正数前显示 + , 负数前显示 - ;
(空格)表示在正数前加空格;

`b`、`d`、`o`、`x`分别是二进制、十进制、八进制、十六进制。
```

![format](/image/python-format-1.png)


---
以上，对你有帮助的话，点赞❤️吧～～
