# 进一步Scala #

本篇继续上一篇对 Scala 的整体介绍，本篇进一步解释 Scala 的一些高级特性，当你学完本篇后，就有足够的知识编写一些实用的 Scala 脚本应用了。
## 第七步：使用类型参数化数组 ##
在 Scala 中你可以使用 new 来实例化一个类。当你创建一个对象的实例时，你可以使用数值或类型参数。如果使用类型参数，它的作用类似 Java 或 .Net 的 Generic 类型。所不同的是 Scala 使用方括号来指明数据类型参数，而非尖括号。比如

```
val greetStrings =new Array[String](3)
greetStrings(0)="Hello"
greetStrings(1)=","
greetStrings(2)="world!\n"
for(i <- 0 to 2)
  print(greetStrings(i))
```

可以看到 Scala 使用[]来为数组指明类型化参数，本例使用 String 类型，数组使用()而非[]来指明数组的索引。其中的 for 表达式中使用到 0 to 2 ,这个表达式演示了 Scala 的一个基本规则，如果一个方法只有一个参数，你可以不用括号和. 来调用这个方法。因此这里的 0 to 2, 其实为（0）.to(2) 调用的为整数类型的 to 方法，to 方法使用一个参数。

Scala 中所有的基本数据类型也是对象（和 Java 不同），因此 0 可以有方法（实际上调用的是 RichInt 的方法），这种只有一个参数的方法可以使用操作符的写法（不用.和括号），实际上 Scala 中表达式 1+2,最终解释为 (1).+(2) + 也是 Int 的一个方法，和 Java 不同的是，Scala 对方法的名称没有太多的限制，你可以使用符合作为方法的名称。

这里也说明为什么 Scala 中使用（）来访问数组元素，在 Scala 中，数组和其它普遍的类定义一样，没有什么特别之处，当你在某个值后面使用（）时，Scala 将其翻译成对应对象的 apply 方法。因此本例中 greetStrings(1) 其实调用 greetString.apply(1) 方法。这种表达方法不仅仅只限于数组，对于任何对象，如果在其后面使用（）,都将调用该对象的 apply 方法。同样的如果对某个使用（）的对象赋值，比如：

```
greetStrings(0)="Hello"
```

Scala 将这种赋值转换为该对象的 update 方法， 也就是 greetStrings.update(0,”hello”)。因此上面的例子，使用传统的方法调用可以写成：

```
val greetStrings =new Array[String](3)
greetStrings.update(0,"Hello")
greetStrings.update(1,",")
greetStrings.update(2,"world!\n")
for(i <- 0 to 2)
  print(greetStrings.apply(i))
```

从这点来说，数组在 Scala 中并不某种特殊的数据类型，和普通的类没有什么不同。

不过 Scala 还是提供了初始化数组的简单的方法，比如什么的例子数组可以使用如下代码：

```
val greetStrings =Array("Hello",",","World\n")
```

这里使用（）其实还是调用 Array 类的关联对象 Array 的 apply 方法，也就是

```
val greetStrings =Array.apply("Hello",",","World\n")
```

## 第八步: 使用Lists ##

Scala 也是一个面向函数的编程语言，面向函数的编程语言的一个特点是，调用某个方法不应该有任何副作用，参数一定，调用该方法后，返回一定的结果，而不会去修改程序的其它状态（副作用）。这样做的一个好处是方法和方法之间关联性较小，从而方法变得更可靠和重用性高。使用这个原则也就意味着就变量的设成不可修改的,这也就避免了多线程访问的互锁问题。

前面介绍的数组，它的元素是可以被修改的。如果需要使用不可以修改的序列，Scala 中提供了 Lists 类。和 Java 的 List 不同，Scala 的 Lists 对象是不可修改的。它被设计用来满足函数编程风格的代码。它有点像 Java 的 String，String 也是不可以修改的，如果需要可以修改的 String 对像，可以使用 StringBuilder 类。

比如下面的代码：

```
val oneTwo = List(1,2)
val threeFour = List(3,4)
val oneTwoThreeFour=oneTwo ::: threeFour
println (oneTwo + " and " + threeFour + " were not mutated.")
println ("Thus, " + oneTwoThreeFour + " is a new list")
```

定义了两个 List 对象 oneTwo 和 threeFour,然后通过:::操作符（其实为:::方法）将两个列表链接起来。实际上由于 List 的不可以修改特性，Scala 创建了一个新的 List 对象 oneTwoThreeFour 来保存两个列表连接后的值。

List 也提供了一个::方法用来向 List 中添加一个元素，::方法（操作符）是右操作符，也就是使用::右边的对象来调用它的::方法，Scala 中规定所有以：开头的操作符都是右操作符，因此如果你自己定义以：开头的方法（操作符）也是右操作符。

如下面使用常量创建一个列表：

```
val oneTowThree = 1 :: 2 ::3 :: Nil
println(oneTowThree)
```

调用空列表对象 Nil 的 ::方法 也就是 

```
val oneTowThree =  Nil.::(3).::(2).::(1)
```

