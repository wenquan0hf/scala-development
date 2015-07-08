# 操作基本数据类型 #

Scala 提供了丰富的运算符用来操作前面介绍的基本数据类型。前面说过，这些运算符（操作符）实际为普通类方法的简化（或者称为美化）表示。比如 1+2 ，实际为 (1).+(2) ，也就是调用 Int 类型的+方法。例如：

```
scala> val sumMore = (1).+(2)
sumMore: Int = 3
```

实际上类 Int 定义了多个+方法的重载方法（以支持不同的数据类型）比如和 Long 类型相加。
+符号为一运算符，为一中缀运算符。 在 Scala 中你可以定义任何方法为一操作符。 比如 String 的 IndexOf 方法也可以使用操作符的语法来书写。 例如：

```
scala> val s ="Hello, World"
s: String = Hello, World
scala> s indexOf 'o'
res0: Int = 4
```

由此可以看出运算符在 Scala 中并不是什么特殊的语法，任何 Scala 方法都可以作为操作符来使用。是否是操作符取决于你如何使用这个方法，当你使用 s.indexOf(‘o’) indexOf 不是一个运算符。 而你写成 s indexOf ‘o’ ,indexOf 就是一个操作符，因为你使用了操作符的语法。

除了类似+的中缀运算符（操作符在两个操作符之间），还可以有前缀运算符和后缀运算符。顾名思义前缀运算符的操作符在操作数前面，比如 -7 的“-”。后缀运算符的运算符在操作数的后面，比如 7 toLong 中的 toLong。 前缀和后缀操作符都使用一个操作数，而中缀运算符使用前后两个操作数。Scala 在实现前缀和后缀操作符的方法，其方法名都以 unary\_-开头。比如:表达式 -2.0 实际上调用 （2.0）.unary_-方法。

```
scala> -2.0
res1: Double = -2.0
scala> (2.0).unary_-
res2: Double = -2.0
```

如果你需要定义前缀方法，你只能使用+,-,! 和～四个符号作为前缀操作符。

后缀操作符在不使用.和括号调用时不带任何参数。在 Scala 中你可以省略掉没有参数的方法调用的空括号。按照惯例，如果你调用方法是为了利用方法的“副作用”，此时写上空括号，如果方法没有任何副作用（没有修改其它程序状态），你可以省略掉括号。比如：

```
scala> val s ="Hello, World"
s: String = Hello, World
scala> s toLowerCase
res0: String = hello, world
```

具体 Scala 的基本数据类型支持的操作符，可以参考 Scala API 文档。下面以示例介绍一些常用的操作符：

## 算术运算符 + – × / ##

```
scala> 1.2 + 2.3
res0: Double = 3.5
```

```
scala> 'b' -'a'
res1: Int = 1
```

```
scala> 11 % 4
res2: Int = 3
```

```
scala> 11.0f / 4.0f
res3: Float = 2.75
```

```
scala> 2L * 3L
res4: Long = 6
```

## 关系和逻辑运算符包括 >，<，>=，!等 ##

```
scala> 1 >2
res5: Boolean = false
```

```
scala> 1.0 <= 1.0
res6: Boolean = true
```

```
scala> val thisIsBoring = !true
thisIsBoring: Boolean = false
```

```
scala> !thisIsBoring
res7: Boolean = true
```

```
scala> val toBe=true
toBe: Boolean = true
```

```
scala> val question = toBe || ! toBe
question: Boolean = true
```

要注意的是逻辑运算支持“短路运算”，比如 op1 || op2 ，当 op1=true ，op2 无需再计算，就可以知道结果为 true。这时 op2 表示式不会执行。例如：

```
scala> def salt()= { println("salt");false}
salt: ()Boolean
scala> def pepper() ={println("pepper");true}
pepper: ()Boolean
scala> pepper() && salt()
pepper
salt
res0: Boolean = false
scala> salt() && pepper()
salt
res1: Boolean = false
```

## 位操作符 ##

```
scala> 1 & 2
res2: Int = 0
```

```
scala> 1 | 2
res3: Int = 3
```

```
scala> 1 ^ 2
res4: Int = 3
```

```
scala> ~1
res5: Int = -2
```


## 对象恒等比较 ##
 
如果需要比较两个对象是否相等，可以使用==和!=操作符

```
scala> 1 == 2
res6: Boolean = false
```

```
scala> 1 !=2
res7: Boolean = true
```

```
scala> List(1,2,3) == List (1,2,3)
res8: Boolean = true
```

```
scala> ("he"+"llo") == "hello"
res9: Boolean = true
```


Scala 的==和 Java 不同，scala 的==只用于比较两个对象的值是否相同。而对于引用类型的比较使用另外的操作符 eq 和 ne。

## 操作符的优先级和左右结合性 ##

Scala 的操作符的优先级和 Java 基本相同，如果有困惑时，可以使用（）改变操作符的优先级。 操作符一般为左结合，Scala 规定了操作符的结合性由操作符的最后一个字符定义。对于以“：”结尾的操作符都是右结合，其它的操作符多是左结合。例如： 


a*b 为　a.*(b)　而　a:::b 为 b.:::(a),而 a:::b:::c = a::: (b ::: c) , a*b*c= (a*b)*c
