# 组合和继承–定义无参数方法 #

作为下一步，我们将向 Element 添加显示宽度和高度的方法，height 方法返回 contents 里的行数。width 方法返回第一行的长度，或如果元素没有行记录，返回零。

```
abstract class Element { 
  def contents: Array[String] 
  def height: Int = contents.length 
  def width: Int = if (height == 0) 0 else contents(0).length 
}
```

请注意 Element 的三个方法没一个有参数列表，甚至连个空列表都没有,这种无参数方法在 Scala 里是非常普通的。相对的，带有空括号的方法定义，如 def height(): Int，被称为空括号方法：(empty-paren method)。

Scala的惯例是在方法不需要参数并且只是读取对象状态时使用无参数方法。

此外，我们也可以使用成员变量来定义 width 和 height，例如：

```
abstract class Element { 
  def contents: Array[String] 
  val height = contents.length 
  val width = if (height == 0) 0 else contents(0).length 
}
```

从使用这个类的客户代码来说，这两个实现是等价的，唯一的差别是使用成员变量的方法调用速度要快些，因为字段值在类被初始化的时候被预计算，而方法调用在每次调用的时候都要计算。换句话说，字段在每个 Element 对象上需要更多的内存空间。

特别是如果类的字段变成了访问函数，且访问函数是纯函数的，就是说它没有副作用并且不依赖于可变状态，那么类 Element 的客户不需要被重写。这称为统一访问原则： uniform access principle， 就是说客户代码不应受通过字段还是方法实现属性的决定的影响。

Scala 代码可以调用 Java 函数和类，而 Java 没有使用“统一访问原则”，因此 Java 里是 string.length()，不是 string.length。为了解决这个问题，Scala 对于无参数函数和空括号函数的使用上并不是区分得很严格。也就是，你可以用空括号方法重载无参数方法，并且反之亦可。你还可以在调用任何不带参数的方法时省略空的括号。例如，下面两行在 Scala里都是合法的：

```
Array(1, 2, 3).toString 
"abc".length
```

原则上 Scala 的函数调用中可以省略所有的空括号。但如果使用的函数不是纯函数，也就是说这个不带参数的函数可能修改对象的状态或是我们需要利用它的一些副作用（比如打印到屏幕，读写 I/o)，一般的建议还是使用空括号，比如：

```
"hello".length // 没有副作用，所以无须() 
println() // 最好别省略()
```

总结起来，Scala 里定义不带参数也没有副作用的方法为无参数方法，也就是说，省略空的括号，是鼓励的风格。另一方面，永远不要定义没有括号的带副作用的方法，因为那样的话方法调用看上去会像选择一个字段。
