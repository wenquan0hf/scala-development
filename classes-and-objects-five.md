# 类和对象 (五) #

## 定义运算符 ##

本篇还将接着上篇 Rational 类，我们使用 add 定义两个 Rational 对象的加法。两个 Rational 加法可以写成 x.add(y)或者 x add y 即使使用 x add y 还是没有 x + y 来得简洁。

我们前面说过在 Scala 中运算符（操作符）和普通的方法没有什么区别，任何方法都可以写成操作符的语法。比如：上面的 x add y。而在 Scala 中对方法的名称也没有什么特别的限制，你可以使用符号作为类方法的名称，比如使用+，- * 等符号。因此我们可以重新定义 Rational 如下：

```
class Rational (n:Int, d:Int) {
   require(d!=0)
   private val g =gcd (n.abs,d.abs) 
   val numer =n/g 
   val denom =d/g 
   override def toString = numer + "/" +denom
   def +(that:Rational)  =
     new Rational( 
       numer * that.denom + that.numer* denom,
       denom * that.denom 
     ) 
   def * (that:Rational) =
     new Rational( numer * that.numer, denom * that.denom)
   def this(n:Int) = this(n,1) 
   private def gcd(a:Int,b:Int):Int =
     if(b==0) a else gcd(b, a % b)
}
```

这样就可以使用 + ，* 号来实现 Rational 的加法和乘法。+，\*的优先级是 Scala 预定的，和整数的+，-，* ，/的优先级一样。下面为使用 Rational 的例子：

```
scala> val x= new Rational(1,2)
x: Rational = 1/2
scala> val y=new Rational(2,3)
y: Rational = 2/3
scala> x+y
res0: Rational = 7/6
scala> x+ x*y
res1: Rational = 5/6
```

从这个例子也可以看出 Scala 语言的扩展性，你使用 Rational 对象就像 Scala 内置的数据类型一样。

## Scala中的标识符 ##

从前面的例子我们可以看到 Scala 可以使用两种形式的标志符，字符数字和符号。字符数字使用字母或是下划线开头，后面可以接字母或是数字，符号”$”在 Scala 中也看作为字母。然而以“$”开头的标识符为保留的 Scala 编译器产生的标志符使用，应用程序应该避免使用“$”开始的标识符，以免造成冲突。

Scala 的命名规则采用和 Java 类似的 camel 命名规则，首字符小写，比如 toString。类名的首字符还是使用大写。此外也应该避免使用以下划线结尾的标志符以避免冲突。符号标志符包含一个或多个符号，如+，:，? 等，比如:

\+ ++ ::: < ?> :->

Scala 内部实现时会使用转义的标志符，比如:-> 使用 $colon$minus$greater  来表示这个符号。因此如果你需要在 Java 代码中访问:->方法，你需要使用 Scala 的内部名称 $colon$minus$greater。

混合标志符由字符数字标志符后面跟着一个或多个符号组成，比如 unary_+ 为 Scala 对+方法的内部实现时的名称。字面量标志符为使用“定义的字符串，比如 \`x\` \`yield\`。你可以在“之间使用任何有效的 Scala 标志符，Scala 将它们解释为一个 Scala 标志符，一个典型的使用为 Thread 的 yield 方法， 在 Scala 中你不能使用 Thread.yield()是因为 yield 为 Scala 中的关键字， 你必须使用 Thread.\`yield\`()来使用这个方法。

## 方法重载 ##

和 Java 一样，Scala 也支持方法重载，重载的方法参数类型不同而使用同样的方法名称，比如对于 Rational 对象，+的对象可以为另外一个 Rational 对象，也可以为一个 Int 对象，此时你可以重载+方法以支持和 Int 相加。

```
def + (i:Int) =
     new Rational (numer + i * denom, denom)
```
## 隐式类型转换 ##

上面我们定义 Rational 的加法，并重载+以支持整数，r + 2 ,当如果我们需要 2 + r 如何呢？ 下面的例子：

```
scala> val x =new Rational(2,3)
x: Rational = 2/3
```

```
scala> val y = new Rational(3,7)
y: Rational = 3/7
```

```
scala> val z = 4
z: Int = 4
```

```
scala> x + z
res0: Rational = 14/3
```

```
scala> x + 3
res1: Rational = 11/3
```

```
scala> 3 + x
<console>:10: error: overloaded method value + with alternatives:
  (x: Double)Double <and>
  (x: Float)Float <and>
  (x: Long)Long <and>
  (x: Int)Int <and>
  (x: Char)Int <and>
  (x: Short)Int <and>
  (x: Byte)Int <and>
  (x: String)String
 cannot be applied to (Rational)
              3 + x
                ^
```


可以看到 x+3 没有问题，3 + x 就报错了，这是因为整数类型不支持和 Rational 相加。我们不可能去修改 Int 的定义（除非你重写 Scala 的 Int 定义）以支持 Int 和 Rational 相加。如果你写过 .Net 代码，这可以通过静态扩展方法来实现，Scala 提供了类似的机制来解决这种问题。如果 Int 类型能够根据需要自动转换为 Rational 类型，那么 3 + x  就可以相加。Scala 通过 implicit def 定义一个隐含类型转换，比如定义由整数到 Rational 类型的转换如下：

```
implicit def intToRational(x:Int) = new Rational(x)
```

再重新计算 r+2 和 2 + r 的例子：

```
scala> val r = new Rational(2,3)
r: Rational = 2/3
scala> r + 2
res0: Rational = 8/3
scala> 2 + r
res1: Rational = 8/3
```

其实此时 Rational 的一个+重载方法是多余的， 当 Scala 计算 2+ r，发现 2（Int)类型没有 可以和 Rational 对象相加的方法，Scala 环境就检查 Int 的隐含类型转换方法是否有合适的类型转换方法，类型转换后的类型支持+r，一检查发现定义了由 Int 到 Rational 的隐含转换方法，就自动调用该方法，把整数转换为 Rational 数据类型，然后调用 Rational 对象的+ 方法。从而实现了 Rational 类或是 Int 类的扩展。关于 implicit def 的详细介绍将由后面的文章来说明，隐含类型转换在设计 Scala 库时非常有用。
