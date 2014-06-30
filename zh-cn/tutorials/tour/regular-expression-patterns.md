---
layout: tutorial
title: 正则表达式模式

disqus: true

tutorial: scala-tour
num: 22
---

## 右侧忽略序列模式 ##

右侧忽略模式是一个很有用的特性，可以用来分解任何`Seq[A]`的子类型或者重复声明形式参数的实例类的数据，例如

    Elem(prefix:String, label:String, attrs:MetaData, scp:NamespaceBinding, children:Node*)

在其他例子中，Scala允许模式在最右边的位置上使用通配符星号`_*`用于表示任意长度的序列。
下面的例子说明了一种模式匹配，它匹配一个序列的前缀，并将余下部分绑定到变量`rest`。

	object RegExpTest1 extends App {
		def containsScala(x: String):Boolean = {
			val z: Seq[Char] = x
			z match {
				case Seq('s','c','a','l','a',rest @ _*) =>
					println("rest is "+rest)
					true
				case Seq(_*) => 
					false
			}
		}
	}

不同于之前Scala的版本，将不再允许使用任意正则表达式，原因如下所述。

### 一般的`RegExp`模式暂时从Scala中剔除 ###

由于我们发现了一个准确性的问题，这一特性暂时从Scala语言中剔除了。若用户社区有需要，我们可能以一种改良后的形式重新启用它。

据我们所知，正则表达式匹配对于XML处理并没有我们所预期的那样有用。在真实的XML处理应用中，XPath似乎是一个好得多的选择。当我们发现我们的翻译或者正则表达式模式对于有一些难懂的模式依然存在一些bug，且依然非常难以排除，所以我们决定是时候精简这门语言了。
