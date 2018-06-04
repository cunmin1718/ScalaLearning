------
title: scala入门(2)-中缀表示法和后缀表示法
categories:
- scala
toc: true
------
## 中缀表示法
&emsp;&emsp;这两个语法规则都是针对方法（method）来说的，所以在开始，我们创建两个类，然后添加方法：
``` bash
class Liquor(name: String) {
  def mix(rho: Liquor): String = {
    this.name + " and " + rho.getName
  }
  def getName(): String = this.name
}

object Bartender {
  def main(args: Array[String]): Unit = {
    val gin: Liquor = new Liquor("Gin")
    val tonic: Liquor = new Liquor("Tonic")
    val mixed = gin.mix(tonic)

    println(mixed)
  }
}
```
&emsp;&emsp;我们注意到mix只有一个参数，对于只有一个参数的方法来说，Scala提供了中缀表示法来简化对这种方法的调用语法，我们对以上代码修改，得到：
```  bash
class Liquor(name: String) {
  def mix(rho: Liquor): String = {
    this.name + " and " + rho.getName
  }
  def getName(): String = this.name
}

object Bartender {
  def main(args: Array[String]): Unit = {
    println(new Liquor("Gin") mix new Liquor("Tonic"))
  }
}
```
&emsp;&emsp;我想你已经注意到了，在mix方法的调用操作符（.）和包围参数的小括号都可以省略掉。这就是中缀表示法
>1. 方法的参数只有1个（Arity-1）
>2. 调用时省略调用操作符和包围参数的括号

&emsp;&emsp;往深处想一想，以下这段代码等同于什么呢？
``` bash
object Add {
  def main(args: Array[String]) = {
    println(1 + 2)
    println((1).+(2))
  }
}
```
## 后缀表达式
&emsp;&emsp;那么后缀表达式呢？与中缀表达式类似，后缀表达式适用于无参数的方法（Arity-0），下面是一个例子：
``` bash
class Liquor(name: String) {
  def getName() = this.name
}

object Waiter {
  def main(args: Array[String]) = {
    val whisky = new Liquor("Whisky")
    println(whisky.getName())
  }
}
```
使用后缀表达式后，代码如下：
``` bash
class Liquor(name: String) {
  def getName() = this.name
}

object Waiter {
  def main(args: Array[String]) = {
    println(new Liquor("Whisky") getName)
  }
}
```
>注3：当一个方法的参数个数为0时，在方法声明时可以省略方法名称后的括号。如果声明时不省略括号，调用时允许以带括号或者省略括号的形式调用；如果声明时省略了括号，则调用时只允许不带括号的形式。