# 组合和组合和继承–定义heighten和widen函数 #
我们还需要最后一个改进，之前的 Element 实现不够完善，只支持同样高度和同样宽度的 Element 使用 above 和 beside 函数，比如下面的代码将无法正常工作，因为组合元素的第二行比第一行要长：

```
new ArrayElement(Array("hello")) above 
new ArrayElement(Array("world!"))
```

与之相似的，下面的表达式也不能正常工作，因为第一个 ArrayElement 高度为二，而第二个的高度只是一

```
new ArrayElement(Array("one", "two")) beside 
new ArrayElement(Array("one"))
```

下面代码展示了一个私有帮助方法，widen，能够带个宽度做参数并返回那个宽度的 Element。结果包含了这个 Element 的内容，居中，左侧和右侧留需带的空格以获得需要的宽度。这段代码还展示了一个类似的方法，heighten，能在竖直方向执行同样的功能。widen 方法被 above 调用以确保 Element 堆叠在一起有同样的宽度。类似的，heighten 方法被 beside 调用以确保靠在一起的元素具有同样的高度。有了这些改变，布局库函数可以使用了

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
  def widen(w: Int): Element =
  if (w <= width) this
  else {
    val left = Element.elem(' ', (w - width) / 2, height)
        var right = Element.elem(' ', w - width - left.width, height)
        left beside this beside right
  } 
def heighten(h: Int): Element =
  if (h <= height) this  
  else {  
    val top = Element.elem(' ', width, (h - height) / 2)
        var bot = Element.elem(' ', width, h - height - top.height)
        top above this above bot
  }  
  override def toString = contents mkString "\n"\
}
```
