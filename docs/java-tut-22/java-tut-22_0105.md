# 问题和练习：数字

> 原文：[`docs.oracle.com/javase/tutorial/java/data/QandE/numbers-questions.html`](https://docs.oracle.com/javase/tutorial/java/data/QandE/numbers-questions.html)

## 问题

1.  使用 API 文档找到以下问题的答案：

    1.  你可以使用哪个`Integer`方法将`int`转换为以十六进制表示数字的字符串？例如，哪个方法将整数 65 转换为字符串“41”？

    1.  你会使用哪个`Integer`方法将以基数 5 表示的字符串转换为相应的`int`？例如，如何将字符串“230”转换为整数值 65？展示你将用于完成此任务的代码。

    1.  你可以使用哪个 Double 方法来检测浮点数是否具有特殊值“非数字”（`NaN`）？

1.  以下表达式的值是多少，为什么？

    ```java
    Integer.valueOf(1).equals(Long.valueOf(1))

    ```

## 练习

1.  将`MaxVariablesDemo`更改为显示最小值而不是最大值。您可以删除与变量`aChar`和`aBoolean`相关的所有代码。输出是什么？

1.  创建一个程序，从命令行读取未指定数量的整数参数并将它们相加。例如，假设您输入以下内容：

    ```java
    java Adder 1 3 2 10

    ```

    程序应显示`16`，然后退出。如果用户只输入一个参数，程序应显示错误消息。您可以基于`ValueOfDemo`编写您的程序。

1.  创建一个类似于上一个程序但具有以下区别的程序：

    +   它不是读取整数参数，而是读取浮点参数。

    +   它显示参数的总和，小数点右侧正好有两位。

    例如，假设您输入以下内容：

    ```java
    java FPAdder 1 1e2 3.0 4.754

    ```

    程序将显示`108.75`。根据您的区域设置，小数点可能是逗号（`,`）而不是句号（`.`）。

检查你的答案。
