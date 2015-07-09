# Trait 的基本概念 #

在 Scala中Trait 为重用代码的一个基本单位。一个 Traits 封装了方法和变量，和 Interface 相比，它的方法可以有实现，这一点有点和抽象类定义类似。但和类继承不同的是，Scala 中类继承为单一继承，也就是说子类只能有一个父类。当一个类可以和多个 Trait 混合，这些 Trait 定义的成员变量和方法也就变成了该类的成员变量和方法，由此可以看出 Trait 集合了 Interface 和抽象类的优点，同时又没有破坏单一继承的原则。

下面我们来看看Trait的基本用法：

定义一个 Trait 的方法和定义一个类的方法非常类似，除了它使用 trait 而非 class 关键字来定义一个 trait。

```
trait Philosophical{
  def philosophize() {
    println("I consume memeory, therefor I am!")
  }
}
```

这个 Trait 名为 Philosophical。它没有声明基类，因此和类一样，有个缺省的基类 AnyRef。它定义了一个方法，叫做 philosophize。这是个简单的 Trait，仅够说明 Trait 如何工作。

一但定义好 Trait，它就可以用来和一个类混合，这可以使用 extends 或 with 来混合一个 trait。例如：

```
class Frog extends Philosophical{
  override def toString="gree"
}
```

这里我们使用 extends 为 Frog 添加 Philosophical Trait 属性，因此 Frog 缺省继承自 Philosophical 的父类 AnyRef，这样 Frog 类也具有了 Philosophical 的性质（因此 Trait 也可以翻译成特质，但后面我们还是继续使用 Trait 原文）。

```
scala> val frog = new Frog
frog: Frog = green
```

```
scala> frog.philosophize
I consume memeory, therefor I am!
```

可以看到 Frog 添加了 Philosophical（哲学性）也具有了哲学家的特性，可以说出类似“我思故我在”的话语了。和 Interface 一样，Trait 也定义一个类型，比如：

```
scala> val phil:Philosophical = frog
phil: Philosophical = green
```

```
scala> phil.philosophize
I consume memeory, therefor I am!
```

变量 phil 的类型为 Philosophical。

如果你需要把某个 Trait 添加到一个有基类的子类中，使用 extends 继承基类，而可以通过 with  添加 Trait。比如：

```
class Animal
class Frog extends Animal with Philosophical{
  override def toString="green"
}
```

还是和 Interface 类似，可以为某个类添加多个 Trait 属性，此时使用多个 with 即可，比如：

```
class Animal
trait HasLegs 
class Frog extends Animal with Philosophical with HasLegs{
  override def toString="green"
}
```

目前为止你看到的例子中，类 Frog 都继承了 Philosophical 的 philosophize 实现。此外 Frog 也可以重载 philosophize 方法。语法与重载基类中定义的方法一样。

```
class Animal
trait HasLegs 
class Frog extends Animal with Philosophical with HasLegs{
  override def toString="green"
  def philosophize() {
    println("It ain't easy being " + toString + "!")
  }
}
```

因为 Frog 的这个新定义仍然混入了特质 Philosophize，你仍然可以把它当作这种类型的变量使用。但是由于 Frog 重载了 Philosophical 的 philosophize 实现，当你调用它的时候，你会得到新的回应：

```
scala> val phrog:Philosophical = new Frog
phrog: Philosophical = green
```

```
scala> phrog.philosophize
It ain't easy being green!
```

这时你或许推导出以下结论：Trait 就像是带有具体方法的 Java 接口，不过其实它能做的更多。Trait 可以，比方说，声明字段和维持状态值。实际上，你可以用 Trait 定义做任何用类定义做的事，并且语法也是一样的，除了两点。第一点，Trait 不能有任何“类”参数，也就是说，传递给类的主构造器的参数。换句话说，尽管你可以定义如下的类：

```
class Point(x: Int, y: Int)
```

但下面的 Trait 定义直接报错：

```
scala> trait NoPoint(x:Int,y:Int)
<console>:1: error: traits or objects may not have parameters
       trait NoPoint(x:Int,y:Int)
```

