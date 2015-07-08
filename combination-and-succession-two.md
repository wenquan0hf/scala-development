# 组合和继承–抽象类 #
上一篇我们定义了我们需要解决的问题，我们首要的任务是定义 Element 类型，这个类型用来表示一个布局元素。由于每个元素为一个具有二维矩形形状的字符串，因此我们理所当然的可以定义个成员变量 contents，用来表示这个二维布局元素的内容。这个元素我们使用一个字符串的数组来表示，这个数组的每个字符串元素代表布局的一行。也就是 contents 的类型为 Array[String]。

```
abstract class Element {
  def contents: Array[String]
}
```

在这个类中，成员 contents 使用了没有定义具体实现的方法来定义，这个方法称为一“抽象方法”，一个含有抽象方法的类必须定义成抽象类，也就是使用abstract关键字来定义类。

abstract 修饰符表示所定义的类可能含有一些没有定义具体实现的抽象成员，因此你不能构建抽象类的实例，如果你试图这么做，编译器将报错：

```
scala> new Element
<console>:9: error: class Element is abstract; cannot be instantiated
              new Element
              ^
```

后面的文章将继续介绍如何创建这个抽象类的子类，你可以构造这些子类的具体实例,这是因为这些子类实现了抽象成员。

要注意的是 contents 方法本身没有使用 abstract 修饰符，一个没有定义实现的方法就是抽象方法，和 Java 不同的是，抽象方法不需要使用 abstract 修饰符来表示，只要这个方法没有具体实现，就是抽象方法，相反，如果该方法有具体实现，称为“具体(concrete)”方法。

另一个术语用法需要分辨声明： declaration 和定义： definition。类 Element 声明了抽象方法  contents，但当前没有定义具体方法。然而下一节，我们要定义一些具体方法来加强 Element。
