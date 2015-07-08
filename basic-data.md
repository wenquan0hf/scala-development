# 基本数据类型 #

本篇介绍 Scala 支持的基本数据类型，如果你是个 Java 程序员，你会发现 Java 支持的基本数据类型，Scala 都有对应的支持，不过 Scala 的数据类型都是对象(比如整数），这些基本类型都可以通过隐式自动转换的形式支持比 Java 基本数据类型更多的方法。隐式自动转换的概念将在后面介绍，简单的说就是可以为基本类型提供扩展，比如如果调用 （-1）.abs() ，Scala 发现基本类型 Int 没有提供 abs（）方法，但可以发现系统提供了从 Int 类型转换为 RichInt 的隐式自动转换，而 RichInt 具有 abs 方法，那么 Scala 就自动将 1 转换为 RichInt 类型，然后调用 RichInt 的 abs 方法。

Scala 的基本数据类型有: Byte,Short，Int，Long 和 Char （这些成为整数类型）。整数类型加上 Float 和 Double 成为数值类型。此外还有 String 类型，除 String 类型在 java.lang 包中定义，其它的类型都定义在包 scala 中。比如 Int 的全名为 scala.Int。实际上 Scala 运行环境自动会载入包 scala 和 java.lang 中定义的数据类型，你可以使用直接使用 Int，Short，String 而无需再引入包或是使用全称。

下面的例子给出了这些基本数据类型的字面量用法，由于 Scala 支持数据类型推断，你在定义变量时多数可以不指明数据类型，而是由 Scala 运行环境自动给出变量的数据类型：

```
Welcome to Scala version 2.11.0-M5 (OpenJDK 64-Bit Server VM, Java 1.7.0_25).
Type in expressions to have them evaluated.
Type :help for more information.
```

```
scala> var hex=0x5
hex: Int = 5
scala> var hex2=0x00ff
hex2: Int = 255
scala> val prog=0xcafebabel
prog: Long = 3405691582
scala> val littler:Byte= 38
littler: Byte = 38
scala> val big=1.23232
big: Double = 1.23232
scala> val a='A'
a: Char = A
scala> val f ='\u0041'
f: Char = A
scala> val hello="hello"
hello: String = hello
scala> val longString=""" Welcome to Ultamix 3000. Type "Help" for help."""
longString: String = " Welcome to Ultamix 3000. Type "Help" for help."
scala> val bool=true
bool: Boolean = true  
scala>  
```

Scala的基本数据类型的字面量也支持方法（这点和 Java 不同，Scala 中所有的数值字面量也是对象），比如：

```
scala> (-2.7).abs
res3: Double = 2.7
scala> -2.7 abs
warning: there were 1 feature warning(s); re-run with -feature for details
res4: Double = 2.7
scala> 0 max 5
res5: Int = 5
scala> 4 to 6
res6: scala.collection.immutable.Range.Inclusive = Range(4, 5, 6)  
```

这些方法其实是对于数据类型的 Rich 类型的方法，Rich 类型将在后面再做详细介绍。
