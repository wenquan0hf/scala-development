# Scala 基本数据类型的实现方法 #

Scala 的基本数据类型是如何实现的？实际上，Scala 以与 Java 同样的方式存储整数：把它当作 32 位的字类型。这对于有效使用 JVM 平台和与 Java 库的互操作性方面来说都很重要。标准的操作如加法或乘法都被实现为数据类型基本运算操作。然而，当整数需要被当作（Java）对象看待的时候，Scala 使用了“备份”类 java.lang.Integer。如在整数上调用 toString 方法或者把整数赋值给 Any 类型的变量时，就会这么做，需要的时候，Int 类型的整数能自动转换为 java.lang.Integer 类型的“装箱整数(boxed integer)”。

这些听上去和 Java 的 box 操作很像，实际上它们也很像，但这里有一个重要的差异，Scala 使用 box 操作比在 Java 中要少的多：

```
// Java代码 
boolean isEqual(int x,int y) { 
  return x == y; 
} 
System.out.println(isEqual(421,421));
```

你当然会得到 true。现在，把 isEqual 的参数类型变为j ava.lang.Integer（或 Object，结果都一样）：

```
// Java代码 
boolean isEqual(Integer x, Integery) { 
  return x == y; 
} 
System.out.println(isEqual(421,421));
```

你会发现你得到了 false！原因是数 421 使用”box”操作了两次，由此参数 x 和 y 是两个不同的对象，因为在引用类型上==表示引用相等，而 Intege r是引用类型，所以结果是 false。这是展示了 Java 不是纯面向对象语言的一个方面。我们能清楚观察到基本数据值类型和引用类型之间的差别。

现在在 Scala 里尝试同样的实验：

```
scala> def isEqual(x:Int,y:Int) = x == y
isEqual: (x: Int, y: Int)Boolean
```

```
scala> isEqual(421,421)
res0: Boolean = true
```

```
scala> def isEqual(x:Any,y:Any) = x == y
isEqual: (x: Any, y: Any)Boolean
```

```
scala> isEqual(421,421)
res1: Boolean = true
```

Scala 的 == 设计出自动适应变量类型的操作，对值类型来说，就是自然的（数学或布尔）相等。对于引用类型，==被视为继承自 Objec t的 equals 方法的别名。比如对于字符串比较：

```
scala> val x = "abcd".substring(2)
x: String = cd
```

```
scala> val y = "abcd".substring(2)
y: String = cd
```

```
scala> x==y
res0: Boolean = true
```

而在 Java 里，x 与 y 的比较结果将是 false。程序员在这种情况应该用 equals，不过它容易被忘记。

然而，有些情况你需要使用引用相等代替用户定义的相等。例如，某些时候效率是首要因素，你想要把某些类哈希合并： hash cons 然后通过引用相等比较它们的实例，为这种情况，类 AnyRef 定义了附加的 eq  方法，它不能被重载并且实现为引用相等（也就是说，它表现得就像 Java 里对于引用类型的==那样）。同样也有一个 eq 的反义词，被称为 ne。例如：

```
scala> val x =new String("abc")
x: String = abc
```

```
scala> val y = new String("abc")
y: String = abc
```

```
scala> x == y
res0: Boolean = true
```

```
scala> x eq y
res1: Boolean = false
```

```
scala> x ne y
res2: Boolean = true
```

