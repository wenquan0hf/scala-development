# 函数–头等公民 #

Scala 中函数为头等公民，你不仅可以定义一个函数然后调用它，而且你可以写一个未命名的函数字面量，然后可以把它当成一个值传递到其它函数或是赋值给其它变量。下面的例子为一个简单的函数字面量（参考整数字面量，3 为一整数字面量）。

```
(x :Int ) => x +1
```

这是个函数字面量，它的功能为+1. 符好 => 表示这个函数将符号左边的东西（本例为一个整数），转换成符号右边的东西（加 1）。

函数字面量为一个对象（就像 3 是个对象），因此如果你愿意的话，可以把这个函数字面量保持在一个变量中。这个变量也是一函数，因此你可以使用函数风格来调用它，比如：

```
scala> var increase = (x :Int ) => x +1
increase: Int => Int = <function1>
scala> increase(10)
res0: Int = 11
```

注意函数字面量 (x:Int) => x + 1 的类型为，它在 Scala 内部表示为带有一个参数的类 Function1 的一个对象，其它如 FunctionN 代表带有N个参数的函数，Function0 代表不含参数的函数类型。

如果函数定义需要多条语句，可以使用{},比如：

```
scala> var increase = (x :Int ) => {
     |    println("We")
     |    println("are")
     |    println("here")
     |    x + 1
     |    }
increase: Int => Int = <function1>
scala> increase (10)
We
are
here
res0: Int = 11
```

什么我们了解到了函数字面量的基本概念，它可以作为参数传递个其它函数，比如很多 Scala 的库允许你使用函数作为参数，比如 foreach 方法，它使用一个函数参数，为集合中每个运算调用传入的函数。例如：


```
scala> val someNumbers = List ( -11, -10, - 5, 0, 5, 10)
someNumbers: List[Int] = List(-11, -10, -5, 0, 5, 10)
scala> someNumbers.foreach((x:Int) => println(x))
-11
-10
-5
0
5
10
```

再比如，Scala 的集合也支持一个 filter 方法用来过滤集合中的元素，filter 的参数也是一个函数，比如：

```
scala> someNumbers.filter( x => x >0)
res1: List[Int] = List(5, 10)
```

使用 x => x >0，过滤掉小于 0 的元素。 如果你熟悉 lambda 表达式，x => x >0 为函数的 lambda 表达式。
