# 函数–尾递归 #

在前面的文章中我们提到过可以使用递归函数来消除需要使用 var 变量的 while 循环。下面为一个使用逼近方法求解的一个递归函数表达：

```
def approximate(guess: Double) : Double =
  if (isGoodEnough(guess)) guess
  else approximate(improve(guess))
```

通过实现合适的 isGoodEnough 和 improve 函数，说明这段代码在搜索问题中经常使用。 如果你打算 approximate 运行的快些，你很可能使用下面循环来实现什么的算法：

```
def approximateLoop(initialGuess: Double) : Double = {
  var guess = initialGuess
  while(!isGoodEnough(guess))
    guess=improve(guess)
  guess
 } 
```

那么这两种实现哪一种更可取呢？ 从简洁度和避免使用 var 变量上看，使用函数化编程递归比较好。但是有 whil e循环是否运行效率更高些？实际上，如果我们通过测试，两种方法所需时间几乎相同，这听起来有些不可思议，因为回调函数看起来比使用循环要耗时得多。

其实，对于 approximate 的递归实现，Scala 编译器会做些优化，我们可以看到 approximate 的实现，最后一行还是调用 approximate 本身，我们把这种递归叫做尾递归。Scala 编译器可以检测到尾递归而使用循环来代替。因此，你应该习惯使用递归函数来解决问题，如果是尾递归，那么在效率时不会有什么损失。

## 跟踪尾递归函数 ##

一个尾递归函数在每次调用不会构造一个新的调用栈(stack frame)。所有递归都在同一个执行栈中运行，如果你在调试时，如果在尾递归中调试错误，不会看到嵌套的调用栈，比如下面的代码，非尾递归的一个实现：

```
scala> def boom(x:Int):Int=
     |   if(x==0) throw new Exception("boom!") else boom(x-1) + 1
boom: (x: Int)Int
```

```
scala> boom(5)
java.lang.Exception: boom!
  at .boom(<console>:8)
  at .boom(<console>:8)
  at .boom(<console>:8)
  at .boom(<console>:8)
  at .boom(<console>:8)
  at .boom(<console>:8)
  ... 32 elided
```

boom 不是一个尾递归，因为最后一行为一个递归加一操作，可以看到调用 boom(5)的调用栈，为多层。

我们修改这段代码，使它构成一个尾递归：

```
scala> def bang(x:Int):Int=
     |        if(x==0) throw new Exception("boom!") else bang(x-1)
bang: (x: Int)Int
```

```
scala> bang(5)
java.lang.Exception: boom!
  at .bang(<console>:8)
  ... 32 elided
```


这一次，只看到一层调用栈，Scala 编译器将尾递归优化从循环实现，如果你不想使用这个特性，可以添加 scalac 编译参数 -g:notailcalls 来取消这个优化，此后，如果抛出异常，尾递归也会显示多层调用栈。

## 尾递归的一些局限性 ##

目前来说，Scala 编译器只能对那些直接实现尾递归的函数，比如前面的 approximate 和 bang，如果一个函数间接实现尾递归，比如下面代码：

```
def isEven(x:Int): Boolean =
  if(x==0) true else isOdd(x-1)
def isOdd(x:Int): Boolean=
  if(x==0) false else isEven(x-1)
```

isEven 和 isOdd 事件也是为递归，但不是直接定义的尾递归，scala 编译器无法对这种递归进行优化。
