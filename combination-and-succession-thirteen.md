# 组合和继承–定义 factory 对象 #

到目前为止，我们定义了关于布局元素类的一个层次结构。你可以把包含这个层次关系的类作为 API 接口提供给其它应用，但有时你可以希望对函数库的用户隐藏这种层次关系，这通常可以使用 factory(构造工厂）对象来实现。一个 factory 对象定义了用来构造其它对象的函数。库函数的用户可以通过工厂对象来构造新对象，而不需要通过类的构造函数来创建类的实例。使用工厂对象的好处是，可以统一创建对象的接口并且隐藏被创建对象具体是如何来表示的。这种隐藏可以使得你创建的函数库使用变得更简单和易于理解，也正是隐藏部分实现细节，可以使你有机会修改库的实现而不至于影响库的接口。

实现 factory 对象的一个基本方法是采用 singleton 模式，在 Scala 中，可以使用类的伴随对象(companion 对象）来实现。比如：

```
object Element {
  def elem(contents: Array[String]):Element =
   new ArrayElement(contents)
  def elem(chr:Char, width:Int, height:Int) :Element =
    new UniformElement(chr,width,height)
  def elem(line:String) :Element =
    new LineElement(line)
}
```

我们先把之前 Element 的实现列在这里：

```
abstract class Element {
  def contents: Array[String]
  def height: Int = contents.length
  def width: Int = if (height == 0) 0 else contents(0).length
  def above(that: Element) :Element =
    new ArrayElement(this.contents ++ that.contents)
  def beside(that: Element) :Element = {
    new ArrayElement(
      for(
        (line1,line2) <- this.contents zip that.contents
      ) yield line1+line2
    )
  }
  override def toString = contents mkString "\n"
}
```

有了 object Element（类 Element 的伴随对象），我们可以利用 Element 对象提供的 factory 方法，重新实现类 Element 的一些方法：

```
abstract class Element {
  def contents: Array[String]
  def height: Int = contents.length
  def width: Int = if (height == 0) 0 else contents(0).length
  def above(that: Element) :Element =
    Element.elem(this.contents ++ that.contents)
  def beside(that: Element) :Element = {
    Element.elem(
      for( 
        (line1,line2) <- this.contents zip that.contents
      ) yield line1+line2
    ) 
  }
  override def toString = contents mkString "\n"
}
```

这里我们重写了 above 和 beside 方法，使用伴随对象的 factory 方法 Element.elem 替代 new  构造函数。

这样修改之后，库函数的用户不要了解 Element 的继承关系，甚至不需要知道类 ArrayElement，LineElement 定义的存在，为了避免用户直接使用 ArrayElement 或 LineElement 的构造函数来构造类的实例，因此我们可以把 ArrayElement，UniformElement 和 LineElement 定义为私有，定义私有可以也可以把它们定义在类 Element 内部（嵌套类）。下面为这种方法的使用：

```
object Element {
  private class ArrayElement(val contents: Array[String])
    extends Element {
  }
  private class LineElement(s:String) extends ArrayElement(Array(s)) {
    override def width = s.length
    override def height = 1
  }
  private class UniformElement (ch :Char,
    override val width:Int,
    override val height:Int
  ) extends Element{
    private val line=ch.toString * width
    def contents = Array.fill(height)(line)
  }
  def elem(contents: Array[String]):Element =
   new ArrayElement(contents)
  def elem(chr:Char, width:Int, height:Int) :Element =
    new UniformElement(chr,width,height)
  def elem(line:String) :Element =
    new LineElement(line)
}
```
