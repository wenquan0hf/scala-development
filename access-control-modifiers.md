# 访问控制修饰符 #

包的成员，类或对象可以使用访问控制修饰符，比如 private 和 protected 来修饰，通过这些修饰符可以控制其他部分对这些类，对象的访问。Scala 和访问控制大体上和 Java 类似，但也有些重要的不同，本篇将介绍这些。

## 私有成员 ##

Scala 的私有成员和 Java 类似，一个使用 private 修饰过的类或对象成员，只能在该类或对象中访问，在 Scala 中，也可以在嵌套的类或对象中使用。比如：

```
class Outer{
  class Inner{
    private def f(){
      println("f")
    }
    class InnerMost{
      f() //OK
    }
  }
  (new Inner).f();// error: f is not accessible
}
```

在Scala 中，(new Inner).f()是不合法的，因为它是在 Inner 中定义的私有类型，而在 InnerMos t 中访问 f 却是合法的，这是因为 InnerMost 是包含在 Inner 的定义中（子嵌套类型）。 在 Java 语言中，两种访问都是可以的。Java 允许外部类型访问其包含的嵌套类型的私有成员。

## 保护成员 ##

和私有成员类似，Scala 的访问控制比 Java 来说也是稍显严格些。在 Scala 中，由 Protected 定义的成员只能由定义该成员和其派生类型访问。而在 Java 中，由 Protected 定义的成员可以由同一个包中的其它类型访问。在 Scala 中，可以通过其它方式来实现这种功能。

下面为 protected 的一个例子：

```
class p{
  class Super{
    protected def f() {
      println("f")
    }
  }
  class Sub extends Super{
    f()
  }
  class Other{
    (new Super).f() //error: f is not accessible
  }
}
```

## 公开成员 ##

public 访问控制为 Scala 定义的缺省方式，所有没有使用 private 和 protected 修饰的成员都是“公开的”，可以被自由访问。Scala 不需要使用 public 来指定“公开访问”修饰符。
