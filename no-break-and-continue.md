# 没有“break”和“continue”的日子 #

你也许注意到到目前为止，我们介绍 Scala 的内置的控制结构时，没有提到使用 break，和 continue。Scala 特地没有在内置控制结构中包含 break 和 continue 是因为这两个控制结构和函数字面量有点格格不入，函数字面量我们将在后面介绍，函数字面量和其它类型字面量，比如数值字面量  4，5.6 相比，他们在 Scala 的地位相同。

我们很清楚 break 和 continue 在循环控制结构中的作用，Scala 内置控制结构特地去掉了 break 和 continue，是为了更好的适应函数化编程。不同你不用担心，Scala 提供了多种方法来替代 break 和 continue 的作用。

一个简单的方法是使用一个 if 语句来代替一个 continue，使用一个布尔控制量来去除一个 break。比如下面的 Java 代码使用 continue 和 break 在循环结构中：

```
int i=0;
boolean foundIt=false;
while(i <args.length) {
  if (args[i].startWith("-")) {
    i=i+1;
	continue;
  }
  if(args[i].endsWith(".scala")){
    foundIt=true;
	break;
  }
  i=i+1;
 }
 ```
 
这段 Java 代码实现的功能是从一组字符串中寻找以“ .scala ”结尾的字符串，但跳过以“-”开头的字符串。

下面我们使用 if 和 boolean 变量，逐句将这段实现使用 Scala 来实现（不使用 break 和continue)如下：

```
var i=0
var foundIt=false
while (i < args.length && !foundIt) {
  if (!args(i).startsWith("-")) {
    if(args(i).endsWith(".scala"))
      foundIt=true
  }
  i=i+1
}
```

可以看到，我们使用 if（于前面 continue 条件相反）去掉了 continue，而重用了 foundIt 布尔变量，去掉了 break。这段代码和前面 Java 实现非常类似，并且使用了两个 var 变量，使用纯函数化编程的一个方法是去掉 var 变量的使用，递归函数（回溯函数）的使用是通常使用的一个方法来去除循环结构中使用 var 变量。

使用递归函数重新实现上面代码实现的查询功能：

```
def searchFrom(i:Int) : Int =
  if( i >= args.length) -1
  else if (args(i).startsWith("-")) searchFrom (i+1)
  else if (args(i).endsWith(".scala")) i
  else searchFrom(i+1)
val i = searchFrom(0)
```

在函数化编程中使用递归函数来实现循环是非常常见的一种方法，我们应用熟悉使用递归函数的用法。

如果你实在还是希望使用 break，Scala 在 scala.util.control 包中定义了 break 控制结构，它的实现是通过抛出异常给上级调用函数，有希望的可以参考 Scala 源码，下面给出使用 break 的一个例子，不停的从屏幕读取一个非空行，如果用户输入一个空行，则退出循环。

```
import scala.util.control.Breaks._
import java.io._
val in = new BufferedReader(new InputStreamReader(System.in))
breakable {
  while(true) {
    println("? ")
    if(in.readLine()=="") break
  }
}
```
