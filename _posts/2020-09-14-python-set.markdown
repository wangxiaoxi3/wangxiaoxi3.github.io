---
layout: post
title: Python-字典与序列
date: 2020-09-14 14:30:00 +0900
category: Python
---
#### 一、集合（set）和字典（dict）区别：
1、字典是一系列由键key和值value配对组成的元素的集合，在python3.7+，字典被确定为有序。3.6之前是无序的，其长度大小可变，元素可以任意的删减和改变。

2、集合没有键和值的配对，是一系列无序的，唯一的元素组合。

#### 二、创建集合（set）和字典（dict）
```
# 创建字典
d1 = {'name':'jason','age':20,'gender':'male'}
d2 = dict({'name':'jason','age':20,'gender':'male'})
d3 = dict([('name','jason'),('age',20),('gender','male')])
d4 = dict(name='jason',age=20,gender='male')

#创建集合
s1 = {1,2,3}
s2 = set([1,2,3])
```


#### 三、集合（set）操作方法
```
s = {5,1,2,3}
# 判断元素在不在集合内
1 in s
> True
# 添加元素
s.add(4)
> {5,1,2,3,4}
# 删除元素
s.remove(4)
> {5,1,2,3}
# 集合排序
sorted(s)
> [1,2,3,5](排序后变成list)
```


#### 四、字典（dict）操作方法
```
d = {'name':'jason','age':20,'gender':'male'}
# 判断元素在不在集合内
‘name' in d
> True
'male' in d
> True
# 索引键
d['name']
> 'jason'
# get(key, default) 函数进行索引。如果键不存在，调用get()函数可以返回一个默认值
d.get('sex','null')
> 'null'
# 增加元素
d['dob'] = '1999-02-12'
> d = {'name':'jason','age':20,'gender':'male','dob':'1999-02-12'}
# 更新键对应的值
d['name'] = 'makes'
> d = {'name':'makes','age':20,'gender':'male'}
# 删除键对应的值
d.pop['name']
> d = {'age':20,'gender':'male'}
# 根据字典键的升序排序
d = {'b': 1, 'a': 2, 'c': 10}
d_sorted_key = sorted(d.items(),key=lambda x:x[0])
[('a',2),('b',1),('c',10)]
# 根据字典值的升序排序
d = {'b': 1, 'a': 2, 'c': 10}
d_sorted_value = sorted(d.items(),key=lambda x:x[1])
[('b',1),('a',2),('c',10)]
```
