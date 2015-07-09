# Ordered Trait #

比较对象也是胖接口来的比较方便的一个应用领域，当你需要比较两个有顺序关系的对象时，如果只需要一个方法就可以知道需要比较的结果就非常便利。比如，你需要“小于”关系，你希望使用“< “比较就可以了，如果是“小于等于”，使用”<=”就可以。如果使用瘦接口来定义类，也许你只定义了一个<比较方法，那么如果需要小于等于，你可能需要使用(x<y)|| (x==y)。一个胖接口定义了所有可能的比较运算符，使得你可以直接使用<=来书写代码。

但胖接口带来的便利也是有代价的,回头看看我们前面定义的[Rational类](http://www.imobilebbs.com/wordpress/archives/4826)；

如果我们需要定义比较操作，则需要定义如下代码：

```
class Rational (n:Int, d:Int) {
 ...
  def < (that:Rational) =     this.numer * that.denom  > that.numer * this.denom
  def > (that:Rational) = that < this
  def <=(that:Rational) = (this < that) || (this == that)   def >=(that:Rational) = (this  > that) || (this == that)
}
```

这个类定义了四个比较运算符 < ，>，< =和>=。首先我们注意到后面三个比较运算符，都是通过第一个比较运算符来实现的。其次，我们也可以看到，后面三个比较操作对于任意对象都是适用的，和对象的类型无关。而需要实现这四个比较运算的胖接口都要重复这些代码。

Scala 对于比较这种常见的操作，提供了 Ordered Trait 定义，使用它，你可以把所有的比较运算的代码使用一个 compare 定义来替代。这个 ordered trait 可以让需要实现比较运算的类，通过和 Ordered 特质“融合”，而只需实现一个 compare 方法即可。

因此我们可以修改前面的实现如下：

```
class Rational (n:Int, d:Int) extends Ordered[Rational]{
  ...
  override def compare (that:Rational)=
    (this.numer*that.denom)-(that.numer*that.denom)
}
```

要注意两点，一是 Ordered 需要指明类型参数 Ordered[T] ，类型参数我们将在后面介绍，这里只需要知道添加所需比较类型的类名称，本例为 Rational，此外，需要使用 compare 方法。 它比较有序对象，=0，表示两个对象相同，>0表示前面大于后面对象，<0表示前面小于后面对象。

下面为测试结果：

```
scala> class Rational (n:Int, d:Int) extends Ordered[Rational]{
     |   require(d!=0)
     |   private val g =gcd (n.abs,d.abs)
     |   val numer =n/g
     |   val denom =d/g
     |   override def toString = numer + "/" +denom
     |   def +(that:Rational)  =
     |     new Rational(
     |       numer * that.denom + that.numer* denom,
     |       denom * that.denom
     |     )
     |   def * (that:Rational) =
     |     new Rational( numer * that.numer, denom * that.denom)
     |   def this(n:Int) = this(n,1)
     |   private def gcd(a:Int,b:Int):Int =
     |     if(b==0) a else gcd(b, a % b)
     |
     |   override def compare (that:Rational)=
     |     (this.numer*that.denom)-(that.numer*that.denom)
     |
     | }
defined class Rational
scala> val half =new Rational(1,2)
half: Rational = 1/2
scala>   val third=new Rational(1,3)
third: Rational = 1/3
scala> half < third res0: Boolean = false
scala> half >= third
res1: Boolean = true
```

因此，你在需要实现比较对象时，首先需要考虑 Ordered Trait，看看这个 Trait 能否满足要求，然后通过和这个 Trait “混合”就可以很方便的实现对象之间的比较。

此外要注意  Ordered Trait 没有定义equal 方法，因为如果需要定义 equal 方法，那么需要检查传入参数的类型，Ordered Trait 无法实现，因此你如果需要==比较运算符，需要另外定义。
