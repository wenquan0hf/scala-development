# 函数–函数字面量的一些简化写法 #
Scala 提供了多种方法来简化函数字面量中多余的部分，比如前面例子中 filter 方法中使用的函数字面量,完整的写法如下：

```
 (x :Int ) => x +1
```

首先可以省略到参数的类型，Scala 可以根据上下文推算出参数的类型，函数定义可以简化为：

```
 (x) => x +1
```

这个函数可以进一步去掉参数的括号，这里的括号不起什么作用：

```
 x => x +1
```

Scala 还可以进一步简化，Scala 允许使用“占位符”下划线”_”来替代一个或多个参数，只要这个参数值函数定义中只出现一次，Scala编译器可以推断出参数。比如：

```
scala> val someNumbers = List ( -11, -10, - 5, 0, 5, 10)
someNumbers: List[Int] = List(-11, -10, -5, 0, 5, 10)
scala> someNumbers.filter(_ >0)
res0: List[Int] = List(5, 10)
```

可以看到简化后的函数定义为 _ > 0，你可以这样来理解，就像我们以前做过的填空题，“_”为要填的空，Scala 来完成这个填空题，你来定义填空题。

有时，如果你使用_来定义函数，可能没有提供足够的信息给 Scala 编译器，此时 Scala 编译器将会报错，比如，定义一个加法函数如下：

```
scala> val f = _ + _
<console>:7: error: missing parameter type for expanded function ((x$1, x$2) => x$1.$plus(x$2))
       val f = _ + _
               ^
<console>:7: error: missing parameter type for expanded function ((x$1: <error>, x$2) => x$1.$plus(x$2))
       val f = _ + _
```

Scala 编译器无法推断出\_的参数类型，就报错了，但如果你给出参数的类型，依然可以使用_来定义函数，比如：

```
scala> val f = (_ :Int ) + ( _ :Int)
f: (Int, Int) => Int = <function2>
scala> f (5,10)
res1: Int = 15
```

因为_替代的参数在函数体中只能出现一次，因此多个“\_”代表多个参数。第一个“\_”代表第一个参数，第二个“\_”代表第二个参数，以此类推。
