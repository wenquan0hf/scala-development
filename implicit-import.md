# 隐含的 import #

Scala 缺省为每个文件添加如下几个 package。 这几个包无需明确指明。

```
import java.lang._   //everything in the java.lang package
import scala._       //everything in the scala package
import Predef._      //everything in the Predef object
```

因此在写 Scala 应用之前，先了解下这些缺省包定义了那些类和功能。

此外这三个包的顺序也需要了解一下，比如 StringBuilder 类定义在包 scala 和 java.lang 包中，后定义的 import 会覆盖前面的定义，因此如果不明确指明，
StringBuilder 为 scala.StringBuilde r而非 java.lang.StringBuilder。

注意这里的 scala._ 指所有 scala 下的包，包括子包，也就是所有[http://www.scala-lang.org/files/archive/api/2.10.3/#package](http://www.scala-lang.org/files/archive/api/2.10.3/#package)

Predef 为一对象（非报名），因此可以直接使用 Predef 对象定义的方法（静态引用）。因此在写代码之前了解 Scala 包和 Predef 定义的功能尤其重要。

![](images\18.png)
