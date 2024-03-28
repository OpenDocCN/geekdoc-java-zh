# 为什么使用泛型？

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/why.html`](https://docs.oracle.com/javase/tutorial/java/generics/why.html)

简而言之，泛型使*类型*（类和接口）在定义类、接口和方法时成为参数。就像在方法声明中使用的更熟悉的*形式参数*一样，类型参数提供了一种方式让您可以重复使用相同的代码以不同的输入。不同之处在于，形式参数的输入是值，而类型参数的输入是类型。

使用泛型的代码比不使用泛型的代码有许多好处：

+   编译时进行更强的类型检查。

    Java 编译器对泛型代码应用强类型检查，如果代码违反类型安全性，则会发出错误。修复编译时错误比修复运行时错误更容易，后者可能很难找到。

+   消除强制类型转换。

    不使用泛型的以下代码片段需要进行强制类型转换：

    ```java
    List list = new ArrayList();
    list.add("hello");
    String s = (String) list.get(0);

    ```

    当重写为使用泛型时，代码不需要进行强制类型转换：

    ```java
    List<String> list = new ArrayList<String>();
    list.add("hello");
    String s = list.get(0);   // no cast

    ```

+   使程序员能够实现泛型算法。

    通过使用泛型，程序员可以实现适用于不同类型集合的泛型算法，可以进行定制，并且是类型安全且更易阅读的。
