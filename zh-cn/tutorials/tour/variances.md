---
layout: tutorial
title: 差异

disqus: true

tutorial: scala-tour
num: 31
---

Scala支持[范型类](generic-classes.html)类型参数的差异注释。非Java 5(也称作 [JDK 1.5](http://java.sun.com/j2se/1.5/))中，差异注释在定义类抽象时加入，然而在Java5中，variance annotations却是在使用类抽象时指定。

在关于[范型类](generic-classes.html)的页面中，给出了一个可变栈的例子。我们解释过这个类`Stack[T]`定义的类型从属于关于这个类型参数的不可变子类型。这样可以限制类抽象的重用。我们现在得到了对于没有限制的栈的一个功能性（也就是：不可变）的实现。请注意这里有一个高级的例子，以一种不寻常的方式结合使用[多态方法](polymorphic-methods.html)，[类型下限](lower-type-bounds.html)以及协变类型注释。此外，我们使用[内部类](inner-classes.html)来串联栈元素而无需现实链接。

    class Stack[+A] {
      def push[B >: A](elem: B): Stack[B] = new Stack[B] {
        override def top: B = elem
        override def pop: Stack[B] = Stack.this
        override def toString() = elem.toString() + " " +
                                  Stack.this.toString()
      }
      def top: A = sys.error("no element on stack")
      def pop: Stack[A] = sys.error("no element on stack")
      override def toString() = ""
    }
    
    object VariancesTest extends App {
      var s: Stack[Any] = new Stack().push("hello");
      s = s.push(new Object())
      s = s.push(7)
      println(s)
    }

注释`+T`声明了类型`T`仅可用在协变位置。同样，`-T`则声明了`T`仅可用在反变位置。对于协变类型参数，我们得到了关于这个类型参数的协变子类型关系。对于我们的例子意味着当`T`是`S`的子类型时`Stack[T]`也是`Stack[S]`的子类型。相反地则是标记`-`的类型参数。

对于这个栈的例子我们必须在逆变位置上使用协变类型参数`T`从而能够定义`push`方法。由于我们想要栈的协变子类型，因此我们使用了一个敲门以及`push`方法的参数类型的抽象。我们得到了一个利用元素类型`T`作为`push`类型参数下界的多态方法。这样的作用是使得`T`的变化与其作为协变类型参数的声明保持同步。现在栈便是协变的，但是我们的解决方案允许比如可以在整数栈中压入字符串。结果是`Stack[Any]`类型的栈；因此只要结果用于我们期望一个整数栈的上下文时，我们实际上可以发现这个错误。否则我们仅仅得到了一个更泛化元素类型的栈。
