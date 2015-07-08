# 使用 import #
和 Java 一样，Scala 也是通过 import 语句引用其它包中定义的类型，类型引入后，可以使用短名称来引用该类型而无需使用全路径。要注意的 Scala 使用“_” 而非”*”作为通配符。

```
//easy access to Fruit
import bobsdelights.Fruit
```

```
//easy access to all members of bobdelights
import bobsdelights._
```

```
//easy access to all member of Fruits
import bobsdelights.Fruits._
```

所定义的类型中包 bobsdelights 中：

```
package bobsdelights
abstract class Fruit(
  val name: String,
  val color:String
)
object Fruits{
  object Apple extends Fruit ("apple","red")
  object Orange extends Fruit("orange","orange")
  object Pear extends Fruit("pear","yellowish")
  val menu=List(Apple,Orange,Pear)
}
```

第一个为引用单个类型，第二个为按需引用，和 Java 不同的是，是使用“_”代替“*”，第三个类似于 Java 中的静态引用，可以直接使用 Fruits 中定义的对象。

此外 Scala 中的 import 语句的使用比较灵感，可以用在代码的任意部分，而不一定需要在文件开头定义。比如下面 import 定义在函数内部：

```
import bobsdelights.Fruit
def showFruit(fruit:Fruit){
  import fruit._
  println(name+"s are" + color)
}
```

方法 showFruit 引入 fruit 对象（非类型）的所有成员，fruit 的类型为 Fruit，因此可以在函数直接使用 fruit 的成员变量，而无需使用 fruit 限定符。这个方法和下面代码是等价的：

```
import bobsdelights.Fruit
def showFruit(fruit:Fruit){
   println(fruit.name+"s are" + fruit.color)
}
```

和 Java 相比，Scala 的 import 的使用更加灵活：


- 可以出现在文件中任何地方
- 可以 import 对象（singleton 或者普通对象）和 package 本身
- 支持对引入的对象重命名或者隐藏某些类型

下面的例子直接引入包名称，而非包中成员，引入包后，可以使用相对名称来指代某个类型（有些类型文件系统的路径）


```
import java.util.regex
class AStarB {
  val pat= regex.Pattern.compile("a*b")
}
```

import 也可以用来重命名或者隐藏某些类型，比如：

```
import Fruits.{Apple,Orange}
```

仅仅引用 Fruits 中的 Apple 和 Orangle 类型。

下面的例子使用=>重命名类型：

```
import Fruits.{Apple=>MaIntosh,Orange}
```

同样重命名也可以重新定义包名称，比如:

```
import java.{sql => S}
```

将引入的包 java.sql 该名为 java.S 因此可以使用 S.Date 来代替 sql.Date。

如果需要隐藏某个类型，可以使用 Type => _ ，将某个类型改名为\_就达到隐藏某个类型的效果，比如:

```
import Fruits.{Apple=>_,_}
```

这个引用，引入 Fruits 中除 Apple 之外的其它类型。
