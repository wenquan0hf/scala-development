# 函数–闭包 #

到目前为止我们介绍的函数都只引用到传入的参数，假如我们定义如下的函数：

```
(x:Int) => x + more
```

这里我们引入一个自由变量 more。它不是所定义函数的参数，而这个变量定义在函数外面，比如：

```
var more =1
```

那么我们有如下的结果：

```
scala> var more =1
more: Int = 1
scala> val addMore = (x:Int) => x + more
addMore: Int => Int = <function1>
scala> addMore (100)
res1: Int = 101
```

这样定义的函数变量 addMore 成为一个“闭包”，因为它引用到函数外面定义的变量，定义这个函数的过程是将这个自由变量捕获而构成一个封闭的函数。有意思的是，当这个自由变量发生变化时，Scala 的闭包能够捕获到这个变化，因此 Scala 的闭包捕获的是变量本身而不是当时变量的值。

比如：

```
scala> more =  9999
more: Int = 9999
```

```
scala> addMore ( 10)
res2: Int = 10009
```

同样的，如果变量在闭包在发生变化，也会反映到函数外面定义的闭包的值。比如：

```
scala> val someNumbers = List ( -11, -10, -5, 0, 5, 10)
someNumbers: List[Int] = List(-11, -10, -5, 0, 5, 10)
```

```
scala> var sum =0
sum: Int = 0
```


```
scala> someNumbers.foreach ( sum += _)
```

```
scala> sum
res4: Int = -11
```

可以看到在闭包中修改 sum 的值，其结果还是传递到闭包的外面。

如果一个闭包所访问的变量有几个不同的版本，比如一个闭包使用了一个函数的局部变量（参数），然后这个函数调用很多次，那么所定义的闭包应该使用所引用的局部变量的哪个版本呢？ 简单的说，该闭包定义所引用的变量为定义该闭包时变量的值，也就是定义闭包时相当于保存了当时程序状态的一个快照。比如我们定义下面一个函数闭包：

```
scala> def makeIncreaser(more:Int) = (x:Int) => x + more
makeIncreaser: (more: Int)Int => Int
```

```
scala> val inc1=makeIncreaser(1)
inc1: Int => Int = <function1>
```

```
scala> val inc9999=makeIncreaser(9999)
inc9999: Int => Int = <function1>
```

```
scala> inc1(10)
res5: Int = 11
```

```
scala> inc9999(10)
res6: Int = 10009
```

当你调用 makeIncreaser(1)时，你创建了一个闭包，该闭包定义时 more的值为 1，而调用 makeIncreaser(9999)所创建的闭包的 more 的值为 9999。此后你也无法修改已经返回的闭包的 more 的值。因此 inc1 始终为加一，而 inc9999 始终为加 9999。
