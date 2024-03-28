# 问题和练习：类

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/QandE/creating-questions.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/QandE/creating-questions.html)

## 问题

1.  考虑以下类：

    ```java
    public class IdentifyMyParts {
        public static int x = 7; 
        public int y = 3; 
    }

    ```

    1.  类变量是什么？

    1.  实例变量是什么？

    1.  以下代码的输出是什么：

        ```java
        IdentifyMyParts a = new IdentifyMyParts();
        IdentifyMyParts b = new IdentifyMyParts();
        a.y = 5;
        b.y = 6;
        a.x = 1;
        b.x = 2;
        System.out.println("a.y = " + a.y);
        System.out.println("b.y = " + b.y);
        System.out.println("a.x = " + a.x);
        System.out.println("b.x = " + b.x);
        System.out.println("IdentifyMyParts.x = " + IdentifyMyParts.x);

        ```

## 练习

1.  编写一个类，其实例代表一副扑克牌中的一张牌。扑克牌有两个独特的属性：等级和花色。确保保留你的解决方案，因为你将被要求在枚举类型中重新编写它。

    * * *

    **提示：**

    你可以使用`assert`语句来检查你的赋值。你可以写：

    ```java
    assert (boolean expression to test); 

    ```

    如果布尔表达式为假，你将收到一个错误消息。例如，

    ```java
    assert toString(ACE) == "Ace";

    ```

    应该返回`true`，这样就不会有错误消息。

    如果你使用`assert`语句，你必须用`ea`标志运行你的程序：

    ```java
    java -ea YourProgram.class

    ```

    * * *

1.  编写一个类，其实例代表一副**完整**的扑克牌。你也应该保留这个解决方案。

1.  3\. 编写一个小程序来测试你的牌组和卡片类。这个程序可以简单到创建一副牌并显示其卡片。

检查你的答案。
