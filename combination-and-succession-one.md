# 组合和继承–概述 #
在前面我们介绍了 Scala 面向对象的一些基本概念

- [Scala开发教程(4): 类和对象 (一)](http://www.imobilebbs.com/wordpress/archives/date/2013/10/07)
- [Scala开发教程(5): 类和对象 (二)](http://www.imobilebbs.com/wordpress/archives/date/2013/10/08)
- [Scala开发教程(8): 类和对象 (三)](http://www.imobilebbs.com/wordpress/archives/4822)
- [Scala开发教程(9): 类和对象 (四)](http://www.imobilebbs.com/wordpress/archives/4824)
- [Scala开发教程(10): 类和对象 (五)](http://www.imobilebbs.com/wordpress/archives/4826)

从本篇开始继续介绍 Scala 面向对象方法的知识，定义一个新类的方法主要有两种模式：一个通过组合的方式，新创建的类通过引用其它类组合而成，通过这些引用类组合来完成新功能，而是通过继承的方式来扩展基类。为了更好的介绍 Scala 类的组合和继承，以及抽象类，无参数方法，扩展类，方法的重载等，我们打算使用一个现实的例子来说明，因此本篇首先定义需要解答的问题。

我们的需要是定义一个函数库，这个库用来定义在平面上（二维空间）布局元素，每个元素使用一个含有文字的矩形来表示。为方便起见，我们定义一个类构造工厂方法“elem”根据传入的数据来创建一个布局元素。这个方法的接口定义如下：

```
elem(s: String) : Element
```

你可以看到，布局元素使用类型 Element 来构造其模型，你可以调用 above，和 beside 方法来创建一个新的布局元素，这个新的布局元素有两个已经存在的布局元素组合而成，例如：下面的表达式使用多个布局元素构造一个更大区域的布局元素：

```
val column1 = elem(&quot;Hello&quot;) above elem(&quot;***&quot;)
val column2 = elem(&quot;**&quot;) above (&quot;World&quot;)
column beside column2
```

将打印出下面结果：

```
Hello ***
*** world
```

这个例子使用布局元素，是非常好的一个例子可以用来说明一个对象可以使用更简单的对象通过组合的方式来构造。后面的几篇文章将以此为基础，我们将定义一些类，这些类支持使用数组，线段，矩形（简单部件）来构造，并定义组合算子（操作符) above 和 beside。

使用组合算子的概念来设计函数库是一种非常好的方法，它能回报以考虑在应用域构建对象的基础方法。什么是简单对象？用什么方式能让更多有趣的对象通过简单对象构造出来？组合子是怎么挂在一起的？什么是最通用的组合？它们满足任何有趣的规则吗？如果你对这些问题都有好的答案，你的库设计就在正轨上了。
