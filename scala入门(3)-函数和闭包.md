------
title: scala入门(3)-函数和闭包
categories:
- scala
toc: true
------
##  函数方法
Scala函数定义具有以下形式 -
**语法**
``` bash
def functionName ([list of parameters]) : [return type] = {
  function body
  return [expr]
}
```
* 规范写法，scala 函数的返回值是最后一行代码；
``` bash
def max(x: Int, y: Int): Int = {
  if(x >y)
    x
  else
    y
}
```
* 不写明返回值的类型，程序会自行判断，最后一行代码的执行结果为返回值；
``` bash
def max(x: Int, y: Int)  =  {
  if(x >y)
    x
  else
    y
}
```
说明：在递归函数中必须有结果类型
* 如果函数只有一行，可以省略花括号
``` bash
def max(x: Int, y: Int) = if(x >y) x else y
```

### 带默认参数的函数
``` bash
def combine(content: String, left: String = "[", right: String = "]") = left + content + right
println(combine("hello"))
```
### 不定参数
``` bash
def connected(args: Int*) = {
  var result = 0
  for (arg <- args)
    result += arg
  result
}
```
## 本地函数
``` bash
def processData(fileName: String, width: Int) = {
  /**
    * 本地函数也叫内部函数。作用范围只能在函数内部
    * 类似于private做限定
    */
  def processLine(line: String) = {
    if (line.length > width)
      println(fileName + ": " + line)
  }
  val source = Source.fromFile(fileName)
  for (line <- source.getLines)
    processLine(line)
}
```
&emsp;&emsp;&emsp;&emsp;函数processLine为函数processDate的内部函数也为私有函数，外部不可访问，是函数操作的实现即打印满足条件的数据。Source读取文件，调用函数processLine进行函数实现。
## Scala函数字面量
&emsp;&emsp;Scala中函数为头等公民，你不仅可以定义一个函数然后调用它，而且你可以写一个未命名的函数字面量，然后可以把它当成一个值传递到其它函数或是赋值给其它变量。
``` bash
(x :Int, y: Int) => x + y
```
=>指明这个函数把左边的东西转变成右边的东西。
&emsp;&emsp;函数值是对象，所以如果你愿意，可以将其存入变量。它们也是函数，所以你可以使用通常的括号函数调用写法调用它们。
``` bash
var increase = (x :Int ) => x + 1
increase(10)
```
### 做为参数赋值给其它变量
``` bash
val someNumbers = List ( -11, -10, - 5, 0, 5, 10)
someNumbers.foreach((x:Int) => println(x))
```
### Scala函数字面量的短格式
* 去除参数类型
``` bash
someNumbers.foreach((x) => println(x))
```
* Scala允许使用”占位符”下划线”_”来替代一个或多个参数
``` bash
someNumbers.filter(_ >0)
```
## 闭包
&emsp;&emsp;闭包是一个函数，返回值依赖于声明在函数外部的一个或多个变量。
&emsp;&emsp;闭包通常来讲可以简单的认为是可以访问一个函数里面局部变量的另外一个函数。
如下面这段匿名的函数：
``` bash
val multiplier = (i:Int) => i * 10
```
&emsp;&emsp;函数体内有一个变量 i，它作为函数的一个参数。如下面的另一段代码：
``` bash
val multiplier = (i:Int) => i * factor
```
&emsp;&emsp;在 multiplier 中有两个变量：i 和 factor。其中的一个 i 是函数的形式参数，在 multiplier 函数被调用时，i 被赋予一个新的值。然而，factor不是形式参数，而是自由变量，考虑下面代码：
``` bash
var factor = 3
val multiplier = (i:Int) => i * factor
```
&emsp;&emsp;这里我们引入一个自由变量 factor，这个变量定义在函数外面。
&emsp;&emsp;这样定义的函数变量 multiplier 成为一个"闭包"，因为它引用到函数外面定义的变量，定义这个函数的过程是将这个自由变量捕获而构成一个封闭的函数。
``` bash
object Test {
   def main(args: Array[String]) {
      println( "muliplier(1) value = " +  multiplier(1) )
      println( "muliplier(2) value = " +  multiplier(2) )
   }
   var factor = 3
   val multiplier = (i:Int) => i * factor
}
```