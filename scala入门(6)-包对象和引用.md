------
title: scala入门(6)-包对象和引用
categories:
- scala
toc: true
------
```
/* 
 * scala中隐式引入了以下包： 
 * java.lang._ 
 * scala._ 
 * Predef._ 
 */
package com.dt.scala.oop //没有大括号，表示作用域到整个代码块  
  
package com.scala.spark  
  
/* 
 * 在包中可以定义包对象 
 * 包中的所有类都可以访问报对象的成员和方法 
 * 前提是包的名字要和报对象的名字一样 
 */
package object people {
  val defaultName = "Scala"  
}

package people {
  class people {
    var name = defaultName
  }

  class people2 {
    var name2 = defaultName
  }
}  

class people3 {
  //var name3 = defaultName //这里不能访问，people3已经不再people包中
}  

import java.awt.{ Color, Font } //单独引入指定类
import java.util.{ HashMap => JavaHashMap } //重命名：解决java中类和scala中的类的命名冲突
import scala.{ StringBuilder => _ } //隐藏这个类

class PackageOps {}

package spark.navigation {//支持包的嵌套
  abstract class Navigator {
    def act
  }

  //测试代码专门在测试的包里
  package tests {
    // 在spark.navigation.tests包里
    class NavigatorSuite
  }

  package impls {
    class Action extends Navigator {
      def act = println("Action")
    }
  }
}

package hadoop {
  package navigation {
    class Navigator
  }

  package launch {
    class Booster {
      val nav = new navigation.Navigator
    }
  }
}
```