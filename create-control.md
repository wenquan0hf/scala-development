# 创建新的控制结构 #
对于支持函数作为“头等公民”的语言，你可以有效的创建新的控制结构即使该语言语法上固定的。你所要做的事创建一个方法，该方法使用函数类型作为参数。

比如： 下面为一个“双倍”的控制结构，这个“双倍”控制结构可以重复一个操作，然后返回结果。

```
scala> def twice (op:Double => Double, x:Double) =op(op(x))
twice: (op: Double => Double, x: Double)Double
```

```
scala> twice(_ + 1, 5)
res0: Double = 7.0
```

上面调用 twice ，其中 _+1 调用两次，也就是 5 调用两次 +1，结果为 7。

你在写代码时，如果发现某些操作需要重复多次，你就可以试着将这个重复操作写成新的控制结构，在前面我们定义过一个 filesMatching 函数

```
def filesMatching(
    matcher: (String) => Boolean) = {
    for(file <- filesHere; if matcher(file.getName))
      yield file
   }
```

如果我们把这个函数进一步通用化，可以定义一个通用操作如下：

打开一个资源，然后对资源进行处理，最后释放资源，你可以为这个“模式”定义一个通用的控制结构如下：

```
def withPrintWriter (file: File, op: PrintWriter => Unit) {
  val writer=new PrintWriter(file)
  try{
    op(writer)
  }finally{
    writer.close()
  }
}
```

使用上面定义，我们使用如下调用：

```
withPrintWriter(
   new File("date.txt"),
   writer => writer.println(new java.util.Date)
)
```

使用这个方法的优点在于 withPrintWriter，而不是用户定义的代码，withPrintWriter 可以保证文件在使用完成后被关闭，也就是不可能发生忘记关闭文件的事件。这种技术成为“租赁模式”，这是因为这种类型的控制结构，比如 withPrintWriter 将一个 PrintWriter 对象“租”给 op 操作，当这个 op 操作完成后，它通知不再需要租用的资源，在 finally 中可以保证资源被释放，而无论 op 是否出现异常。

这里调用语法还是使用函数通常的调用方法，使用（）来列出参数，在 Scala 中如果你调用函数只有一个参数，你可以使用{}来替代().比如下面两种语法是等价的：

```
scala> println ("Hello,World")
Hello,World
```

```
scala> println { "Hello,world" }
Hello,world
```

上面第二种用法，使用{}替代了()，但这只适用在使用一个参数的调用情况。 前面定义 withPrintWriter 函数使用了两个参数，因此不能使用{}来替代()，但如果我们使用柯里化重新定义下这个函数如下：

```
import scala.io._
import java.io._
def withPrintWriter (file: File)( op: PrintWriter => Unit) {
  val writer=new PrintWriter(file)
  try{
    op(writer)
  }finally{
    writer.close()
  }
}
```

将一个参数列表，变成两个参数列表，每个列表含一个参数，这样我们就可以使用如下语法来调用

```
withPrintWriter 
val file = new File("date.txt")
withPrintWriter(file){
  writer => writer.println(new java.util.Date)
}
```

第一个参数我们还是使用（）（我们也可以使用{})，第二个参数我们使用{}来替代()，这样修改过的代码使得 withPrintWriter 看起来和 Scala 内置的控制结构语法一样。
