# 问题和练习：接口

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/QandE/interfaces-questions.html`](https://docs.oracle.com/javase/tutorial/java/IandI/QandE/interfaces-questions.html)

## 问题

1.  一个实现`java.lang.CharSequence`接口的类需要实现哪些方法？

1.  以下接口有什么问题？

    ```java
    public interface SomethingIsWrong {
        void aMethod(int aValue){
            System.out.println("Hi Mom");
        }
    }

    ```

1.  修复问题 2 中的接口。

1.  以下接口是否有效？

    ```java
    public interface Marker {
    }

    ```

## 练习

1.  编写一个实现`java.lang`包中`CharSequence`接口的类。你的实现应该将字符串倒序返回。从本书中选择一句话作为数据。编写一个小的`main`方法来测试你的类；确保调用所有四个方法。

1.  假设你已经编写了一个定期通知其客户端当前日期和时间的时间服务器。编写一个接口，服务器可以使用它来强制执行特定的协议。

检查你的答案。