Scala 的 List 类还定义其它很多很有用的方法，比如 head, last,length, reverse,tail 等这里就不一一说明了，具体可以参考 List 的文档。

## 第九步：使用元组（ Tuples ) ##

Scala 中另外一个很有用的容器类为 Tuples，和 List 不同的 Tuples 可以包含不同类型的数据，而 List 只能包含同类型的数据。Tuples 在方法需要返回多个结果时非常有用。（ Tuple 对应到数学的矢量的概念）。

一旦定义了一个元组，可以使用 .\_和索引来访问员组的元素（矢量的分量，注意和数组不同的是，元组的索引从 1 开始）。

```
val pair=(99,"Luftballons")
println(pair._1)
println(pair._2)
```

元祖的实际类型取决于它的分量的类型，比如上面 pair 的类型实际为 Tuple2[Int,String]，而  (‘u’,’r’,”the”,1,4,”me”) 的类型为 Tuple6[Char,Char,String,Int,Int,String]。  

目前 Scala 支持的元祖的最大长度为 22。如果有需要，你可以自己扩展更长的元祖。

## 第十步： 使用 Sets 和 Maps ##
Scala 语言的一个设计目标是让程序员可以同时利用面向对象和面向函数的方法编写代码，因此它提供的集合类分成了可以修改的集合类和不可以修改的集合类两大类型。比如 Array 总是可以修改内容的，而 List 总是不可以修改内容的。类似的情况，Scala 也提供了两种 Sets 和 Map 集合类。

比如 Scala API 定义了 Set 的基 Trait 类型 Set（ Trait 的概念类似于 Java 中的 Interface，所不同的 Scala 中的 Trait 可以有方法的实现），分两个包定义 Mutable （可变）和 Immutable （不可变），使用同样名称的子 Trait。下图为 Trait 和类的基础关系：

![](images\9.png)

使用 Set 的基本方法如下：

```
var jetSet = Set ("Boeing","Airbus")
jetSet +="Lear"
println(jetSet.contains("Cessna"))
```

缺省情况 Set 为 Immutable Set，如果你需要使用可修改的集合类（ Set 类型），你可以使用全路径来指明 Set，比如 scala.collection.mutalbe.Set 。

Scala 提供的另外一个类型为 Map 类型，Scala 也提供了 Mutable 和 Immutable 两种 Map 类型。

![](images\10.png)

Map 的基本用法如下（ Map 类似于其它语言中的关联数组如 PHP ）

```
val romanNumeral = Map ( 1 -> "I" , 2 -> "II",
  3 -> "III", 4 -> "IV", 5 -> "V")
println (romanNumeral(4))
```

## 第十一步： 学习识别函数编程风格 ##

Scala 语言的一个特点是支持面向函数编程，因此学习 Scala 的一个重要方面是改变之前的指令式编程思想（尤其是来自 Java 或 C# 背景的程序员），观念要想函数式编程转变。首先在看代码上要认识哪种是指令编程，哪种是函数式编程。实现这种思想上的转变，不仅仅会使你成为一个更好的 Scala 程序员，同时也会扩展你的视野，使你成为一个更好的程序员。

一个简单的原则，如果代码中含有 var 类型的变量，这段代码就是传统的指令式编程，如果代码只有 val 变量，这段代码就很有可能是函数式代码，因此学会函数式编程关键是不使用 vars 来编写代码。

来看一个简单的例子：

```
def printArgs ( args: Array[String]) : Unit ={
	var i=0
	while (i < args.length) {
	  println (args(i))
	  i+=1
	}
}
```

来自 Java 背景的程序员开始写 Scala 代码很有可能写成上面的实现。我们试着去除 vars 变量，可以写成跟符合函数式编程的代码：

```
def printArgs ( args: Array[String]) : Unit ={
	for( arg <- args)
	  println(arg)
}
```

或者更简化为：

```
def printArgs ( args: Array[String]) : Unit ={
	args.foreach(println)
}
```

这个例子也说明了尽量少用 vars 的好处，代码更简洁和明了，从而也可以减少错误的发生。因此 Scala 编程的一个基本原则上，能不用 Vars，尽量不用 vars,能不用 mutable变量，尽量不用 mutable变量，能避免函数的副作用，尽量不产生副作用。

## 第十二步： 读取文件 ##

使用脚本实现某个任务，通常需要读取文件，本节介绍 Scala 读写文件的基本方法。比如下面的例子读取文件的每行，把该行字符长度添加到行首：

```
import scala.io.Source
if (args.length >0 ){
  for( line <- Source.fromFile(args(0)).getLines())
    println(line.length + " " + line)
}
   else
      Console.err.println("Please enter filename")
```


可以看到 Scala 引入包的方式和 Java 类似，也是通过 import 语句。文件相关的类定义在 scala.io 包中。 如果需要引入多个类，Scala 使用 “_” 而非 “*”。

通过前面两篇文章的介绍，你应该对 Scala 编程有了一个大概的了解，可以编写一些简单的 Scala 脚本语言。Scala 的功能远远不止这些，在后面的文章我们在一一详细介绍。
