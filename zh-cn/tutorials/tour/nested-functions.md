---
layout: tutorial
title: 嵌套函数

disqus: true

tutorial: scala-tour
num: 13
---

Scala中可以嵌套函数定义。下面的对象提供了一个`filter`函数来从一个整数的列表中抽取低于一个临界值的值：

    object FilterTest extends App {
      def filter(xs: List[Int], threshold: Int) = {
        def process(ys: List[Int]): List[Int] =
          if (ys.isEmpty) ys
          else if (ys.head < threshold) ys.head :: process(ys.tail)
          else process(ys.tail)
        process(xs)
      }
      println(filter(List(1, 9, 2, 8, 3, 7, 4), 5))
    }

_注意：嵌套函数`process`引用定义在外部范围的变量`threshold`作为`filter`的参数值。_

这个程序的输入是：

    List(1,2,3,4)