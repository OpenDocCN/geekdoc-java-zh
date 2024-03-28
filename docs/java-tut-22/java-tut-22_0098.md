# 问题和练习：继承

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/QandE/inherit-questions.html`](https://docs.oracle.com/javase/tutorial/java/IandI/QandE/inherit-questions.html)

## 问题

1\. 考虑以下两个类：

```java
public class ClassA {
    public void methodOne(int i) {
    }
    public void methodTwo(int i) {
    }
    public static void methodThree(int i) {
    }
    public static void methodFour(int i) {
    }
}

public class ClassB extends ClassA {
    public static void methodOne(int i) {
    }
    public void methodTwo(int i) {
    }
    public void methodThree(int i) {
    }
    public static void methodFour(int i) {
    }
}

```

a. 哪个方法覆盖了超类中的方法？

b. 哪个方法隐藏了超类中的方法？

c. 其他方法做什么？

2\. 考虑你在问题和练习：类中编写的`Card`、`Deck`和`DisplayDeck`类。每个类应该覆盖哪些`Object`方法？

## 练习

1\. 编写你在问题 2 中回答的方法的实现。

检查你的答案。
