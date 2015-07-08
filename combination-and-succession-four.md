# 组合和继承–扩展类 #
我们需要能够创建新的布局元素对象，前面定义的 Element 为抽象类，不能直接用来创建该类的对象，因此我们需要创建 Element 的子类。这些子类需要实现 Element 类定义的抽象函数。

Scala 中派生子类的方法和 Java一样，也是通过 extends 关键字。比如定义一个 ArrayElement:

```
class ArrayElement(conts: Array[String]) extends Element {
  def contents: Array[String] = conts
}
```

其中 extends 具有两个功效，一是让 ArrayElement 继承所有 Element 类的非私有成员，第二使得 ArrayElement 成为 Element 的一个子类。而 Elemen t称为 ArrayElement 的父类。

如果你在定义类时没有使用 extends 关键字，在 Scala 中这个定义类缺省继承自 scala.AnyRef，如同在 Java 中缺省继承自 java.lang.Object。这种继承关系如下图：

![](images\11.png)

这幅图中也显示了 ArrayElement 和 Array[String] 之间的“组合”关系”(composition)，类 ArrayElement 中定义了对 Array[String]类型对象的一个引用。

ArrayElement 继承了 Element 的所有非私有成员，同时定义了一个 contents 函数，这个函数中其父类（基类）中是抽象的，因此可以说 ArrayElement 中的 contents 函数实现了父类中的这个抽象函数，也可以说“重载”(override)了父类中的同名函数。ArrayElement 继承了 Element 的 width 和 height 方法，因此你可以使用 ArrayElement.width 来查询宽度，比如：

```
scala> val ae=new ArrayElement(Array("hello","world"))
ae: ArrayElement = ArrayElement@729c1e43
```

```
scala> ae.width
res0: Int = 5
```


派生也意味着子类的值可以用在任何可以使用同名父类值的地方，比如：

```
val e: Element = new ArrayElement(Array("hello"))
```
