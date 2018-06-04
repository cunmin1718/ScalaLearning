------
title: scala入门(4)-高阶函数
categories:
- scala
toc: true
------
&emsp;&emsp;高阶函数是函数的参数也是函数。（因为函数的参数可以是变量，而函数又可以赋值给变量，即函数和变量地位一样，所以函数参数也可以是函数）

高阶函数的好处之一是它能让你创造控制抽象从而减少代码重复。
``` bash
object FileMatcher {
  private def filesHere = (new java.io.File(".")).listFiles

  def filesEnding(query: String) = {
    for(file <- filesHere; if file.getName.endsWith(query))
      yield file
  }

  def filesContaining(query: String) = {
    for(file <- filesHere; if file.getName.concat(query))
      yield file
  }

  def filesRege(query: String) = {
    for(file <- filesHere; if file.getName.matches(query))
      yield file
  }
}
```
这个版本中使用matcher针对查询检查文件名。更精确的说法是这个检查不依赖于matcher定义了什么。现在看下matcher类型。它是一个函数，
``` bash
def fileMatching(query : String, matcher: (String, String) => Boolean) = {
  for(file <- filesHere; if matcher(file.getName, query))
    yield file
}
```
高阶函数主要有两种：
* 一种是将一个函数当做另外一个函数的参数（即函数参数）；
* 另外一种是返回值是函数的函数。
## 柯里化(currying)
&emsp;&emsp;curring:实际链接两个传统函数，第一个函数调用带单个名为x的参数，并返回第二个函数的函数值，第二个函数带Int参数y
``` bash
def multiple(x: Int, y: Int) = x * y
def multipleOne(x: Int) = (y: Int) => x * y
println(multipleOne(6)(7))
def curring(x: Int)(y: Int) = x * y
println(curring(10)(10))
```