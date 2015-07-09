# 组合和继承–使用 override 修饰符 #

在前面的例子 LineElement 使用了 override 来修饰 width 和 height 成员变量，在 Scala 中需要使用 override 来重载父类的一个非抽象成员，实现抽象成员无需使用 override，如果子类没有重载父类中的成员，不可以使用 override 修饰符。

这个规则可以帮助编译器发现一些难以发现的错误，可以增强系统安全进化。比如，如果你把 height 拼写错误为 hight，使用 override 编译器会报错

````
root@mail:~/scala# scalac demo.scala 
demo.scala:13: error: method hight overrides nothing
  override def hight = 1
               ^
one error found
```

这个规则对于系统的演讲尤为重要，假设你定义了一个 2D 图形库。你把它公开，并广泛使用。库的下一个版本里你想在你的基类 Shape 里增加一个新方法：

```
def hidden(): Boolean
```

你的新方法将被用在许多画图方法中去决定是否需要把形状画出来，这将可以大大提高系统绘图的性能，但你不可以冒着破坏客户代码的风险做这件事。毕竟客户说不定已经使用不同的  hidde n实现定义了 Shape 的子类。或许客户的方法实际上是让对象消失而不是检测是否对象是隐藏的。因为这两个版本的 hidden 互相重载，你的画图方法将停止对象的消失，这可真不是你想要的！

如果图形库和它的用户是用 Scala 写的，那么客户的 hidden 原始实现就不会有 override 修饰符，因为这时候还没有另外一个使用那个名字的方法。一旦你添加了 hidden 方法到你 Shape 类的第二个版本，客户的重编译将给出像下列这样的错误：

```
.../Shapes.scala:6: error: error overriding method 
		hidden in class Shape of type ()Boolean; 
method hidden needs 'override' modifier 
def hidden(): Boolean =
```

也就是说，代之以错误的执行，你的客户将得到一个编译期错误，这常常是更可取的。
