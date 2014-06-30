---
layout: tutorial
title: XML 处理

disqus: true

tutorial: scala-tour
num: 33
---

Scala能够很容易地创建，解析和处理XML文档。XML数据在Scala中能以一种通用的数据形式，或者带有特定数据的数据形式来表示。后者由*数据绑定* 工具`schema2src`支持。

### 运行时表示 ###

XML 数据被表示为标记树。Scala 1.2（之前版本需要使用 -Xmarkupoption）开始，你可以使用标准XML语法方便地创建这样地标记节点。

看看以下XML文档：

    <html>
      <head>
        <title>Hello XHTML world</title>
      </head>
      <body>
        <h1>Hello world</h1>
        <p><a href="http://scala-lang.org/">Scala</a> talks XHTML</p>
      </body>
    </html>

这份文件可通过下面地Scala程序创建:

    object XMLTest1 extends App {
      val page = 
      <html>
        <head>
          <title>Hello XHTML world</title>
        </head>
        <body>
          <h1>Hello world</h1>
          <p><a href="scala-lang.org">Scala</a> talks XHTML</p>
        </body>
      </html>;
      println(page.toString())
    }

可以混用Scala语法和XML:

    object XMLTest2 extends App {
      import scala.xml._
      val df = java.text.DateFormat.getDateInstance()
      val dateString = df.format(new java.util.Date())
      def theDate(name: String) = 
        <dateMsg addressedTo={ name }>
          Hello, { name }! Today is { dateString }
        </dateMsg>;
      println(theDate("John Doe").toString())
    }

### 数据绑定 ###
许多时候下，你需要拥有你所处理地XML文档地DTD。你可能会想创建特殊地Scala类，和一些代码来解析XML，并保存。Scala自带一个非常棒地工具将会你的DTD转化为一系列Scala类定义，所有这些都是为了你。注意schema2src工具的文档和例子是无法在Burak书[初级Scala XML书]((http://burak.emir.googlepages.com/scalaxbook.docbk.html)中找到。

