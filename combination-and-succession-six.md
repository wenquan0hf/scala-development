# 组合和继承–定义参数化成员变量 #

我们回到前面定义的类 ArrayElement，它有一个参数 conts，其唯一的目的是用来复制到 contents 成员变量。而参数名称 conts 是为了让它看起来和成员变量 contents 类似，而有不至于和成员变量名冲突。

Scala 支持使用参数化成员变量，也就是把参数和成员变量定义合并到一起来避免上述代码：

```
class ArrayElement(val contents: Array[String]) 
  extends Element {
}
```

要注意的是，现在参数 contents 前面加上了 val 关键字，这是前面使用同名参数和同名成员变量的一个缩写形式。使用 val 定义了一个无法重新赋值的成员变量。这个成员变量初始值为参数的值，可以在类的外面访问这个成员变量。它的一个等效的实现如下：

```
class ArrayElement(val x123: Array[String]) 
  extends Element {
   val contents: Array[String] = x123
}
```

Scala 也允许你使用 var 关键字来定义参数化成员变量，使用 var 定义的成员变量，可以重新赋值。
此外 Scala 也允许你使用 private，protected，override 来修饰参数化成员变量，和你定义普通的成员变量的用法一样。 比如：

```
class Cat {
  val dangerous =false
}
```

```
class Tiger (
  override val dangerous: Boolean,
  private var age: Int
) extends Cat
```           

这段代码中 Tiger 的定义其实为下面类定义的一个缩写：

```
class Tiger(param1: Boolean, param2: Int) extends Cat { 
	override val dangerous = param1 
	private var age = param2 
}            
```

两个成员都初始化自相应的参数。我们任意选择了这些参数名，param1 和 param2。重要的是它们不会与范围内的任何其它名称冲突。
