# 问题和练习：实现方式

> 原文：[`docs.oracle.com/javase/tutorial/collections/implementations/QandE/questions.html`](https://docs.oracle.com/javase/tutorial/collections/implementations/QandE/questions.html)

## 问题

1.  你计划编写一个程序，使用几个基本的集合接口：`Set`、`List`、`Queue` 和 `Map`。你不确定哪种实现方式最适合，所以决定在了解程序在实际环境中如何运行之前，先使用通用的实现方式。这些实现方式是哪些？

1.  如果你需要一个提供按值排序迭代的`Set`实现，应该使用哪个类？

1.  你使用哪个类来访问包装器实现？

## 练习

1.  编写一个程序，将由第一个命令行参数指定的文本文件读入一个`List`中。然后，程序应该打印文件中的随机行，打印的行数由第二个命令行参数指定。编写程序时，应一次性分配正确大小的集合，而不是在读取文件时逐渐扩展。提示：要确定文件中的行数，可以使用[`java.io.File.length`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#length--)来获取文件的大小，然后除以平均行的假定大小。

检查你的答案。
