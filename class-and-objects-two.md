# 类和对象 (二) #

前面提到 Scala 比 Java 更加面向对象，这是因为 Scala 不允许类保护静态元素(静态变量或静态方法）。在 Scala 中提供类似功能的是成为“[Singleton（单例对象）](https://en.wikipedia.org/wiki/Singleton_pattern)”的对象。在 Scala 中定义 Singleton 对象的方法除了使用 object，而非 class 关键字外和类定义非常类似，下面例子创建一个 ChecksumAccumulator 对象：

```
object ChecksumAccumulator {
   private val cache = Map [String, Int] ()
   def calculate(s:String) : Int =
      if(cache.contains(s))
         cache(s)
      else {
         val acc=new ChecksumAccumulator
         for( c <- s)
            acc.add(c.toByte)
         val cs=acc.checksum()
         cache += ( s -> cs)
         cs
       }
}
```

这个对象和上一篇创建的类 ChecksumAccumulator 同名，这在 Scala 中把这个对象成为其同名的类的“伴侣”对象（ Companion object )。 如果你需要定义的类的 companion 对象，Scala 要求你把这两个定义放在同一个文件中。类和其 companion 对象可以互相访问对方的私有成员。

如果你是 Java 成员，可以把 Singleton 对象看成以前 Java 定义静态成员的地方。你可以使用类似 Java 静态方法的方式调用 Singleton 对象的方法，比如下面为这个例子完整的代码：

```
import scala.collection.mutable.Map
class ChecksumAccumulator{
   private var sum=0
   def add(b:Byte) :Unit = sum +=b
   def checksum() : Int = ~ (sum & 0xFF) +1
}

object ChecksumAccumulator {
   private val cache = Map [String, Int] ()
   def calculate(s:String) : Int =
      if(cache.contains(s))
         cache(s)
      else {
         val acc=new ChecksumAccumulator
         for( c <- s)
            acc.add(c.toByte)
         val cs=acc.checksum()
         cache += ( s -> cs)
         cs
       }
}

println ( ChecksumAccumulator.calculate("Welcome to Scala Chinese community"))
```

Scala 的 singleton 对象不仅限于作为静态对象的容器，它在 Scala 中也是头等公民，但仅仅定义 Singleton 对象本身不会创建一个新的类型，你不可以使用 new 再创建一个新的 Singleton 对象（这也是 Singleton 名字的由来），此外和类定义不同的是，singleton 对象不可以带参数（类定义参数将在后面文章介绍）。

回过头来看看我们的第一个例子 “Hello World“。

```
object HelloWorld {
  def main(args: Array[String]) {
    println("Hello, world!")
  }
}
```

这是一个最简单的 Scala 程序,HelloWorld 是一个 Singleton 对象，它包含一个 main 方法（可以支持命令行参数），和 Java 类似，Scala 中任何 Singleto 对象，如果包含 main 方法，都可以作为应用的入口点。

在这里要说明一点的是，在 Scala 中不要求 public 类定义和其文件名同名，不过使用和 public 类和文件同名还是有它的优点的，你可以根据个人喜好决定是否遵循 Java 文件命名风格。

最后提一下 Scala 的 trait 功能，Scala 的 trait 和 Java 的 Interface 相比，可以有方法的实现（这点有点像抽象类,但如果是抽象类，就不会允许继承多个抽象类）。Scala 的 Trait 支持类和 Singleton 对象和多个 Trait 混合（使用来自这些 Trait 中的方法，而不时不违反单一继承的原则）。

Scala 为 Singleton 对象的 main 定义了一个 App trait 类型，因此上面的例子可以简化为：

```
object HelloWorld extends App{
   println("Hello, world!")
}
```

这段代码就不能作为脚本运行，Scala 的脚本要求代码最后以表达式结束。因此运行这段代码，需要先编译这段代码：

```
scalac Helloworld.scala
```

编译好之后，运行该应用

```
scala HelloWord
```

`注意`： Scala 提供了一个快速编译代码的辅助命令 fsc (fast scala compliler) ，使用这个命令，只在第一次使用 fsc时启动 JVM，之后 fsc 在后台运行，这样就避免每次使用 scalac 时都要载入相关库文件，从而提高编译速度。
