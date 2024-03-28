# 定义一个接口

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/interfaceDef.html`](https://docs.oracle.com/javase/tutorial/java/IandI/interfaceDef.html)

一个接口声明由修饰符、关键字`interface`、接口名称、一个逗号分隔的父接口列表（如果有）、和接口主体组成。例如：

```java
public interface GroupedInterface extends Interface1, Interface2, Interface3 {

    // constant declarations

    // base of natural logarithms
    double E = 2.718282;

    // method signatures
    void doSomething (int i, double x);
    int doSomethingElse(String s);
}

```

`public`访问修饰符表示接口可以被任何包中的任何类使用。如果不指定接口为 public，则接口只能被与接口在同一包中定义的类访问。

一个接口可以扩展其他接口，就像一个类可以子类化或扩展另一个类一样。然而，一个类只能扩展一个其他类，而一个接口可以扩展任意数量的接口。接口声明包括一个逗号分隔的所有它扩展的接口的列表。

## 接口主体

接口主体可以包含抽象方法，默认方法，和静态方法。接口中的抽象方法后跟一个分号，但不包含大括号（抽象方法不包含实现）。默认方法使用`default`修饰符定义，静态方法使用`static`关键字定义。接口中的所有抽象、默认和静态方法都隐式地是`public`的，因此可以省略`public`修饰符。

此外，一个接口可以包含常量声明。在接口中定义的所有常量值都隐式地是`public`、`static`和`final`的。再次，你可以省略这些修饰符。
