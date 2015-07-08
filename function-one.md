# 函数–类成员函数 #
当程序越来越大，你需要将代码细化为小的容易管理的模块。Scala 支持多种方法来细化程序代码，这些方法也为有经验的程序员已经掌握的：使用函数，和 Java 相比，Scala 提供了多种 Java 不支持的方法来定义函数，除了类成员函数外，Scala 还支持嵌套函数，函数字面量，函数变量等。

本篇先介绍类或对象的成员函数。这也是最常见的定义函数的方法。例如下面的例子定义了两个成员函数：

```
import scala.io.Source
object LongLines {
  def processFile(filename: String, width: Int) {
    val source= Source.fromFile(filename)
    for (line <- source.getLines())
      processLine(filename,width,line)
   }
   private def processLine(filename:String,
     width:Int, line:String){
     if(line.length > width)
       println(filename + ":" +line.trim)
   }
}
```

成员函数 processFile 使用两个参数，一个文件名，一个为字符长度，其作用是打印出文件中超过指定字符长度的所有行。它调用另外一个私有成员函数 processLine 完成实际的操作。

这个成员函数，如果作为脚本使用，可以使用如下代码：

```
LongLines.processFile(args(0),args(1).toInt)
```

可以看到 Scala 类成员函数的使用方法和其它面向对象的程序语言如 Java 基本一致。在后面的文章将介绍 Scala 函数不同于 Java 的一些特性。
