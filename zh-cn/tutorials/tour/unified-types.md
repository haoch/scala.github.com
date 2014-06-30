---
layout: tutorial
title: 统一类型

disqus: true

tutorial: scala-tour
num: 30
---

对比Java而言，Scala中所有值都是对象（包括数值和函数）。因为Scala是基于类的，所有值都是类的实例。下图说明了类的层次结构。

![Scala Type Hierarchy]({{ site.baseurl }}/resources/images/classhierarchy.img_assist_custom.png)

## Scala Class Hierarchy ##

## scala类的层次结构 ##

所有类的超类`scala.Any`有两个直接子类`scala.AnyVal`和`scala.AnyRef`表示两种不同类的领域：值类和引用类。所有值类都是预先定义的；它们对应着Java类语言的原始类型。所有其他的类定义引用类型。用户定义的类默认定义引用类型；即它们通常（间接地）为`scala.AnyRef`的子类。Scala中每个用户定义的类隐式继承特性`scala.ScalaObject`。来自Scala运行的基础环境（比如Java运行时环境）的类不继承`scala.ScalaObject`。如果Scala 用在Java运行时环境，那么`scala.AnyRef`对应着`java.lang.Object`。

请注意上面的图也显示了称之为视图的值类之间隐式转换。这里的例子证明数值，字符，布尔值和函数就像其他每个对象一样也都是对象：
 
    object UnifiedTypes extends App {
      val set = new scala.collection.mutable.LinkedHashSet[Any]
      set += "This is a string"  // add a string
      set += 732                 // add a number
      set += 'c'                 // add a character
      set += true                // add a boolean value
      set += main _              // add the main function
      val iter: Iterator[Any] = set.iterator
      while (iter.hasNext) {
        println(iter.next.toString())
      }
    }

程序以继承至`App`的顶级单例对象的形式声明了一个应用`UnifiedTypes`。这个应用定义了局部变量`set`，引用一个类`LinkedHashSet[Any]`的实例。程序将不同的元素添加到这个集合中。这些元素必须符合声明的集合元素类型`Any`。最后，所有元素的字符串表示答应出来。

这是程序的输出：

    This is a string
    732
    c
    true
    <function>
