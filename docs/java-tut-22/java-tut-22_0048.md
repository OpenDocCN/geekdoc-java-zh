# 为你的类提供构造函数

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/constructors.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/constructors.html)

一个类包含用于从类蓝图创建对象的构造函数。构造函数声明看起来像方法声明，只是它们使用类的名称并且没有返回类型。例如，`Bicycle`有一个构造函数：

```java
public Bicycle(int startCadence, int startSpeed, int startGear) {
    gear = startGear;
    cadence = startCadence;
    speed = startSpeed;
}

```

要创建一个名为`myBike`的新`Bicycle`对象，需要通过`new`运算符调用构造函数：

```java
Bicycle myBike = new Bicycle(30, 0, 8);

```

`new Bicycle(30, 0, 8)`在内存中为对象创建空间并初始化其字段。

尽管`Bicycle`只有一个构造函数，但它可以有其他构造函数，包括无参数构造函数：

```java
public Bicycle() {
    gear = 1;
    cadence = 10;
    speed = 0;
}

```

`Bicycle yourBike = new Bicycle();`调用无参数构造函数以创建一个名为`yourBike`的新`Bicycle`对象。

由于它们具有不同的参数列表，两个构造函数都可以在`Bicycle`中声明。与方法一样，Java 平台根据参数列表中参数的数量和类型区分构造函数。你不能为同一个类编写具有相同数量和类型参数的两个构造函数，因为平台无法区分它们。这样做会导致编译时错误。

你不必为你的类提供任何构造函数，但在这样做时必须小心。编译器会自动为没有构造函数的任何类提供一个无参数默认构造函数。这个默认构造函数将调用超类的无参数构造函数。在这种情况下，如果超类没有无参数构造函数，编译器会报错，因此你必须验证它是否有。如果你的类没有显式的超类，那么它有一个隐式的超类`Object`，它有一个无参数构造函数。

你可以自己使用超类构造函数。本课程开头的`MountainBike`类就是这样做的。这将在后面关于接口和继承的课程中讨论。

你可以在构造函数的声明中使用访问修饰符来控制哪些其他类可以调用该构造函数。

* * *

**注意：**如果另一个类无法调用`MyClass`构造函数，则无法直接创建`MyClass`对象。

* * *
