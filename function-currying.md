# 柯里化函数 #

前面我们说过，Scala 允许程序员自己新创建一些控制结构，并且可以使得这些控制结构在语法看起来和 Scala 内置的控制结构一样，在 Scala 中需要借助于[柯里化(Currying)](https://www.wikipedia.org/search-redirect.php?family=wikipedia&search=%E6%9F%AF%E9%87%8C%E5%8C%96%28Currying%29&language=zh&go=++%E2%86%92++&go=Go)，柯里化是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。

下面先给出一个普通的非柯里化的函数定义，实现一个加法函数：

```
scala> def plainOldSum(x:Int,y:Int) = x + y
plainOldSum: (x: Int, y: Int)Int
```

```
scala> plainOldSum(1,2)
res0: Int = 3
```

下面在使用“柯里化”技术来定义这个加法函数，原来函数使用一个参数列表，“柯里化”把函数定义为多个参数列表：

```
scala> def curriedSum(x:Int)(y:Int) = x + y
curriedSum: (x: Int)(y: Int)Int
```

```
scala> curriedSum (1)(2)
res0: Int = 3
```

当你调用 curriedSum (1)(2)时，实际上是依次调用两个普通函数（非柯里化函数），第一次调用使用一个参数 x，返回一个函数类型的值，第二次使用参数y调用这个函数类型的值，我们使用下面两个分开的定义在模拟 curriedSum 柯里化函数：

首先定义第一个函数：

```
scala> def first(x:Int) = (y:Int) => x + y
first: (x: Int)Int => Int
```

然后我们使用参数1调用这个函数来生成第二个函数（回忆前面定义的闭包）。

``` 
scala> val second=first(1)
second: Int => Int = <function1>
```

```
scala> second(2)
res1: Int = 3
```

first，second的定义演示了柯里化函数的调用过程，它们本身和 curriedSum 没有任何关系，但是我们可以使用 curriedSum 来定义 second，如下：

```
scala> val onePlus = curriedSum(1)_
onePlus: Int => Int = <function1>
```


下划线“_” 作为第二参数列表的占位符， 这个定义的返回值为一个函数，当调用时会给调用的参数加一。

```
scala> onePlus(2)
res2: Int = 3
```

通过柯里化，你还可以定义多个类似 onePlus 的函数，比如 twoPlus

```
scala> val twoPlus = curriedSum(2) _
twoPlus: Int => Int = <function1>
```

```
scala> twoPlus(2)
res3: Int = 4
```

