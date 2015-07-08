# 类和对象 (四) #

## 添加成员变量 ##
本篇继续上一篇，前面我们定义了 Rational 的主构造函数，并检查了输入不允许分母为 0。下面我们就可以开始实行两个 Rational 对象相加的操作。我们需要实现的函数化对象，因此 Rational 的加法操作应该是返回一个新的 Rational 对象，而不是返回被相加的对象本身。我们很可能写出如下的实现：

```
class Rational (n:Int, d:Int) {
   require(d!=0)
   override def toString = n + "/" +d
   def add(that:Rational) : Rational =
     new Rational(n*that.d + that.n*d,d*that.d)
}
```

实际上编译器会给出如下编译错误：

```
<console>:11: error: value d is not a member of Rational
            new Rational(n*that.d + that.n*d,d*that.d)
                                ^
<console>:11: error: value d is not a member of Rational
            new Rational(n*that.d + that.n*d,d*that.d)
```

这是为什么呢？尽管类参数在新定义的函数的访问范围之内，但仅限于定义类的方法本身(比如之前定义的 toString 方法，可以直接访问类参数），但对于 that 来说，无法使用 that.d 来访问 d. 因为 that 不在定义的类可以访问的范围之内。此时需要定类的成员变量。（注：后面定义的 case class 类型编译器自动把类参数定义为类的属性，这是可以使用 that.d  等来访问类参数）。

修改 Rational 定义，使用成员变量定义如下：

```
class Rational (n:Int, d:Int) {
   require(d!=0)
   val number =n
   val denom =d 
   override def toString = number + "/" +denom 
   def add(that:Rational)  =
     new Rational(
       number * that.denom + that.number* denom,
       denom * that.denom
     )
}
```

要注意的我们这里定义成员变量都使用了 val ，因为我们实现的是“immutable”类型的类定义。number 和 denom 以及 add 都可以不定义类型，Scala 编译能够根据上下文推算出它们的类型。

```
scala> val oneHalf=new Rational(1,2)
oneHalf: Rational = 1/2
scala> val twoThirds=new Rational(2,3)
twoThirds: Rational = 2/3
scala> oneHalf add twoThirds
res0: Rational = 7/6
scala> oneHalf.number
res1: Int = 1
```

可以看到，这是就可以使用 .number 等来访问类的成员变量。

## 自身引用 ##
Scala 也使用 this 来引用当前对象本身，一般来说访问类成员时无需使用 this ，比如实现一个 lessThan 方法，下面两个实现是等效的。

```
def lessThan(that:Rational) =
   this.number * that.denom < that.number * this.denom
```

和

```
def lessThan(that:Rational) =
   number * that.denom < that.number * denom
```

但如果需要引用对象自身，this 就无法省略，比如下面实现一个返回两个 Rational 中比较大的一个值的一个实现：

```
def max(that:Rational) =
      if(lessThan(that)) that else this
```

其中的 this 就无法省略。

## 辅助构造函数 ##
在定义类时，很多时候需要定义多个构造函数，在 Scala 中，除主构造函数之外的构造函数都称为辅助构造函数（或是从构造函数），比如对于 Rational 类来说，如果定义一个整数，就没有必要指明分母，此时只要整数本身就可以定义这个有理数。我们可以为 Rational 定义一个辅助构造函数，Scala 定义辅助构造函数使用 this(…)的语法，所有辅助构造函数名称为 this。

```
def this(n:Int) = this(n,1)
```

所有 Scala 的辅助构造函数的第一个语句都为调用其它构造函数，也就是 this(…)，被调用的构造函数可以是主构造函数或是其它构造函数（最终会调用主构造函数），这样使得每个构造函数最终都会调用主构造函数，从而使得主构造函数称为创建类单一入口点。在 Scala 中也只有主构造函数才能调用基类的构造函数，这种限制有它的优点，使得 Scala 构造函数更加简洁和提高一致性。

## 私有成员变量和方法 ##

Scala 类定义私有成员的方法也是使用 private 修饰符，为了实现 Rational 的规范化显示，我们需要使用一个求分子和分母的最大公倍数的私有方法 gcd。同时我们使用一个私有变量 g 来保存最大公倍数,修改 Rational 的定义：

```
scala> class Rational (n:Int, d:Int) {
     |    require(d!=0)
     |    private val g =gcd (n.abs,d.abs) 
     |    val number =n/g 
     |    val denom =d/g 
     |    override def toString = number + "/" +denom
     |    def add(that:Rational)  = 
     |      new Rational( 
     |        number * that.denom + that.number* denom,
     |        denom * that.denom 
     |      ) 
     |    def this(n:Int) = this(n,1) 
     |    private def gcd(a:Int,b:Int):Int =
     |      if(b==0) a else gcd(b, a % b)
     | }
defined class Rational
scala> new Rational ( 66,42)
res0: Rational = 11/7
```

注意 gcd 的定义，因为它是个回溯函数，必须定义返回值类型。Scala 会根据成员变量出现的顺序依次初始化它们，因此g必须出现在 number 和 denom 之前。
