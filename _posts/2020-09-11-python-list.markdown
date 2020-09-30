---
layout: post
title: Python-List操作
date: 2020-09-11 14:30:00 +0900
categories: Python
---
# 前言：
序列是Python中最基本的数据结构。序列中的每个元素都分配一个数字 - 它的位置，或索引，第一个索引是0，第二个索引是1，依此类推。

---
#### 1、创建list
只要把逗号分隔的不同的数据项使用方括号括起来即可。如下所示：
```
list0 = []  # 空列表
list1 = ['phthon', 'java', 1997, 2000]
list2 = [1, 2, 3, 4, 5 ]
list3 = ["a", "b", "c", "d"]
```
与字符串的索引一样，列表索引从0开始。列表可以进行截取、组合等。

----
#### 2、更新list
使用append()方法：用于在列表末尾添加新的对象。如下所示：
```
alist = ['sys']
alist.append('app')
alist.append('web')
print (alist)
# 以上实例输出结果如下：
>>>['sys','app','web']
```
使用insert()方法：用于将指定对象插入列表的指定位置。如下所示：
```
alist = [123, 'xyz', 'zara', 'only']
alist.insert( 3, 2018)
print (alist)
# 以上实例输出结果如下：
>>> [123, 'xyz', 'zara', 2018, 'only']
```
使用extend() 方法：用于在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）。如下所示：
```
alist = [123, 'vm', 'zara', 'only', 123]
blist = [2018, 'mini']
alist.extend(blist)
print (alist)
# 以上实例输出结果如下：
>>>[123, 'vm', 'zara', 'only', 123, 2018, 'mini']
```
----
#### 3、删除list
使用pop() 方法：用于移除列表中的一个元素（默认最后一个元素），并且返回该元素的值。
```
alist = [0, 1, 2]
list_pop1=alist.pop()  # 默认删除list中最后一个元素“2”
list_pop2=alist.pop(1)  # 删除list中指定下标1的元素“1”
print (list_pop2)  # 返回该元素的值
```
使用remove() 方法：用于移除列表中某个值的第一个匹配项。无返回值。
```
alist = [123, 'xyz', 'zara', 'abc', 'xyz']
alist.remove('xyz')
print (alist)
# 以上实例输出结果如下：
>>>[123, 'zara', 'abc', 'xyz']
```
----
#### 4、查看list
使用count() 方法：用于统计某个元素在列表中出现的次数。
```
alist = [123, 'xyz', 'zara', 'abc', 123]
print (alist.count(123))
# 以上实例输出结果如下：
>>>2
```
使用index()方法：用于从列表中找出某个值第一个匹配项的索引位置。
```
alist = [123, 'xyz', 'zara', 'abc', 123]
print (alist.index(123))
# 以上实例输出结果如下：
>>>0
```
使用索引来访问列表中的值。注：切片同样适用于字符串，字符串也有下标
方法|说明
---|---
name[n:m]|切片是不包含后面那个元素的值（顾头不顾尾）
name[:m] |如果切片前面一个值缺省的话，从开头开始取
name[n:] |如果切片后面的值缺省的话，取到末尾
name[:] |如果全部缺省，取全部
name[n:m:s] | 隔`s`个元素取一次; 步长是正数从左往右取; 步长是负数从右往左取
```
alist =  [123, 'xyz', 'zara', 2018, 'only']
alist[0] # 输出结果：123
alist[-2] # 输出结果：2018
alist[1:] # 输出结果：['xyz', 'zara', 2018, 'only']
alist[:3] # 输出结果：[123, 'xyz', 'zara']
alist[1:3] # 输出结果：['xyz', 'zara']
```
----
#### 5、排序和反序
使用sort() 方法：用于对原列表进行排序，如果指定参数，则使用比较函数指定的比较函数。
语法：`list.sort(cmp=None, key=None, reverse=False)`
>参数：
`cmp` ：可选参数, 如果指定了该参数会使用该参数的方法进行排序。
`key`：主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。
`reverse`： 排序规则，reverse = True 降序， reverse = False 升序（默认）。
```
alist = [123, 'xyz', 'zara', 2018, 'only']
alist.sort()  #降序
alist.sort(reverse = True)   #升序
```
使用reverse() 方法：用于反向列表中元素。
```
alist = [123, 'xyz', 'zara', 2018, 'only']
alist.reverse()
print(alist)
#以上实例输出结果如下：
>>>['only', 2018, 'zara', 'xyz', 123]
```
----
#### 6、列表操作的函数
函数|说明
---|---
`len(list)`|列表元素个数
`max(list)`|返回列表元素最大值
`min(list)`|返回列表元素最小值
`list(seq)`|将元组转换为列表

---
以上，对你有帮助的话，点赞❤️吧～～
