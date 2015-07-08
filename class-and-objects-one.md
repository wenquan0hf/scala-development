# 类和对象 (一) #

有了前面的 Scala 基础，从本篇开始由浅到易逐步介绍 Scala 编程的各个方面，博客不可能做到面面俱到，还是希望你有些编程基础，尤其是有些面向对象的编程基础，如 Java，C++，C# 等更好。除支持函数化编程外，Scala 也是一个纯面向对象的编程语言。本篇和下篇介绍 Scala 的类和对象。

首先介绍 Scala 的类定义，我们以一个简单的例子开始，创建一个计算整数累计校验和的类

```
ChecksumAccumulator
class ChecksumAccumulator{
   private var sum=0
   def add(b:Byte) :Unit = sum +=b
   def checksum() : Int = ~ (sum & 0xFF) +1
}
```

可以看到 Scala 类定义和 Java 非常类似，也是以 class 开始，和 Java 不同的，Scala 的缺省修饰符为 public ，也就是如果不带有访问范围的修饰符 public,protected,private，Scala 缺省定义为 public。类的方法以 def 定义开始，要注意的 Scala 的方法的参数都是 val 类型，而不是 var 类型，因此在函数体内不可以修改参数的值，比如如果你修改 add 方法如下：

```
 def add(b:Byte) :Unit ={
       b=1
       sum+=b
   }
```

此时编译器会报错：

```
/root/scala/demo.scala:5: error: reassignment to val
 b=1
 ^
 one error found
```

类的方法分两种，一种是有返回值的，一种是不含返回值的，没有返回值的主要是利用代码的“副作用”，比如修改类的成员变量的值或者读写文件等。Scala 内部其实将这种函数的返回值定为 Unit（类同 Java 的 void 类型），对于这种类型的方法，可以省略掉“=”号，因此如果你希望函数返回某个值，但忘了方法定义中的“=”，Scala 会忽略方法的返回值，而返回 Unit。

再强调一下，Scala 代码无需使用“；”结尾，也不需要使用 return返回值，函数的最后一行的值就作为函数的返回值。

但如果你需要在一行中书写多个语句，此时需要使用“；”隔开，不过不建议这么做。你也可以把一条语句分成几行书写，Scala 编译器大部分情况下会推算出语句的结尾，不过这样也不是一个好的编码习惯。
