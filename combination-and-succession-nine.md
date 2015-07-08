# 组合和继承–多态和动态绑定 #

在前面的例子我们看到类型为 Element 的变量可以保存 ArrayElement 类型的对象，这种现象称为“多态”。也就是基类类型的变量可以保存其子类类型的对象，到目前为止我们定义了两个 Element 的子类，ArrayElement 和 LineElement。你还可以定义其它子类，比如：

```
class UniformElement (ch :Char,
  override val width:Int,
  override val height:Int
) extends Element{
  private val line=ch.toString * width
  def contents = Array.fill(height)(line)
}
```

结合前面定义的类定义，我们就有了如下图所示的类层次关系：

![](images\13.png)

Scala 将接受所有的下列赋值，因为赋值表达式的类型符合定义的变量类型：

```
val e1: Element = new ArrayElement(Array("hello", "world")) 
val ae: ArrayElement = new LineElement("hello") 
val e2: Element = ae val 
e3: Element = new UniformElement('x', 2, 3)
```

若你检查继承层次关系，你会发现这四个 val 定义的每一个表达式，等号右侧表达式的类型都在将被初始化的等号左侧的 val 类型的层次之下。

另一方面，如果调用变量（对象）的方法或成员变量，这个过程是一个动态绑定的过程，也就是说调用哪个类型的方法取决于运行时变量当前的类型，而不是定义变量的类型。

为了显示这种行为，我们在 Element 中添加一个 demo 方法，定义如下：

```
abstract class Element { 
  def demo() { 
    println("Element's implementation invoked") 
  } 
} 
```

```
class ArrayElement extends Element { 
  override def demo() { 
    println("ArrayElement's implementation invoked") 
  } 
} 
```

```
class LineElement extends ArrayElement { 
  override def demo() { 
    println("LineElement's implementation invoked")
  }
} 
```

```
// UniformElement inherits Element’s demo 
class UniformElement extends Element
```

如果你使用交互式 Scala 解释器来测试，你可以定义如下的方法：

```
def invokeDemo(e: Element) { 
  e.demo() 
}
```

下面我们分别使用 ArrayElement，LineElement 和 UniformElement 来调用这个方法：

```
scala> invokeDemo(new ArrayElement)
ArrayElement's implementation invoked
```

```
scala> invokeDemo(new LineElement)
LineElement's implementation invoked
```

```
scala> invokeDemo(new UniformElement)
Element's implementation invoked
```

可以看到由于 ArrayElement 和 LineElement 重载了 Element 的 demo 方法，因此调用 invokeDemo 时由于“动态绑定”因此会调用这些子类的 demo 方法，而由于 UniformElement 没有重载 Element 的 demo 方法，动态绑定时也会调用 UniformElement 的 demo 方法（但此时实际为基类的 demo 方法）。
