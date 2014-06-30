---
layout: tutorial
title: 视图

disqus: true

tutorial: scala-tour
num: 32
---

[隐式参数](implicit-parameters.html)和方法也可以定义称为 _视图_ 的隐式转换。从类型`S`到类型`T`的视图是由具有函数类型`S => T`的隐式值或者可转换为该类型某个值的隐式方法来定义。

视图适用于两种情况：

* 如果表达式`e`是类型`S`，而`S`并不符合表达式的期望类型`T`。
* 在选择`T`类型的`e`的`e.m`中，如果选择`m`不表示`T`的成员。

在第一种情况下，视图`v`中搜索适用于`e`，而结果类型符合`T`。
在第二种情况下，视图`v`中搜索适用于`e`，而结果包含名为`m`的成员。

下面的基于两个`List[Int]`类型的列表xs和ys的操作是合法的：

	xs <= ys

假设下面定义的隐式方法`list2ordered`和`int2ordered`在范围内：

    implicit def list2ordered[A](x: List[A])
        (implicit elem2ordered: a => Ordered[A]): Ordered[List[A]] =
      new Ordered[List[A]] { /* .. */ }
    
    implicit def int2ordered(x: Int): Ordered[Int] = 
      new Ordered[Int] { /* .. */ }

该`list2ordered`函数也可以表示对于类型参数的 _视图界限_ 的使用：

    implicit def list2ordered[A <% Ordered[A]](x: List[A]): Ordered[List[A]] = ...
    
Scala编译器然后生成代码等价于上面给出的`list2ordered`的定义。

隐式导入的对象`scala.Predef` 声明了多个预定义类型（例如`Pair`）和方法（例如`assert`），但也有几个视图。下面的例子预定义视图`charWrapper`的思想：

    final class RichChar(c: Char) {
      def isDigit: Boolean = Character.isDigit(c)
      // isLetter, isWhitespace, etc.
    }
    object RichCharTest {
      implicit def charWrapper(c: char) = new RichChar(c)
      def main(args: Array[String]) {
        println('0'.isDigit)
      }
    }

