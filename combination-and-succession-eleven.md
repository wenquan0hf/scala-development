# 组合和继承–使用组合还是继承？ #
前面我们说过，构建新类的两个基本方法是组合和继承，如果你的主要目的是代码重用，那么最好使用组合的方法构造新类，使用继承的方法构造新类造成的可能问题是，无意的修改基类可能会破坏子类的实现。

关于继承关系你可以问自己一个问题，是否它建模了一个 is-a 关系。例如，说 ArrayElement 是 Element 是合理的。你能问的另一个问题是，是否客户想要把子类类型当作基类类型来用。

前一个版本中，LineElement 与 ArrayElement 有一个继承关系，从那里继承了 contents。现在它在 ArrayElement 的例子里，我们的确期待客户会想要把 ArrayElement 当作 Element 使用。

请看下面的类层次关系图：

![](images\14.png)

看着这张图，问问上面的问题,是否感觉其中的任何关系有可疑吗？尤其是，对你来说 LineElement 是 ArrayElement 是否显而易见呢？你是否认为客户会需要把 LineElement 当作 ArrayElement 使用？实际上，我们把 LineElement 定义为 ArrayElement 主要是想重用 ArrayElement 的 contents 定义。因此或许把 LineElement 定义为 Element 的直接子类会更好一些，就像这样：

```
 class LineElement(s: String) extends Element {  
   val contents = Array(s)  
   override def width = s.length  
   override def height = 1 
  } 
```

前一个版本中，LineElemen t与 ArrayElement 有一个继承关系，从那里继承了 contents。现在它与 Array 有一个组合关系：在它自己的 contents 字段中持有一个字串数组的引用。有了 LineElement 的这个实现，Element 的继承层级现在如下图所示：

![](images\15.png)

因此在选用组合还是通过继承来构造新类时，需要根据需要选择合适的方法。
