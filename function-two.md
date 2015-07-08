# 函数–局部函数 #
上个例子中 ProcessFile 使用了一个非常重要的设计原则–应用程序可以分解成多个小的函数，每个小的函数完成一个定义完好的功能。使用这程序设计风格可以使得程序员有相当数量的程序构造模块，通过这些小的构造模块的组合来完成较复杂的功能。每个小的构造模块一个应该足够简洁以帮助理解。

这样带来的一个问题是，这些小的辅助函数的名称可能会影响到程序空间，你不能在同个程序员使用两个相同名称的函数，即使你定义私有函数。如果你设计函数库，你也不希望有些辅助函数为库函数的用户直接调用，对于 Java 来说，你可以通过私有成员函数。而 Scala 除了支持私有成员函数外，还支持局部函数（其作用域和局部变量类似）。
  
也就是说可以在函数的内部再定义函数，如同定义一个局部变量，例如，修改前面的 ProcessFile 的例子如下：

```
import scala.io.Source
object LongLines {
  def processFile(filename: String, width: Int) {
    def processLine(filename:String,
     width:Int, line:String){
     if(line.length > width)
       println(filename + ":" +line.trim)
   }
    val source= Source.fromFile(filename)
    for (line <- source.getLines())
      processLine(filename,width,line)
   }
}
```

这个例子不私有成员函数 processLine 移动到 processFile 内部，成为一个局部函数，也正因为 processLine 变成了 processFile 的一个局部函数，因此 processLine 可以直接访问到 processFile 的参数 filename 和 width,因此代码可以进一步优化如下：

```
import scala.io.Source
object LongLines {
  def processFile(filename: String, width: Int) {
    def processLine(line:String){
     if(line.length > width)
       println(filename + ":" +line.trim)
   }
    val source= Source.fromFile(filename)
    for (line <- source.getLines())
      processLine(line)
   }
}
```

代码变得更简洁，是不是，局部函数的作用域和局部变量作用域一样，局部函数访问包含该函数的参数是非常常见的一种嵌套函数的用法。
