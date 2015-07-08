# 组合和继承–实现类 Element 的 above，beside 和 toString()方法 #
我们接着实现类 Element 的其它方法，如 above, beside 和 toString 方法。

above 方法，意味着把一个布局元素放在另外一个布局元素的上方，也就是把这两个元素的 contents 的内容连接起来。我们首先实现 above 函数的第一个版本：

```
def above(that: Element) :Element =
    new ArrayElement(this.contents ++ that.contents)
```

Scala 中 Array 使用 Java Array 来实现，但添加很多其它方法，尤其是 Scala 中 Array 可以转换为 scala.Seq 类的实例对象，scala.Seq 为一个序列结构并提供了许多方法来访问和转换这个序列。

实际上上面 above 的实现不是十分有效，因为它不允许你把不同长度的布局元素叠加到另外一个布局元素上面，但就目前来说，我们还是暂时使用这个实现，只使用同样长度的布局元素，后面再提供这个版本的增强版本。

下面我们再实现类 Element 的另外一个 beside 方法，把两个布局元素并排放置，和前面一样，为简单起见，我们暂时只考虑相同高度的两个布局元素：

```
def beside(that: Element) :Element = {
    val contents = new Array[String](this.contents.length)
    for(i <- 0 until this.contents.length)
      contents(i)=this.contents(i) + that.contents(i)
    new ArrayElement(contents)
  }
```

尽管上面的实现满足 beside 要求，但采用的还是指令式编程，我们使用函数说编程可以实现下面简化代码：

```
def beside(that: Element) :Element = {
    new ArrayElement(
      for(
        (line1,line2) <- this.contents zip that.contents
      ) yield line1+line2
    )
  } 
```

这里我们使用了 Array 的 zip 操作符，可以用来将两个数组转换成二元组的数组，zip 分别取两个数组对应的元素组成一个新的二元祖，比如：

```
scala> Array( 1,2,3) zip Array("a","b")
res0: Array[(Int, String)] = Array((1,a), (2,b))
```

如果一个数组长度大于另外一个数组，多余的元素被忽略。 for 的 yield 部分用来构成一个新元素。

最后我们实现 Element 的 toString 方法用来显示布局元素的内容：

```
override def toString = contents mkString "\n"
```

这里使用 mkString 函数，这个函数可以应用到任何序列数据结构（包括数组），也就是把 contents 的每个元素调用 toString，然后使用“\n”分隔。
