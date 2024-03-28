# 问题和练习：嵌套类

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/QandE/nested-questions.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/QandE/nested-questions.html)

## 问题

1.  程序`Problem.java`无法编译。你需要做什么才能使其编译？为什么？

1.  使用 Java API 文档中`Box`类（位于`javax.swing`包中）的文档来帮助回答以下问题。

    1.  `Box`定义了哪个静态嵌套类？

    1.  `Box`定义了哪个内部类？

    1.  `Box`的内部类的超类是什么？

    1.  从任何类中可以使用`Box`的哪些嵌套类？

    1.  如何创建`Box`的`Filler`类的实例？

## 练习

1.  获取文件`Class1.java`。编译并运行`Class1`。输出是什么？

1.  以下练习涉及修改类`DataStructure.java`，该类在内部类示例部分讨论。

    1.  定义一个名为`print(DataStructureIterator iterator)`的方法。使用`EvenIterator`类的实例调用此方法，使其执行与`printEven`方法相同的功能。

    1.  调用方法`print(DataStructureIterator iterator)`，使其打印具有奇数索引值的元素。使用匿名类作为方法的参数，而不是接口`DataStructureIterator`的实例。

    1.  定义一个名为`print(java.util.function.Function<Integer, Boolean> iterator)`的方法，执行与`print(DataStructureIterator iterator)`相同的功能。使用 lambda 表达式调用此方法，以打印具有偶数索引值的元素。再次使用 lambda 表达式调用此方法，以打印具有奇数索引值的元素。

    1.  定义两个方法，使得以下两个语句打印具有偶数索引值和具有奇数索引值的元素：

        ```java
        DataStructure ds = new DataStructure()
        // ...
        ds.print(DataStructure::isEvenIndex);
        ds.print(DataStructure::isOddIndex);
        ```

检查你的答案。
