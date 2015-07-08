#函数–部分应用的函数#
前面例子中我们使用“\_” 来代替单个的参数，实际上你也可以使用“\_”来代替整个参数列表，比如说，你可以使用 print _ 来代替 println (_).someNumbers.foreach(println _)。

Scala 编译器自动将上面代码解释成：

```
someNumbers.foreach( x => println (x))
```

因此这里的“\_” 代表了 Println 的整个参数列表，而不仅仅替代单个参数。当你采用这种方法使用“\_”，你就创建了一个部分应用的函数(partially applied function)。 在 Scala 中，当你调用函数，传入所需参数，你就把函数“应用”到参数。 比如：一个加法函数。

```
scala> def sum = (_:Int) + (_ :Int) + (_ :Int)
sum: (Int, Int, Int) => Int
scala> sum (1,2,3)
res0: Int = 6
```

一个部分应用的函数指的是你在调用函数时，不指定函数所需的所有参数，这样你就创建了一个新的函数，这个新的函数就称为原始函数的部分应用函数，比如说我们固定 sum 的第一和第三个参数，定义如下的部分应用函数：

```
scala> val b = sum ( 1 , _ :Int, 3)
b: Int => Int = <function1>
scala> b(2)
res1: Int = 6
```

变量 b 的类型为一函数，具体类型为 Function1（带一个参数的函数），它是由 sum 应用了第一个和第三个参数，构成的。调用b(2），实际上调用 sum (1,2,3)。

再比如我们定义一个新的部分应用函数，只固定中间参数：

```
scala> val c = sum (_:Int, 2, _:Int)
c: (Int, Int) => Int = <function2>
scala> c(1,3)
res2: Int = 6
```

变量 c 的类型为 Function2,调用 c(1,3) 实际上也是调用 sum (1,2,3)。

在 Scala 中，如果你定义一个部分应用函数并且能省去所有参数，比如 println _ ，你也可以省掉“_”本身，比如：

```
someNumbers.foreach(println _)
```

可以写成：

```
someNumbers.foreach(println)
```

