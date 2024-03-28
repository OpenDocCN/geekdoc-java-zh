# 问题和练习：对象

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/QandE/objects-questions.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/QandE/objects-questions.html)

## 问题

1.  以下程序有什么问题？

    ```java
    public class SomethingIsWrong {
        public static void main(String[] args) {
            Rectangle myRect;
            myRect.width = 40;
            myRect.height = 50;
            System.out.println("myRect's area is " + myRect.area());
        }
    }

    ```

1.  以下代码创建了一个数组和一个字符串对象。在代码执行后，这些对象有多少个引用？这两个对象是否有资格进行垃圾回收？

    ```java
    ...
    String[] students = new String[10];
    String studentName = "Peter Parker";
    students[0] = studentName;
    studentName = null;
    ...

    ```

1.  程序如何销毁它创建的对象？

## 练习

1.  修复问题 1 中显示的名为`SomethingIsWrong`的程序。

1.  给定以下名为`NumberHolder`的类，编写一些代码来创建该类的一个实例，初始化其两个成员变量，然后显示每个成员变量的值。

    ```java
    public class NumberHolder {
        public int anInt;
        public float aFloat;
    }

    ```

检查你的答案。
