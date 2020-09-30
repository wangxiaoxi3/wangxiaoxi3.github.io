---
layout: post
title: Python-异常总结
date: 2020-09-14 14:30:00 +0900
categories: Python
---

### 一、常见异常总结

| 字符 | 说明 |
| :----: | :----: |
|BaseException|     所有异常的基类|
|SysternExit|    解释器请求退出|
|KeyboardInterrupt  |   用户中断执行|
|Exception|     常规错误的基类|
|Stopiteration|    迭代器没有更多的值|
|GeneratorExit|   生成器发生异常来通知退出|
|StandardError |   所有的内建标准异常的基类|
|ArithmeticError |  所有数值计算错误的基类|
| FloadingPointError |  浮点计算错误|
| OverflowError |  数值运算超出最大限制|
| ZeroDivsionError|  除(或取模)零 (所有数据类型)，进行数学运算时除数为0时会出现此异常|
| AssertionError|    断言语句失败|
| AttributeError|   对象没有这个属性|
| EOFError |  没有内建输入,到达EOF 标记|
| EnvironmentError  | 操作系统错误的基类|
| IOError |  输入/输出操作失败|
| OSError |   操作系统错误|
| WindowsError |  系统调用失败（Only available on Windows.）|
| ImportError|    导入模块/对象失败|
| LookupError |  无效数据查询的基类|
| IndexError|    序列中没有此索引(index)|
| KeyError|    在字典中查找一个不存在的key抛出的异常|
| MemoryError|    内存溢出错误(对于Python 解释器不是致命的)|
| NameError |    未声明/初始化对象 (没有属性)|
| UnboundLocalError |   访问未初始化的本地变量|
| ReferenceError |    弱引用(Weak reference)试图访问已经垃圾回收了的对象|
| RuntimeError |   一般的运行时错误|
| NotlmplementedError |   尚未实现的方法|
| SyntaxError |  语法错误|
| IndentationError|    缩进错误|
| TabError |    和空格混用|
| SysternError|    一般的解释器系统错误|
| TypeError |     对类型无效的操作|
| ValueError |     传入无效的参数|
| UnicodeError |    相关的错误|
| UnicodeDecodeError |   解码时的错误|
| UnicodeEncodeError |    编码时错误|
| UnicodeTranslateError|    转换时错误|
| Warning|    警告的基类|
| DeprecationWarning |   关于被弃用的特征的警告|
| FutureWarning |   关于构造将来语义会有改变的警告|
| OverflowWarning  | 旧的关于自动提升为长整型(long)的警告|
| PendingDeprecationWarning  | 关于特性将会被废弃的警告|
| RuntimeWarning|    可疑的运行时行为(runtime behavior)的警告|
| SyntaxWarning |   可疑的语法的警告|
| UserWarning  |   可疑的语法的警告|

官方doc:  [Built-in Exceptions — Python 3.8.0 documentation](https://docs.python.org/3/library/exceptions.html?highlight=taberror#IndentationError)


### 二、总结
* 异常，通常是指程序运行的过程中遇到了错误，终止并退出。我们通常使用 try except 语句去处理异常，这样程序就不会被终止，仍能继续执行。
* 处理异常时，如果有必须执行的语句，比如文件打开后必须关闭等等，则可以放在 finally block 中。
* 异常处理，通常用在你不确定某段代码能否成功执行，也无法轻易判断的情况下，比如数据库的连接、读取等等。正常的 flow-control 逻辑，不要使用异常处理，直接用条件语句解决就可以了。

