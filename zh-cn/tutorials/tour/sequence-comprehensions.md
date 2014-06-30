---
layout: tutorial
title: 序列推导

disqus: true

tutorial: scala-tour
num: 7
---

Scala 提供一种轻量级符号表示*序列推导*。推导的是形式是`for (enumerators) yield e`，这里的`enumerators`指的是一系列分号分割的枚举成员。*枚举器*可以是一个引入新值的生成器，或者是一个过滤器。推导过程调用枚举器生成的每个绑定的主体`e`并返回这些值的序列。

例如：
 
    object ComprehensionTest1 extends App {
      def even(from: Int, to: Int): List[Int] =
        for (i <- List.range(from, to) if i % 2 == 0) yield i
      Console.println(even(0, 20))
    }

函数中的for表达式引入了新的`Int`类型的变量`i`，之后以此绑定到列表`List(from, from + 1, ..., to - 1)`的所有值。条件`if i % 2 == 0`过滤掉所有奇数，以至于主体（仅包含表达式i）只对偶数执行。所以，整个for表达式返回了一个偶数的列表。

程序产生以下输出：

    List(0, 2, 4, 6, 8, 10, 12, 14, 16, 18)

这里是一个更复杂的例子，计算所有从`0`到`n-1`之间和等于指定值`v`的数值对：

    object ComprehensionTest2 extends App {
      def foo(n: Int, v: Int) =
        for (i <- 0 until n;
             j <- i until n if i + j == v) yield
          Pair(i, j);
      foo(20, 32) foreach {
        case (i, j) =>
          println("(" + i + ", " + j + ")")
      }
    }

这个例子表明推倒并不仅限与列表。上面的程序反而使用了迭代器。任何支持操作`filterWith`，`map`，和`flatMap`（带有适当类型）的数据类型都能用于序列推导。

程序输出如下：

    (13, 19)
    (14, 18)
    (15, 17)
    (16, 16)

还有一种特殊形式的序列推导返回`Unit`。这里由一系列生成器和过滤器生成的绑定值用来执行附带作用。程序员必须省略关键字`yield`来使用这样一个序列推导。

这里的程序等价于前一个，但是使用了序列推导的特例`Unit`：
 
    object ComprehensionTest3 extends App {
      for (i <- Iterator.range(0, 20);
           j <- Iterator.range(i, 20) if i + j == 32)
        println("(" + i + ", " + j + ")")
    }


