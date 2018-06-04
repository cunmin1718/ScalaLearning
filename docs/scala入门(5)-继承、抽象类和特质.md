## 继承
&emsp;&emsp;继承是面向对象的概念，用于代码的可重用性。可以通过使用extends关键字来实现继承。 子类继承父类时，必须把父类的主构造器填充满。
```
class Persion1(val name: String, var age: Int) {
  println("The primary constructor of Person")
  val school = "BJ"
  def sleep = "8 hours"
  override def toString = "I am a Persion1"
}
class Worker(name: String, age: Int, val salary: Long) extends Persion1(name, age) {
  println("This is the subClass of Person")
  override val school = "Spark"
  override def toString = "I am a Worker " + super.sleep
}
object OverrideOpertions {
  def main(args: Array[String]): Unit = {
    val w = new Worker("Spark3", 5, 10000)
    println(w.school)
    println(w.name)
    println(w.salary)
    println(w.toString)
  }
}
```
## 抽象
* 抽象类不能被实例化，包含若干定义不完全的方法，具体的实现由子类去实现。
* 创建抽象类 使用abstract关键字
* 重写超类的抽象方法时，不需要使用override关键字。
```
abstract class SupperTeacher(val name: String) {
  var id: Int
  var age: Int
  def teach
}

class TeacherForMaths(name: String) extends SupperTeacher(name) {
  override var id = name.hashCode
  override var age = 29

  override def teach {println("Teach!!!")}
}
```
## trait
&emsp;&emsp;在Scala中，Trait是一种特殊概念。首先，Trait可以被作为接口来使用，此时Trait与Java的接口非常类似。同时在Trait可以定义抽象方法，其与抽象类中的抽象方法一样，不给出方法的具体实现。 
>注意：类使用extends继承Trait，与Java不同，这里不是implement，在Scala中，无论继承类还是继承Trait都是用extends关键字。

* 在Scala中，类继承Trait后，必须实现其中的抽象方法，实现时不需要使用override关键字，同时Scala同Java一样，不支持类多继承，但支持多重继承Trait，使用with关键字即可。
```
trait HelloTrait {
  def sayHello(name: String)
}

trait MakeFriendTrait {
  def makeFriends(friend: String)
}

class Person(val name: String) extends HelloTrait with  MakeFriendTrait {
  def sayHello(name: String): Unit = println("hello" + name)
  def makeFriends(friend: String): Unit = println("hello" + name)
}
```
* 在Trait中定义具体方法：通俗来讲，就是trait可以包含一些很多类都通用的功能，如打印日志等，在Spark中也使用Trait定义了通用的日志打印方法。也就是说Scala中的Trait不只定义抽象方法，还可以定义具体方法，也有的说法是Trait的功能混入了类。 
```
trait Logger {
    def log(msg: String) {}
}
class ConcreateLogger extends Logger {
  def save {
      log("1111")
  }
}
```
* 在Trait中定义具体字段：在Scala中，Trait可以定义具体字段，继承Trait的类就自动获取了Trait中定义的类。 
注意：这里与继承Class不同，如果继承Class获取的字段，实际定义在父类中，而继承Trait获取的字段，就直接添加到了类中。
```
trait A {
    val eyeNum: Int = 2
}
class B extends A {
  def sayHello = println("Hello," + eyeNum)
}
```
* 在Trait中可以定义抽象字段，而Trait中的具体方法可以基于抽象字段来编写，但继承Trait的类，则必须覆盖抽象的field，提供具体的值。 
```
trait A {
  val message: String
  def say = print()
}

class B extends  A {
  def make: Unit = {
    say
  }

  val message: String = "hello"
}
```

## Trait构造机制
&emsp;&emsp;在Scala中，Trait是有构造代码的，就是Trait中不包含在任何方法中的代码，而继承了Trait的构造机制如下： 
* 父类的构造函数 
* Trait的构造代码执行，多个Trait从左向右依次执行 
* 构造Trait时会先构造父Trait，如果多个Trait继承同一个父Trait，则父Trait只会构造一次 
* 所有trait构造结束之后，子类的构造函数执行 


## 什么时候应该使用特质而不是抽象类？ 

&emsp;&emsp;如果你想定义一个类似接口的类型，你可能会在特质和抽象类之间难以取舍。这两种形式都可以让你定义一个类型的一些行为，并要求继承者定义一些其他行为。一些经验法则：

>优先使用特质。一个类扩展多个特质是很方便的，但却只能扩展一个抽象类。
>如果你需要构造函数参数，使用抽象类。因为抽象类可以定义带参数的构造函数，而特质不行。例如，你不能说trait t(i: Int) {}，参数i是非法的。

