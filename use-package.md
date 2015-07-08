# 使用 Package–将代码放入包中 #
软件开发过程减小程序之间的“耦合性”至关重要，降低耦合性的一个方法是模块化，Scala 提供和 Java 类似的分包机制，但又稍有不同，因此即使你了解 Java 语言，还是建议您阅读本篇和后续几篇介绍 Scala 的 Package 和 Import 的文章。

我们之前的例子，没有明确使用 package，因此它们存在于“未命名”的包中，或是缺省包中。

在Scala将代码定义到某个包中有两种方式：

第一种方法和 Java 一样，在文件的头定义包名，这种方法就后续所有代码都放在该报中。
比如：

```
package bobsrockets.navigation
class Navigator
```

第二种方法有些类似 C#，如：

```
package bobsrockets.navigation {
  class Navigator 
}
```

第二种方法，可以在一个文件中定义多个包。
