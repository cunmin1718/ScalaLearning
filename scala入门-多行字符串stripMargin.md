------
title: scala入门-多行字符串stripMargin
categories:
- scala
toc: true
------
问题描述：
    在Scala代码块中如何创建多行字符串，是否存在类似其他语言的“定界符”语法？
解决方法：
    要在Scala中创建多行字符串，就需要了解Scala的Multiline String。在Scala中，利用三个双引号包围多行字符串就可以实现。

代码实例如：
``` bash
val foo = """This is
a scala multiline
String"""
```
运行结果为：
```
This is
   a scala  multiline
   String
```
但上述方法存在一个缺陷问题就在与每一行可能与我们输入的内容，带有空格之类，导致每一行的开始位置不能整洁对齐。而在实际应用场景下，有时候我们就是确实需要在scala创建多少字符串，但是每一行需要固定对齐。解决该问题的方法就是应用scala的stripMargin方法，在scala中stripMargin默认是“|”作为出来连接符，在多行换行的行头前面加一个“|”符号即可。

