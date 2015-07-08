# try表达式处理异常 #
Scala 的异常处理和其它语言比如 Java 类似，一个方法可以通过抛出异常的方法而不返回值的方式终止相关代码的运行。调用函数可以捕获这个异常作出相应的处理或者直接退出，在这种情况下，异常会传递给调用函数的调用者，依次向上传递，直到有方法处理这个异常。
## 抛出异常 ##
Scala 抛出异常的方法和 Java一样，使用 throw 方法，例如，抛出一个新的参数异常：

```
throw new IllegalArgumentException
```

尽管看起来似乎有些自相矛盾，Scala 中，throw 也是一个表达式，也是有返回值的，比如下面的例子：

```
val half =
  if (n % 2 == 0)
    n/2
  else
    throw new RuntimeException("n must be even")
```


当 n 为偶数时，n 初始化为 n 的一半，而如果 n 为奇数，将在初始化 half 之前就抛出异常，正因为如此，可以把 throw 的返回值的类型为任意类型。技术上来说，抛出异常的类型为 Nothing。对于说明的例子来说整个 if 表达式的类型为可以计算出值的那个分支的类型，如果 n 为 Int，那么 if 表示式的类型也是 Int 类型，而不需要考虑 throw 表达式的类型。

## 捕获异常 ##

Scala 捕获异常的方法和后面介绍的“模式匹配”的使用方法是一致的。比如：

```
import java.io.FileReader
import java.io.FileNotFoundException
import java.io.IOException
try {
  val f = new FileReader("input.txt")
} catch {
  case ex: FileNotFoundException => //handle missing file
  case ex: IOException => //handle other I/O error
}
```


模式匹配将在后面介绍，try-catch 表达式的基本用法和 Java 一样，如果 try 块中代码在执行过程中出现异常，将逐个检测每个 catch 块，在上面的例子，如果打开文件出现异常，将先检查是否是 FileNotFoundException 异常，如果不是，再检查是否是 IOException，如果还不是，在终止 try-catch 块的运行，而向上传递这个异常。

`注意`：和 Java 异常处理不同的一点是，Scala 不需要你捕获 checked 的异常，这点和 C# 一样，也不需要使用 throw 来声明某个异常，当然如果有需要还是可以通过 @throw 来声明一个异常，但这不是必须的。

## finally语句 ##
Scala 也支持 finally 语句，你可以在 finally 块中添加一些代码，这些代码不管 try 块是否抛出异常，都会执行。比如，你可以在 finally 块中添加代码保证关闭已经打开的文件，而不管前面代码中是否出现异常。

```
import java.io.FileReader
val file = new FileReader("input.txt")
try {
  //use the file
} finally {
  file.close()
}
```


## 生成返回值 ##
和大部分Scala 控制结构一样，Scala 的 try-catch-finally 也生成某个值，比如下面的例子尝试分析一个 URL，如果输入的 URL 无效，则使用缺省的 URL 链接地址：

```
import java.net.URL
import java.net.MalformedURLException
def urlFor(path:String) =
  try {
    new URL(path)
  } catch {
    case e: MalformedURLException =>
      new URL("http://www.scala-lang.org")
  }
```

通常情况下，finally 块用来做些清理工作，而不应该产生结果，但如果在 finally 块中使用 return 来返回某个值，这个值将覆盖 try-catch 产生的结果，比如：

```
scala> def f(): Int = try { return 1 } finally { return 2}
f: ()Int
scala> f
res0: Int = 2
```

而下面的代码：


```
scala> def g() :Int = try 1 finally 2
g: ()Int
scala> g
res0: Int = 1
```


结果却是 1，上面两种情况常常使得程序员产生困惑，因此关键的一点是避免在 finally 生成返回值，而只用来做些清理工作，比如关闭文件。
