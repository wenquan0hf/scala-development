# 组合和继承–定义final成员 #
在定义类的继承关系时，有时你不希望基类的某些成员被子类重载，和 Java 类似，在 Scala 中也是使用 final 来修饰类的成员。比如在前面的 ArrayElement 例子，在 demo 方法前加上 final 修饰符：

```
class ArrayElement extends Element { 
  final override def demo() { 
    println("ArrayElement's implementation invoked") 
  } 
} 
```

如果 LineElement 试图重载 demo，则会报错：

```
scala> class LineElement extends ArrayElement { 
     |   override def demo() { 
     |     println("LineElement's implementation invoked")
     |   }
     | 
     | } 
<console>:10: error: overriding method demo in class ArrayElement of type ()Unit;
 method demo cannot override final member
         override def demo() { 
```

如果你希望某个类不可以派生子类，则可以在类定义前加上 final修饰符：

```
final class ArrayElement extends Element { 
   override def demo() { 
    println("ArrayElement's implementation invoked") 
  } 
} 
```

此时如果还是重载 LineElement 的 demo 函数，则会报错：

```
scala> class LineElement extends ArrayElement { 
     |   override def demo() { 
     |     println("LineElement's implementation invoked")
     |   }
     | 
     | } 
<console>:9: error: illegal inheritance from final class ArrayElement
       class LineElement extends ArrayElement { 
```

