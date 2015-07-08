# while 循环  #

Scala 的 while 循环和其它语言如 Java 功能一样，它含有一个条件，和一个循环体，只有条件满足，就一直执行循环体的代码。比如下面的计算最大公倍数的一个实现：

```
def gcdLoop (x: Long, y:Long) : Long ={
   var a=x
   var b=y
   while( a!=0) {
      var temp=a
      a=b % a
      b = temp
  }
  b
}
```

Scala 也有 do-while 循环，它和 while 循环类似，只是检查条件是否满足在循环体执行之后检查。例如：

```
var line=""
do {
   line = readLine()
   println("Read: " + line)
} while (line !="")
```

Scala 的 while 和 do-while 称为“循环”而不是表达式，是因为它不产生有用的返回值（或是返回值为 Unit),可以写成（）.”()”的存在使得 Scala 的 Unit 和 Java 的 void 类型有所不同。比如下面的语句在 Scala 中解释器中执行：

```
scala> def greet() { println("hi")}
greet: ()Unit
scala> greet() == ()
<console>:9: warning: comparing values of types Unit and Unit using `==' will always yield true
              greet() == ()
                      ^
hi
res0: Boolean = true
```

可以看到（或者看到警告） greet()的返回值和()比较结果为 true。

注意另外一种可以返回 Unit 结果的语句为 var 类型赋值语句，如果你使用如下 Java 风格的语句将碰到麻烦：

```
while((line=readLine())!="")
  println("Read: " + line)
```

如果你试图编译或是执行这段代码会有如下警告：

```
/root/scala/demo.scala:2: warning: comparing values of types Unit and String using `!=' will always yield true
while((line=readLine())!="")
```

意思 Unit（赋值语句返回值）和 String 做不等比较永远为 true。什么的代码会是一个死循环。

正因为 While 循环没有值，因此在纯函数化编程中应该避免使用 while 循环，Scala 保留的 While 循环是因为在某些时候使用循环代码比较容易理解，而如果使用纯函数化编程时，需要执行一些重复运行的代码，通常就需要使用回溯函数来实现，回溯函数通常看起来不是很直观。

比如前面计算最大公倍数的函数使用纯函数化编程使用回溯函数实现如下：

```
def gcd (x :Long, y:Long) :Long =
   if (y ==0) x else gcd (y, x % y
```

总的来说，推荐尽量避免在代码使用 while 循环，正如函数化编程要避免使用 var 变量一样。 而使用  while 循环时通常也会使用到 var 变量，因此在你打算使用 while 循环时需要特别小心，看是否可以避免使用它们。
