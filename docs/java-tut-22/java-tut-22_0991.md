# 搜索

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/search.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/search.html)

目录提供的最有用的功能之一是其*黄页*或*搜索*服务。您可以组成一个由您正在寻找的条目的属性组成的查询，并将该查询提交给目录。然后目录返回满足查询条件的条目列表。例如，您可以要求目录返回所有击球平均分大于 200 的条目，或者所有代表姓氏以"Sch"开头的人的条目。

[`DirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html)接口提供了几种搜索目录的方法，具有逐渐增加的复杂性和功能。搜索目录的各个方面在以下部分中介绍：

+   基本搜索

+   搜索过滤器

+   搜索控件
