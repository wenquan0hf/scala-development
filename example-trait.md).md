# Trait示例–Rectangular 对象 #
在设计绘图程序库时常常需要定义一些具有矩形形状的类型：比如窗口，bitmap 图像，矩形选取框等。为了方便使用这些矩形对象，函数库对象类提供了查询对象宽度和长度的方法（比如 width，height)和坐标的 left，right，top 和 bottom 等方法。然而在实现这些函数库的这样方法，如果使用 Java 来实现，需要重复大量代码，工作量比较大（这些类之间不一定可以定义继承关系）。但如果使用 Scala 来实现这个图形库，那么可以使用 Trait，为这些类方便的添加和矩形相关的方法。

首先我们先看看如果使用不使用 Trait 如何来实现这些类，首先我们定义一些基本的几何图形类如 Point  和Rectangle：

```
class Point(val x:Int, val y:Int)
```

```
class Rectangle(val topLeft:Point, val bottomRight:Point){
  def left =topLeft.x
  def right =bottomRight.x
  def width=right-left 
  // and many more geometric methods
}
```

这里我们定义一个点和矩形类，Rectangle 类的主构造函数使用左上角和右下角坐标，然后定义了  left，right，和 width 一些常用的矩形相关的方法。

同时，函数库我们可能还定义了一下 UI 组件（它并不是使用 Retangle 作为基类），其可能的定义如下：

```
abstract class Component {
  def topLeft :Point
  def bottomRight:Point
  def left =topLeft.x
  def right =bottomRight.x
  def width=right-left
  // and many more geometric methods
}
```

可以看到 left，right，width 定义和 Rectangle 的定义重复。可能函数库还会定义其它一些类，也可能重复这些定义。

如果我们使用 Trait，就可以消除这些重复代码，比如我们可以定义如下的 Rectangular Trait 类型：

```
trait Rectangular {
  def topLeft:Point
  def bottomRight:Point
  def left =topLeft.x
  def right =bottomRight.x
  def width=right-left
  // and many more geometric methods
}
```

然后我们修改 Component 类定义使其“融入”Rectangular 特性：

```
abstract class Component extends Rectangular{
 //other methods
}
```

同样我们也修改 Rectangle 定义：

```
class Rectangle(val topLeft:Point, val bottomRight:Point) extends Rectangular{
  // other methods
}
```

这样我们就把矩形相关的一些属性和方法抽象出来，定义在 Trait 中，凡是“混合”了这个 Rectangluar 特性的类自动包含了这些方法：

```
object TestConsole extends App{
   val rect=new Rectangle(new Point(1,1),new Point(10,10))
    println (rect.left)
    println(rect.right)
    println(rect.width)
}
```

运行结果如下：

```
1
 10
 9
```