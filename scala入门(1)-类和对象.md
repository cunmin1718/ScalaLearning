&emsp;&emsp;类是对象的蓝图(或叫模板)。定义一个类后，可以使用关键字new来创建一个类的对象。 通过对象可以使用定义的类的所有功能。
&emsp;&emsp;类和单例对象间的一个差别是，单例对象不带参数，而类可以。因为你不能用new关键字实例化一个单例对象，你没机会传递给它参数。

## 类的定义
``` bash
class Person {  //默认是public,一个scala源文件里可以有很多类 
  //用val修饰的变量是只读属性，有getter但没有setter
  //（相当与Java中用final修饰的变量）
  val id = "9527"

  //用var修饰的变量既有getter又有setter
  var age: Int = 18

  //类私有字段,只能在类的内部使用
  private var name: String = "唐伯虎"

  //对象私有字段,访问权限更加严格的，Person类的方法只能访问到当前对象的字段
  //（不会生成getter、setter）
  private[this] val pet = "小强"
}
```

## 构造器
主构造器会执行类定义中的所有语句
在主构造器上加入private将不会被调用
``` bash
// class Student private (val name: String, val age: Int){
class Student(val name: String, val age: Int){
  //主构造器会执行类定义中的所有语句
  println("执行主构造器")

  private var gender = "male"

  //用this关键字定义辅助构造器
  def this(name: String, age: Int, gender: String){
    //每个辅助构造器必须以主构造器或其他的辅助构造器的调用开始
    this(name, age)
    println("执行辅助构造器")
    this.gender = gender
  }
}
```
## 内部类
Scala内部类是从属于外部类对象的，可以访问外部类属性
第一种方式  在内部类通过【外部类.this.成员名称】 访问外部类成员
``` bash
class OuterClass(val name:String){
   class InnerClass(val name:String){
      def info = println("Outer name :" + OuterClass.this.name + ",Inner Name :" + name);  
   }
}
```
第二种方式   在内部类通过【外部类别名】 访问外部类成员
``` bash
class OuterClass2(val name:String){
  outer =>
   class InnerClass2(val name:String){
      def info = println("Outer name :" + outer.name + ",Inner Name :" + name);
   }
}
```
调用内部类
内部类只属于当前对象，不能访问其他对象的内部类
``` bash
object OuterAndInnerClassTest {
  def main(args: Array[String]): Unit = {
    println("第一种访问方式：")
    val outer1 = new OuterClass("yy")
    val inner1 = new outer1.InnerClass("xx")
    inner1.info
    println("第二种访问方式：")
    val outer2 = new OuterClass2("yy2")
    val inner2 = new outer2.InnerClass2("xx2")
    inner2.info
  }
}
```

## 对象
### 单例对象
在Scala中没有静态方法和静态字段，但是可以使用object这个语法结构来达到同样的目的
* 存放工具方法和常量
* 高效共享单个不可变的实例
* 单例模式
``` bash
object SingletonDemo {
  def main(args: Array[String]) {
    //单例对象，不需要new，用【类名.方法】调用对象中的方法
    val session = SessionFactory.getSession()
    println(session)
  }
}

object SessionFactory{
  //该部分相当于java中的静态块
  var counts = 5
  val sessions = new ArrayBuffer[Session]()
  while(counts > 0){
    sessions += new Session
    counts -= 1
  }

  //在object中的方法相当于java中的静态方法
  def getSession(): Session ={
    sessions.remove(0)
  }
}
  
class Session{

}
```
### 伴生对象
在Scala的类中，与类名相同的对象叫做伴生对象，类和伴生对象之间可以相互访问私有的方法和属性
伴生对象的好处： 
* 因为只实例化一次，因此节省内存空间 
* 全局性、唯一性 
``` bash
class University {
  //在University类中可以访问伴生对象newStudentNo的私有属性
  val id = University.newStudentNo
  private var number = 0

  def aClass(number: Int) {
    this.number += number
  }
}

/**
  * object 可做为静态方法或变量
  * 只有在第一次被访问时才会被初始化
  */
object University {
//伴生对象中的私有属性
  private var studentNo = 0

  def newStudentNo = {
    studentNo += 1
    studentNo
  }
}

object ObjectOps {
  def main(args: Array[String]): Unit = {
    println(University.newStudentNo)
    println(University.newStudentNo)
    println(University.newStudentNo)
  }
}
```
## apply方法
通常我们会在类的伴生对象中定义apply方法，当遇到类名(参数1,...参数n)时apply方法会被调用
``` bash
class ApplyTest {
  def apply() = println("I am into spark so much!!!")
  def haveTry: Unit = {
    println("Have a try on apply!")
  }
}
object ApplyTest {
  def apply() = {
    println("I am into Scala so much !!!")
    new ApplyTest
  }
}
object ApplyOperation {
  def main(args: Array[String]): Unit = {
    val a = ApplyTest() //这里就是使用object 的使用
    a.haveTry
    a() // 这里就是 class 中 apply使用
  }
}
```
>object apply 是一种比较普遍用法。 主要用来解决复杂对象的初始化问题。同时也是单例