# 传名参数 #

上篇我们使用柯里化函数定义一个控制机构 withPrintWriter，它使用时语法调用有如 Scala 内置的控制结构：

```
val file = new File("date.txt")
withPrintWriter(file){
  writer => writer.println(new java.util.Date)
}
```

不过仔细看一看这段代码，它和 scala 内置的 if 或 while 表达式还是有些区别的，withPrintWrite r的{}中的函数是带参数的含有“writer=>”。 如果你想让它完全和 if 和 while 的语法一致，在 Scala 中可以使用传民参数来解决这个问题。

`注`：我们知道通常函数参数传递的两种模式，一是传值，一是引用。而这里是第三种按名称传递。

下面我们以一个具体的例子来说明传名参数的用法：

```
var assertionsEnabled=true
def myAssert(predicate: () => Boolean ) =
  if(assertionsEnabled && !predicate())
    throw new AssertionError
```

这个 myAssert 函数的参数为一个函数类型，如果标志 assertionsEnabled 为 True 时，mymyAssert 根据 predicate 的真假决定是否抛出异常，如果 assertionsEnabled 为 false，则这个函数什么也不做。

这个定义没什么问题，但调用起来看起来却有些别扭，比如：

```
myAssert(() => 5 >3 )
```

还需要 ()=>，你可以希望直接使用 5>3，但此时会报错：

```
scala> myAssert(5 >3 )
<console>:10: error: type mismatch;
 found   : Boolean(true)
 required: () => Boolean
              myAssert(5 >3 )

```

此时，我们可以把按值传递（上面使用的是按值传递，传递的是函数类型的值）参数修改为按名称传递的参数，修改方法，是使用=>开始而不是 ()=>来定义函数类型，如下：

```
def myNameAssert(predicate:  => Boolean ) =
  if(assertionsEnabled && !predicate)
    throw new AssertionError
```

此时你就可以直接使用下面的语法来调用 myNameAssert：

```
myNameAssert(5>3)
```

此时就和 Scala 内置控制结构一样了，看到这里，你可能会想我为什么不直接把参数类型定义为 Boolean，比如：

```
def boolAssert(predicate: Boolean ) =
  if(assertionsEnabled && !predicate)
    throw new AssertionError
```

调用也可以使用

```
boolAssert(5>3)
```

和 myNameAssert 调用看起来也没什么区别，其实两者有着本质的区别，一个是传值参数，一个是传名参数，在调用 boolAssert(5>3)时，5>3 是已经计算出为 true，然后传递给 boolAssert 方法，而 myNameAssert(5>3)，表达式 5>3 没有事先计算好传递给 myNameAssert，而是先创建一个函数类型的参数值，这个函数的 apply 方法将计算5>3，然后这个函数类型的值作为参数传给 myNameAssert。

因此这两个函数一个明显的区别是，如果设置 assertionsEnabled 为 false，然后试图计算 x/0 ==0，

```
scala> assertionsEnabled=false
assertionsEnabled: Boolean = false
```

```
scala> val x = 5
x: Int = 5
```

```
scala> boolAssert ( x /0 ==0)
java.lang.ArithmeticException: / by zero
  ... 32 elided
```

```
scala> myNameAssert ( x / 0 ==0)
```

可以看到 boolAssert 抛出 java.lang.ArithmeticException: / by zero 异常，这是因为这是个传值参数，首先计算 x /0 ，而抛出异常，而 myNameAssert 没有任何显示，这是因为这是个传名参数，传入的是一个函数类型的值，不会先计算 x /0 ==0，而在 myNameAssert 函数体内，由于 assertionsEnabled 为 false，传入的 predicate 没有必要计算(短路计算），因此什么也不会打印。如果我们把 myNameAssert 修改下，把 predicate 放在前面:

```
scala> def myNameAssert1(predicate:  => Boolean ) =
     |   if( !predicate && assertionsEnabled )
     |     throw new AssertionError
myNameAssert1: (predicate: => Boolean)Unit
```

```
scala> myNameAssert1 ( x/0 ==0)
java.lang.ArithmeticException: / by zero
  at $anonfun$1.apply$mcZ$sp(<console>:11)
  at .myNameAssert1(<console>:9)
  ... 32 elided
```

这个传名参数函数也抛出异常（你可以想想是为什么？）

前面的 withPrintWriter 我们暂时没法使用传名参数，去掉 writer=>，否则就难以实现“租赁模式”，不过我们可以看看下面的例子，设计一个 withHelloWorld 控制结构，这个 withHelloWorld 总打印一个“hello,world”

```
import scala.io._
import java.io._
def withHelloWorld ( op: => Unit) {
  op   
  println("Hello,world")
}
```

```
val file = new File("date.txt")
withHelloWorld{
  val writer=new PrintWriter(file)
  try{
   writer.println(new java.util.Date)
  }finally{
    writer.close()
  }
}
```

```
withHelloWorld {
  println ("Hello,Guidebee")
} 
```

-----
```
Hello,world 
Hello,Guidebee
Hello,world
```

可以看到 withHelloWorld 的调用语法和 Scala 内置控制结构非常象了。
