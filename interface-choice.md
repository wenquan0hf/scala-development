# 选择瘦接口还是胖接口设计？ #

Trait 的一种主要应用方式是可以根据类已有的方法自动为类添加方法。也就是说，Trait 可以使得一个瘦接口变得丰满些，把它变成胖接口。

选择瘦接口还是胖接口的体现了面向对象设计中常会面临的在实现者与接口用户之间的权衡。胖接口有更多的方法，对于调用者来说更便捷。客户可以捡一个完全符合他们功能需要的方法。另一方面瘦接口有较少的方法，对于实现者来说更简单。然而调用瘦接口的客户因此要写更多的代码。由于没有更多可选的方法调用，他们或许不得不选一个不太完美匹配他们所需的方法并为了使用它写一些额外的代码。

Java 的接口常常是过瘦而非过胖。例如，从 Java 1.4 开始引入的 CharSequence 接口，是对于字串类型的类来说通用的瘦接口，它持有一个字符序列。下面是把它看作 Scala 中 Trait 的定义：

```
trait CharSequence { 
  def charAt(index: Int): Char 
  def length: Int 
  def subSequence(start: Int, end: Int): CharSequence 
  def toString(): String 
}
```

尽管类 String 成打的方法中的大多数都可以用在任何 CharSequence 上，Java 的 CharSequence 接口定义仅提供了 4 个方法。如果 CharSequence 代以包含全部 String 接口，那它将为 CharSequence 的实现者压上沉重的负担。任何实现 Java 里的 CharSequence 接口的程序员将不得不定义一大堆方法。因为 Scala 的 Trait 可以包含具体方法，这使得创建胖接口大为便捷。

在 Trait 中添加具体方法使得胖瘦对阵的权衡大大倾向于胖接口。不像在 Java 里那样，在 Scala 中添加具体方法是一次性的劳动。你只要在 Trait 中实现方法一次，而不再需要在每个混入 Trait 的方法中重新实现它。因此，与没有 Trait 的语言相比，Scala 里的胖接口没什么工作要做。

要使用 Trait 加强接口，只要简单地定义一个具有少量抽象方法的 Trait——Trait 接口的瘦部分——和潜在的大量具体方法，所有的都实现在抽象方法之上。然后你就可以把丰满了的 Trait 混入到类中，实现接口的瘦部分，并最终获得具有全部胖接口内容的类。
