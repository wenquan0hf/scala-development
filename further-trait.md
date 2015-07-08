# Trait用来实现可叠加的修改操作 #
我们已经看到 Trait 的一个主要用法，将一个瘦接口变成胖接口，本篇我们介绍 Trait 的另外一个重要用法，为类添加一些可以叠加的修改操作。Trait 能够修改类的方法，并且能够通过叠加这些操作（不同组合）修改类的方法。

我们来看这样一个例子，修改一个整数队列，这个队列有两个方法：put 为队列添加一个元素，get 从队列读取一个元素。队列是先进先出，因此 get 读取的顺序和 put 的顺序是一致的。

对于上面的队列，我们定义如下三个 Trait 类型：


- Doubling : 队列中所有元素X2。
- Incrementing： 队列中所有元素递增。
- Filtering： 过滤到队列中所有负数。

这三个 Trait 代表了修改操作，因为它们可以用来修改队列类对象，而不是为队列类定义所有可能的操作。这三个操作是可以叠加的，也就是说，你可以通过这三个基本操作的任意不同组合和原始的队列类“混合”，从而可以得到你所需要的新的队列类的修改操作。

为了实现这个整数队列,我们可以定义这个整数队列的一个基本实现如下：

```
import scala.collection.mutable.ArrayBuffer
abstract class IntQueue {
  def get():Int
  def put(x:Int)
}
class BasicIntQueue extends IntQueue{
  private val buf =new ArrayBuffer[Int]
  def get()= buf.remove(0)
  def put(x:Int) { buf += x }
}
```

下面我们可以使用这个实现，来完成队列的一些基本操作：

```
scala> val queue = new BasicIntQueue
queue: BasicIntQueue = BasicIntQueue@60d134d3
```

```
scala> queue.put (10)
```

```
scala> queue.put(20)
```

```
scala> queue.get()
res2: Int = 10
```

```
scala> queue.get()
res3: Int = 20
```

这个实现完成了对象的基本操作，看起来了还可以，但如果此时有新的需求，希望在添加元素时，添加元素的双倍，并且过滤掉负数，你可以直接修改 put 方法 来完成，但之后需求又变了，添加元素时，添加的为参数的递增值，你也可以修改 put 方法，这样显得队列的实现不够灵活。

我们来看看如果使用 Trait 会有什么结果，我们实现 Doubling，Incrementing，Filtering 如下：

```
trait Doubling extends IntQueue{
  abstract override def put(x:Int) { super.put(2*x)}
}
```

```
trait Incrementing extends IntQueue{
  abstract override def put(x:Int) { super.put(x+1)}
}
```

```
trait Filtering extends IntQueue{
  abstract override def put (x:Int){
    if(x>=0) super.put(x)
  }
}
```

我们可以看到所有的 Trait 实现都已 IntQueue 为基类，这保证这些 Trait 只能和同样继承了 IntQueue 的类“混合”，比如和 BasicIntQueue 混合，而不可以和比如前面定义的 Rational 类混合。

此外 Trait 的 put 方法中使用了 super，通常情况下对于普通的类这种调用是不合法的，但对于 trait来说，这种方法是可行的，这是因为 trait 中的 super 调用是动态绑定的，只要和这个 Trait 混合在其他类或 Trait 之后，而这个其它类或 Trait 定义了 super 调用的方法即可。这种方法是实现可以叠加的修改操作是必须的，并且注意使用 abstract override 修饰符，这种使用方法仅限于 Trait 而不能用作 Class 的定义上。

有了这三个 Trait 的定义，我们可以非常灵活的组合这些 Trait 来修改 BasicIntQueue 的操作。

首先我们使用 Doubling Trait 

```
scala> val queue = new BasicIntQueue with Doubling
queue: BasicIntQueue with Doubling = $anon$1@3b004676
```

```
scala> queue.put(10)
```

```
scala> queue.get()
res1: Int = 20
```

这里通过 BasicIntQueue 和 Doubling 混合，我们构成了一个新的队列类型，每次添加的都是参数的倍增。

我们在使用 BasicIntQueue 同时和 Doubling 和 Increment 混合，注意我们构造两个不同的整数队列，不同时 Doubling 和 Increment 的混合的顺序

```
scala> val queue1 = new BasicIntQueue with Doubling with Incrementing
queue1: BasicIntQueue with Doubling with Incrementing = $anon$1@35849932
```

```
scala> val queue2 = new BasicIntQueue with Incrementing  with Doubling
queue2: BasicIntQueue with Incrementing with Doubling = $anon$1@4a4cdea2
```

```
scala> queue1.put(10)
```

```
scala> queue1.get()
res4: Int = 22
```

```
scala> queue2.put(10)
```

```
scala> queue2.get()
res6: Int = 21
```

可以看到结果和 Trait 混合的顺序有关，简单的说，越后混合的 Trait 作用越大。因此 queue1 先+1，然后 X2，而 queue 先 X2 后+1。

最后我们看看三个 Trait 混合的一个例子：

```
scala> val queue = new BasicIntQueue with Doubling with Incrementing with Filtering
queue: BasicIntQueue with Doubling with Incrementing with Filtering = $anon$1@73a4eb2d
```

```
scala> queue.put(10)
```

```
scala> queue.put(-4)
```

```
scala> queue.put(20)
```

```
scala> queue.get()
res10: Int = 22
```

```
scala> queue.get()
res11: Int = 42
```

```
scala> queue.get()
java.lang.IndexOutOfBoundsException: 0
        at scala.collection.mutable.ResizableArray$class.apply(ResizableArray.scala:44)
        at scala.collection.mutable.ArrayBuffer.apply(ArrayBuffer.scala:44)
        at scala.collection.mutable.ArrayBuffer.remove(ArrayBuffer.scala:163)
        at BasicIntQueue.get(<console>:11)
        at .<init>(<console>:15)
        at .<clinit>(<console>)
        at .<init>(<console>:11)
        at .<clinit>(<console>)
        at $print(<console>)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at scala.tools.nsc.interpreter.IMain$ReadEvalPrint.call(IMain.scala:704)
        at scala.tools.nsc.interpreter.IMain$Request$$anonfun$14.apply(IMain.scala:920)
        at scala.tools.nsc.interpreter.Line$$anonfun$1.apply$mcV$sp(Line.scala:43)
        at scala.tools.nsc.io.package$$anon$2.run(package.scala:25)
        at java.lang.Thread.run(Thread.java:744)
```

最后的异常时因为队列为空（过滤掉了负数），我们没有添加错误处理，元素 -4 没有被添加到了队列中。

由此可以看出，通过 Trait 可以提高类的实现的灵活性，你可以通过这些 Trait 的不同组合定义了多种不同的对列类型。
