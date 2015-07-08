# 组合和继承–调用基类构造函数 #

前面我们定义了两个类，一个为抽象类 Element ，另外一个为派生的实类 ArrayElement。 或许你打算再构造一个新类，这个类使用单个字符串来构造布局元素，使用面向对象的编程方法使得构造这种新类非常容易。比如下面的 LineElement 类：

```
class LineElement(s:String) extends ArrayElement(Array(s)) {
  override def width = s.length
  override def height = 1
}
```

由于 LineElement 扩展了 ArrayElement，并且 ArrayElement 的构造器带一个参数（Array[String]），LineElement 需要传递一个参数到它的基类的主构造器。要调用基类构造器，只要把你要传递的参数或参数列表放在基类名之后的括号里即可。例如，类 LineElement 传递了 Array(s)到 ArrayElement 的主构造器，把它放在基类 ArrayElement 的名称后面的括号里：

```
... extends ArrayElement(Array(s)) ...
```

有了新的子类，布局元素的继承级别现在看起来就如下图所示：

![](images\12.png)

