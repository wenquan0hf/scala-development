# 减低代码重复 #

在前面的文章中，我们说过 Scala 没有内置很多控制结构，这是因为 Scala 赋予了程序员自己扩展控制结构的能力。Scala 支持函数值（值的类型为函数，而非函数的返回值），为避免混淆，我们使用函数类型值来指代类型为函数的值。

所有的函数可以分成两个部分：一是共有部分，这部分在该函数的调用都是相同的，另外一部分为分公共部分，这部分在每次调用该函数上是可以不同的。公共部分为函数的定义体，非公共部分为函数的参数。但你使用函数类型值做为另外一个函数的参数时，函数的非公共部分本身也是一个算法（函数），调用该函数时，每次你都可以传入不同函数类型值作为参数，这个函数称为高阶函数–函数的参数也可以是另外一个函数。

使用高级函数可以帮助你简化代码，它支持创建一个新的程序控制结构来减低代码重复。比如，你打算写一个文件浏览器，你需要写一个 API 支持搜索给定条件的文件。首先，你添加一个方法，该方法可以通过查询包含给定字符串的文件，比如你可以查所有“.scala”结尾的文件。你可以定义如下的 API：

```
object FileMatcher {
  private def filesHere = (new java.io.File(".")).listFiles
  def filesEnding(query : String) =
    for (file <-filesHere; if file.getName.endsWith(query))
      yield file
}
```

filesEnding 方法从本地目录获取所有文件（方法 filesHere)，然后使用过滤条件（文件以给定字符串结尾）输出给定条件的文件。

到目前为止，这代码实现非常好也没有什么重复的代码。后来，你有需要使用新的过滤条件，文件名包含指定字符串，而不仅仅以某个字符串结尾的文件列表。你有实现了下面的 API。

```
def filesContaining( query:String ) =
    for (file <-filesHere; if file.getName.contains(query))
      yield file
```

filesContaining 和 filesEnding 的实现非常类似，不同点在于一个使用 endsWith,另一个使用 contains 函数调用。有过了一段时间，你有想支持使用正则表达式来查询文件，你有实现了下面的对象方法：

```
def filesRegex( query:String) =
   for (file <-filesHere; if file.getName.matches(query))
      yield file
```

这三个函数的算法非常类似，所不同的是过滤条件稍有不同，在 Scala 中我们可以定义一个高阶函数，将这三个不同过滤条件抽象称一个函数作为参数传给搜索算法，我们可以定义这个高阶函数如下：

```
def filesMatching( query:String, 
    matcher: (String,String) => Boolean) = {
    for(file <- filesHere; if matcher(file.getName,query))
      yield file
   }
```

这个函数的第二个参数 matcher 的类型也为函数（如果你熟悉 C#，类似于 delegate)，该函数的类型为 (String,String ) =>Boolean，可以匹配任意使用两个 String 类型参数，返回值类型为 Boolean 的函数。使用这个辅助函数，我们可以重新定义 filesEnding，filesContaining 和 filesRegex。

```
def filesEnding(query:String) =
   filesMatching(query,_.endsWith(_))
```

```
def filesContaining(query:String)=
   filesMatching(query,_.contains(_))
```

```
def filesRegex(query:String) =
   filesMatching(query,_.matches(_))
```

这个新的实现和之前的实现已经简化了不少，实际上代码还可以简化，我们注意到参数 query 在 filesMatching 的作用只是把它传递给 matcher 参数，这种参数传递实际也是无需的，简化后代码如下：

```
object FileMatcher {
  private def filesHere = (new java.io.File(".")).listFiles
  def filesMatching(
    matcher: (String) => Boolean) = {
    for(file <- filesHere; if matcher(file.getName))
      yield file
   }
```

```
  def filesEnding(query:String) =
   filesMatching(_.endsWith(query))
```

```  
def filesContaining(query:String)= 
   filesMatching(_.contains(query))
```

```  
def filesRegex(query:String) = 
   filesMatching(_.matches(query))
}
```

函数类型参数 _.endsWith(query)，_.contains(query)和_.matches(query)为函数闭包，因为它们绑定了一个自由变量 query，因此我们可以看到闭包也可以用来简化代码。
