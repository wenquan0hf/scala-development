# 函数–函数–可变参数，命名参数，缺省参数 #
前面我们介绍的函数的参数是固定的，本篇介绍 Scala 函数支持的可变参数列表，命名参数和参数缺省值定义。

## 重复参数 ##
Scala 在定义函数时允许指定最后一个参数可以重复（变长参数），从而允许函数调用者使用变长参数列表来调用该函数，Scala 中使用“*”来指明该参数为重复参数。例如：

```
scala> def echo (args: String *) =
     |   for (arg <- args) println(arg)
echo: (args: String*)Unit
```

```
scala> echo()
scala> echo ("One")
One
```

```
scala> echo ("Hello","World")
Hello
World
```


在函数内部，变长参数的类型，实际为一数组，比如上例的 String * 类型实际为 Array[String]。 然而，如今你试图直接传入一个数组类型的参数给这个参数，编译器会报错：

```
scala> val arr= Array("What's","up","doc?")
arr: Array[String] = Array(What's, up, doc?)
```

```
scala> echo (arr)
<console>:10: error: type mismatch;
 found   : Array[String]
 required: String
              echo (arr)
                    ^
```

为了避免这种情况，你可以通过在变量后面添加 _*来解决，这个符号告诉 Scala 编译器在传递参数时逐个传入数组的每个元素，而不是数组整体。

```
scala> echo (arr: _*)
What's
up
doc?
```

## 命名参数 ##
通常情况下，调用函数时，参数传入和函数定义时参数列表一一对应。

```
scala> def  speed(distance: Float, time:Float) :Float = distance/time
speed: (distance: Float, time: Float)Float
```

```
scala> speed(100,10)
res0: Float = 10.0
```

使用命名参数允许你使用任意顺序传入参数，比如下面的调用：

```
scala> speed( time=10,distance=100)
res1: Float = 10.0
```

```
scala> speed(distance=100,time=10)
res2: Float = 10.0
```

## 缺省参数值 ##
Scala 在定义函数时，允许指定参数的缺省值，从而允许在调用函数时不指明该参数，此时该参数使用缺省值。缺省参数通常配合命名参数使用，例如：

```
scala> def printTime(out:java.io.PrintStream = Console.out, divisor:Int =1 ) =
     | out.println("time = " + System.currentTimeMillis()/divisor)
printTime: (out: java.io.PrintStream, divisor: Int)Unit
```

```
scala> printTime()
time = 1383220409463
```

```
scala> printTime(divisor=1000)
time = 1383220422
```
