# 问题和练习：接口

> 原文：[`docs.oracle.com/javase/tutorial/collections/interfaces/QandE/questions.html`](https://docs.oracle.com/javase/tutorial/collections/interfaces/QandE/questions.html)

## 问题

1.  在本课程的开始，您了解到核心集合接口被组织成两个不同的继承树。特别是一个接口被认为不是真正的`Collection`，因此位于自己的树的顶部。这个接口的名称是什么？

1.  集合框架中的每个接口都使用`<E>`语法声明，告诉您它是泛型的。当您声明一个`Collection`实例时，指定它将包含的对象类型有什么优势？

1.  什么接口代表不允许重复元素的集合？

1.  什么接口形成了集合层次结构的根？

1.  什么接口代表可能包含重复元素的有序集合？

1.  什么接口代表在处理之前保存元素的集合？

1.  什么接口代表将键映射到值的类型？

1.  什么接口代表双端队列？

1.  列出遍历`List`元素的三种不同方法。

1.  真或假：聚合操作是修改基础集合的变异操作。

## 练习

1.  编写一个以随机顺序打印其参数的程序。不要复制参数数组。演示如何使用流和传统的增强 for 语句打印元素。

1.  取`FindDups`示例，并修改它以使用`SortedSet`而不是`Set`。指定一个`Comparator`，以便在排序和识别集合元素时忽略大小写。

1.  编写一个方法，接受一个`List<String>`并对每个元素应用[`String.trim`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#trim--)。

1.  考虑四个核心接口，`Set`、`List`、`Queue`和`Map`。对于以下四个任务中的每一个，指定哪个核心接口最适合，并解释如何使用它来实现任务。

    1.  Whimsical Toys Inc（WTI）需要记录所有员工的姓名。每个月，将从这些记录中随机选择一个员工以获得免费玩具。

    1.  WTI 已决定每个新产品都将以员工的名字命名，但只使用名字的第一个字母，并且每个名字只能使用一次。准备一个独特的名字列表。

    1.  WTI 决定只想使用最受欢迎的名字来命名其玩具。统计每个名字的员工数量。

    1.  WTI 为当地的长曲棍球队购买季票，将由员工共享。为这项受欢迎的运动创建一个等待名单。

检查你的答案。