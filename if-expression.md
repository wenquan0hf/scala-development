# if 表达式 #
和其它语言（比如 Java，C#)相比，Scala 只内置了为数不多的几种程序控制语句： if，while，for ，try match 以及函数调用，这是因为从Scala 诞生开始就包含了函数字面量，Scala 内核没有定义过多的控制结构，而是可以通过额外的库来扩展程序的控制结构。

Scala的所有控制结构都有返回结果，如果你使用过 Java或C#，就可能了解 Java 提供的三元运算符 ?: ，它的基本功能和 if 一样，当可以返回结果。Scala 在此基础上所有控制结构（while，try，if，等）都可以返回结果。这样做的一个好处是，可以简化代码，如果没有这种特点，程序员常常需要创建一个临时变量用来保存结果。

总的来说，Scala 提供的基本程序控制结构，“麻雀虽小，五脏俱全”，虽然少，但足够满足其他指令式语言（如 Java,C++）所支持的程序控制功能，而且由于这些指令都有返回结果，可以使得代码更为精简。

## if 表达式 ##
Scala 语言的 if 的基本功能和其它语言没有什么不同，它根据条件执行两个不同的分支，比如，使用                     Java 风格编写下面 Scala 的 if 语句的一个例子：

```
var filename="default.txt"
if(!args.isEmpty)
  filename =args(0)
```

上面代码和使用 Java 实现没有太多区别，看起来不怎么像 Scala 风格，我们重新改写一下，利用 if 可以返回结果这个特点。

```
val filename=
   if(!args.isEmpty)   args(0)
   else "default.txt"
```

首先这种代码比前段代码短，更重要的是这段代码使用 val 而无需使用 var 类型的变量。使用 val 为函数式编程风格。