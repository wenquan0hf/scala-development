# Match表达式 #
Scala 的 Match 表达式支持从多个选择中选取其一，类似其它语言中的 switch 语句。通常来说，Scala 的 match 表达式支持任意的匹配模式，这种基本模式将在后面介绍，本篇介绍类似 switch 用法的 match 表达式，也是在多个选项中选择其一。

例如下面的例子从参数中读取食品的名称，然后根据食品的名称，打印出该和该食品搭配的食品，比如输入 ”salt”，与之对应的食品为”pepper”。如果是”chips”，那么搭配的就是“salsa”等等。

```
val firstArg = if (args.length >0 ) args(0) else ""
firstArg match {
  case "salt" => println("pepper")
  case "chips" => println("salsa")
  case "eggs" => println("bacon")
  case _ => println("huh?")
}
```

这段代码和 Java 的 switch 相比有几点不同：  
 一是任何类型的常量都可以用在 case 语句中，而不仅仅是 int 或是枚举类型。  
 二是每个 case 语句无需使用 break，Scala不支持“fall through”。  
 三是 Scala 的缺省匹配为”_”,其作用类似 java 中的 default。  

而最关键的一点是 scala 的 match 表达式有返回值，上面的代码使用的是 println 打印，而实际上你可以使用表达式，比如修改上面的代码如下：

```
val firstArg = if (args.length >0 ) args(0) else ""
val friend = firstArg match {
  case "salt" => "pepper" 
  case "chips" => "salsa" 
  case "eggs" => "bacon" 
  case _ => "huh?" 
}
```             


这段代码和前面的代码是等效的，不同的是后面这段代码 match 表达式返回结果。
