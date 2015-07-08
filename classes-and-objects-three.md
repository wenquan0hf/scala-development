# 类和对象 (三) #

有了前面的 Scala 的基本知识，本篇介绍如何定义完整功能的 Scala 类定义。本篇着重介绍如果定义 Functional objects (函数化对象或是方程化对象），函数化对象指的是所定义的类或对象不包含任何可以修改的状态。
 本篇定义了一个有理数类定义的几个不同版本以介绍 Scala 类定义的几个特性：类参数和构造函数，方法，操作符，私有成员，重载，过载，条件检查，引用自身。

## Rational类定义规范 ##

首先，我们回忆下有理数的定义：一个有理数(rational)可以表示成个分数形式： n/d, 其中 n 和 d 都是整数（d 不可以为 0），n 称为分子(numberator)，d 为分母(denominator)。和浮点数相比，有理数可以精确表达一个分数，而不会有误差。

因此我们定义的 Rational 类支持上面的有理数的定义。支持有理数的加减乘除，并支持有理数的规范表示，比如 2/10，其规范表示为 1/5。分子和分母的最小公倍数为 1。

## 定义Rational ##

有了有理数定义的实现规范，我们可以开始设计类 Rational，一个好的起点是考虑用户如何使用这个类，我们已经决定使用“Immutable”方式来使用 Rational 对象，我们需要用户在定义 Rational 对象时提供分子和分母。因此我们可以开始定义 Rational 类如下：

```
class Rational( n:Int, d:Int)
```

可以看到和 Java 不同的，Scala 的类定义可以有参数，称为类参数，如上面的 n,d。Scala 使用类参数，并把类定义和主构造函数合并在一起，在定义类的同时也定义了类的主构造函数。因此 Scala 的类定义相对要简洁些。

Scala 编译器会编译 Scala 类定义包含的任何不属于类成员和类方法的其它代码，这些代码将作为类的主构造函数，比如我们定义一条打印消息作为类定义的代码：

```
scala> class Rational (n:Int, d:Int) {
     |    println("Created " + n + "/" +d)
     | }
defined class Rational
scala> new Rational(1,2)
Created 1/2
res0: Rational = Rational@22f34036
```


可以看到创建 Ratiaonal 对象时，自动执行类定义的代码（主构造函数）。

## 重新定义类的toString 方法 ##

上面的代码创建 Rational（1，2），Scala 编译器打印出 Rational@22f34036，这是因为使用了缺省的类的 toString()定义（Object 对象的），缺省实现是打印出对象的类名称+“@”+16 进制数（对象的地址），显示结果不是很直观，因此我们可以重新定义类的 toString()方法以显示更有意义的字符。

在 Scala 中，你也可以使用 override 来重载基类定义的方法，而且必须使用 override 关键字表示重新定义基类中的成员。比如：

```
scala> class Rational (n:Int, d:Int) {
     |    override def toString = n + "/" +d
     | }
defined class Rational
scala> val x= new Rational(1,3)
x: Rational = 1/3
scala> val y=new Rational(5,7)
y: Rational = 5/7
```

## 前提条件检查 ##

前面说过有理数可以表示为 n/d (其中 d,n 为证书，而 d 不能为 0），对于前面的 Rational 定义，我们如果使用 0，也是可以的

```
scala> new Rational(5,0)
res0: Rational = 5/0
```

怎么解决分母不能为 0 的问题了，面向对象编程的一个优点是实现了数据的封装，你可以确保在其生命周期过程中是有效的。对于有理数的一个前提条件是分母不可以为 0，Scala 中定义为传入构造函数和方法的参数的限制范围，也就是调用这些函数或方法的调用者需要满足的条件。Scala 中解决这个问题的一个方法是使用 require 方法（require 方法为 Prede f对象的定义的一个方法，Scala 环境自动载入这个类的定义，因此无需使用 import 引入这个对象），因此修改 Rational 定义如下：

```
scala> class Rational (n:Int, d:Int) {
     |    require(d!=0)
     |    override def toString = n + "/" +d
     | }
defined class Rational
scala> new Rational(5,0)
java.lang.IllegalArgumentException: requirement failed
  at scala.Predef$.require(Predef.scala:211)
  ... 33 elided
```

可以看到如果再使用 0 作为分母，系统将抛出 IllegalArgumentException 异常。
