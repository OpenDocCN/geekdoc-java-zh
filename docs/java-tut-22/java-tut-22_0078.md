# 问题和练习：注解

> 原文：[`docs.oracle.com/javase/tutorial/java/annotations/QandE/questions.html`](https://docs.oracle.com/javase/tutorial/java/annotations/QandE/questions.html)

## 问题

1.  以下接口有什么问题？

    ```java
    public interface House {
        @Deprecated
        void open();
        void openFrontDoor();
        void openBackDoor();
    }

    ```

1.  考虑`House`接口的以下实现，如问题 1 所示。

    ```java
    public class MyHouse implements House {
        public void open() {}
        public void openFrontDoor() {}
        public void openBackDoor() {}
    }

    ```

    如果您编译此程序，编译器会产生警告，因为`open`已被弃用（在接口中）。您可以采取什么措施消除该警告？

1.  以下代码是否会编译出错？为什么？

    ```java
    public @interface Meal { ... }

    @Meal("breakfast", mainDish="cereal")
    @Meal("lunch", mainDish="pizza")
    @Meal("dinner", mainDish="salad")
    public void evaluateDiet() { ... }

    ```

## 练习

1.  为增强请求定义一个注解类型，具有`id`、`synopsis`、`engineer`和`date`元素。为工程师指定默认值为`unassigned`，为日期指定默认值为`unknown`。

检查您的答案。
