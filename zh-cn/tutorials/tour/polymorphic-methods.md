---
layout: tutorial
title: 多态方法

disqus: true

tutorial: scala-tour
num: 21
---

Scala中方法可以通过值和类型参数化。比如在类的级别，值参数被围绕在一对圆括弧中，而类型参数声明在一对方括号中。

这里有个例子：

    object PolyTest extends App {
      def dup[T](x: T, n: Int): List[T] =
        if (n == 0)
          Nil
        else
          x :: dup(x, n - 1)

      println(dup[Int](3, 4))
      println(dup("three", 3))
    }

对象``PolyTest`的方法`dup`通过类型`T`参数化，并带有值参数`x:T`和`n:Int`。当方法`dup`被调用时，程序员提供需要的参数 _（看上述程序的第8行）_ ，但是如上述程序第9行所示，成需不需要显式给出实际类型参数。Scala的类型系统能推理出其类型。这是通过看给定的值参数以及方法被调用的上下文来做到的。
Method `dup` in object `PolyTest` is parameterized with type `T` and with the value parameters `x: T` and `n: Int`. When method `dup` is called, the programmer provides the required parameters _(see line 8 in the program above)_, but as line 9 in the program above shows, the programmer is not required to give actual type parameters explicitly. The type system of Scala can infer such types. This is done by looking at the types of the given value parameters and at the context where the method is called.

请注意特性`App`是设计用来写简短的测试程序，但是应该避免生产环境代码（对于Scala版本2.8.x以及更早），因为它可能影响JVM优化生成代码的能力；请使用`def main()`。
