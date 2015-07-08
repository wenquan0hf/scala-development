# 组合和继承–小结 #

前面我们基本完成了布局元素的函数库，现在我们就可以写个程序来使用这个函数库，下面显示螺旋线的程序如下：

```
object Spiral {
  val space = elem (" ")
  val corner = elem ("+")
  def spiral(nEdges:Int, direction:Int): Element = {
    if(nEdges==1)
      elem("+")
    else{
      val sp=spiral(nEdges -1, (direction +3) % 4)
      def verticalBar = elem ('|',1, sp.height)
      def horizontalBar = elem('-',sp.width,1)
      if(direction==0)
        (corner beside horizontalBar) above (sp beside space)
      else if (direction ==1)
        (sp above space) beside ( corner above verticalBar)
      else if(direction ==2 )
        (space beside sp) above (horizontalBar beside corner)
      else
        (verticalBar above corner) beside (space above sp)
  }
}
 def main(args:Array[String]) {
   val nSides=args(0).toInt
   println(spiral(nSides,0))
 }
}
```

因为 Sprial 为一单例对象，并包含 main 方法，因此它为一 Scala 应用程序，可以在命令行使用 scala Sprial xx 来运行这个应用。

```
root@mail:~/scala# scala Spiral 5
+----
|    
| ++ 
|  | 
+--+ 
root@mail:~/scala# scala Spiral 23
+----------------------
|                      
| +------------------+ 
| |                  | 
| | +--------------+ | 
| | |              | | 
| | | +----------+ | | 
| | | |          | | | 
| | | | +------+ | | | 
| | | | |      | | | | 
| | | | | +--+ | | | | 
| | | | | |  | | | | | 
| | | | | ++ | | | | | 
| | | | |    | | | | | 
| | | | +----+ | | | | 
| | | |        | | | | 
| | | +--------+ | | | 
| | |            | | | 
| | +------------+ | | 
| |                | | 
| +----------------+ | 
|                    | 
+--------------------+ 
```

这个例子的完整代码如下：

```
object Element {
  private class ArrayElement(val contents: Array[String])
    extends Element
  private class LineElement(s:String) extends Element {
    val contents=Array(s)
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
import Element.elem
abstract class Element {
  def contents: Array[String]
  def height: Int = contents.length
  def width: Int =  contents(0).length
  def above(that: Element) :Element = {
    val this1=this widen that.width
    val that1=that widen this.width
    elem (this1.contents ++ that1.contents)
  }
  def beside(that: Element) :Element = {
    val this1=this heighten that.height
    val that1=that heighten this.height
    Element.elem(
      for(
        (line1,line2) <- this1.contents zip that1.contents
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
  override def toString = contents mkString "\n"
}
object Spiral {
  val space = elem (" ")
  val corner = elem ("+")
  def spiral(nEdges:Int, direction:Int): Element = {
    if(nEdges==1)
      elem("+")
    else{
      val sp=spiral(nEdges -1, (direction +3) % 4)
      def verticalBar = elem ('|',1, sp.height)
      def horizontalBar = elem('-',sp.width,1)
      if(direction==0)
        (corner beside horizontalBar) above (sp beside space)
      else if (direction ==1)
        (sp above space) beside ( corner above verticalBar)
      else if(direction ==2 )
        (space beside sp) above (horizontalBar beside corner)
      else
        (verticalBar above corner) beside (space above sp)
  }
}
 def main(args:Array[String]) {
   val nSides=args(0).toInt
   println(spiral(nSides,0))
 }
}
```

到目前为止，你看到了 Scala 里与面向对象编程有关的更多的概念。其中，你遇到了抽象类，继承和派生，类层次，参数化字段，及方法重载。你应当已经建立了在 Scala 里构造不太小的类层次的感觉。
