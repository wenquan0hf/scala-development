# 引用包中的代码 #
当我们把代码以层次关系放到包中时，它不仅仅可以帮助人们浏览代码，同时也说明了同一包中的代码具有某些相关性。Scala 可以利用这些相关性来简化代码引用，比较使用短名称，而无需使用包的全路径来访问类定义。

下面我们给出三个简单的例子：

```
package bobsrockets{
  package navigation{
    class Navigator{
      var map =new StarMap   
    }
    class StarMap
  }
```

```
  class Ship {
    val nav= new navigation.Navigator
  }
```

```  
  class fleets{
    class Fleet{
      def addShip() {new Ship}
    }
  }
}
```

在第一个例子中，正如你可以预见的一样，访问同一包中定义的类型，无需使用前缀，直接使用类型的名称即可访问，也就是本例可以直接使用 new StarMap。类 StarMap 和 Navigator 定义在同一个包中。

第二个例子，嵌套的 package 也可以在其父包中被同级别的其它类型直接访问，而无需使用全称，因此第二个例子可以使用 navigation 来直接访问 navigation 包，而无需添加 bobsrockets。

第三个例子，但使用包定义的{}语法结构时，内层的类型可以直接访问其外层定义的类型，因此在类 Fleet 中可以直接访问外层定义的类型 Ship。

要注意的是这种用法只适用于你明确嵌套包定义，如果你采用 Java 语言风格-一个文件定义一个包，那么你只能访问该包中定义的类型。

访问包定义的类型还有一个技巧值得说明一下，比如你定义了一些类型之间可能存在相互隐藏的关系，也就是内层定义的同名类型可能会隐藏外层定义的同名类型，那么你怎么来访问外层定义的类型呢？请看下例：

```
package launch{
  class Booster3
}
package bobsrockets{
  package navigtion{
    package launch{
      class Booster1
  }
  class MissionControl{
    val booster1 =new launch.Booster1
    val booster2=new bobsrockets.launch.Booster2
    val booster3=new _root_.launch.Booster3
   }
  }
  package launch{
    class Booster2
  }
}
```

如何来访问 Booster1，Booster2，和 Booster3 呢？访问 Booster1 比较容易，Booster2 可以通过全称来访问。那么如何访问最外层的 Booster3 呢？内层的包 launch 隐藏了这个外部的同名包。为解决这种情况，Scala 提供了\_root\_，也就是所有最外层的类型都可以当成定义在\_root\_包中。因此 \_root\_.launch.Booster3 可以访问最外层定义的类型。 
