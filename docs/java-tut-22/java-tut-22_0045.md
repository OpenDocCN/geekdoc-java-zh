# 声明类

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html)

你已经看到以下方式定义的类：

```java
class *MyClass* {
    // field, constructor, and 
    // method declarations
}

```

这是一个*类声明*。*类体*（大括号之间的区域）包含了为从类创建的对象的生命周期提供支持的所有代码：用于初始化新对象的构造函数，提供类及其对象状态的字段声明，以及实现类及其对象行为的方法。

前面的类声明是一个最小的类声明。它只包含了类声明中所需的组件。你可以在类声明的开头提供关于类的更多信息，比如其超类的名称，是否实现了任何接口等。例如，

```java
class *MyClass extends MySuperClass implements YourInterface* {
    // field, constructor, and
    // method declarations
}

```

意味着`MyClass`是`MySuperClass`的子类，并且实现了`YourInterface`接口。

你也可以在最开始添加像*public*或*private*这样的修饰符所以你可以看到类声明的开头行可能会变得相当复杂。决定其他类能否访问`MyClass`的修饰符*public*和*private*将在本课程的后面讨论。关于接口和继承的课程将解释在类声明中为什么以及如何使用*extends*和*implements*关键字。目前你不需要担心这些额外的复杂性。

一般来说，类声明可以按顺序包括这些组件：

1.  修饰符，比如*public*、*private*以及你以后会遇到的其他一些修饰符。（但是，请注意，*private*修饰符只能应用于嵌套类。）

1.  类名，按照约定首字母大写。

1.  类的父类（超类）的名称，如果有的话，前面带有关键字*extends*。一个类只能*扩展*（子类化）一个父类。

1.  一个逗号分隔的接口列表，如果有的话，前面带有关键字*implements*。一个类可以*实现*多个接口。

1.  类体，用大括号{}括起来。
