# 包对象 #

到目前为止，我们看到的添加到包的都是类型，Trait 和单例对象(Object)。这些都是指包的定级层次定义的类型。Scala 的定级层次除了可以定义类，Trait，Object 之外，其它可以在类，Trait，Object 内部定义的类型，也都可以直接定义在包中，比如一些通用的函数，变量，你都可以直接定义在包中。

在 Scala 中，可以把这些函数或方法放在一个称为“包对象”中。每个包只有一个包对象，任何放在包对象的类型都可以认为是包自身的成员。
 
例如：

```
//in file bobsdelights/package.scala
package object bobsdelights{
  def showFruit(fruit: Fruit){
    import fruit._
    println(name + "s are " + color)
  }
}
//in file PrintMenu.scala
package printmenu
import bobsdelights.Fruits
import bobsdelights.showFruit
object PrintMenu{
  def main(args:Array[String]){
    for(fruit <- Fruits.menu){
      showFruit(fruit)
    }
  }
}
```

本例中对象 PrintMenu 可以引入包对象中定义的函数 showFruit，方法和引入一个类定义一样，也是通过 import 语句。

包对象通常被编译为 package.class，其包名为定义的包。所有按照惯例一般包对象定义放在 package.scala 中，比如上面的包对象可以放在 bobsdelights 目录下的 package.scala 中。
