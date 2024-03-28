# 使用 this 关键字

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html)

在实例方法或构造函数中，`this`是对*当前对象*的引用  即正在调用其方法或构造函数的对象。你可以通过使用`this`从实例方法或构造函数中引用当前对象的任何成员。

## 使用`this`与字段

使用`this`关键字的最常见原因是因为字段被方法或构造函数参数遮蔽。

例如，`Point`类是这样写的

```java
public class Point {
    public int x = 0;
    public int y = 0;

    //constructor
    public Point(int a, int b) {
        x = a;
        y = b;
    }
}

```

但它也可以这样写：

```java
public class Point {
    public int x = 0;
    public int y = 0;

    //constructor
    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

```

每个构造函数的参数都会遮蔽对象的一个字段  在构造函数内部，**`x`** 是构造函数第一个参数的一个本地副本。要引用`Point`字段的**`x`**，构造函数必须使用`this.x`。

## 使用`this`与构造函数

在构造函数内部，你也可以使用`this`关键字来调用同一类中的另一个构造函数。这样做被称为*显式构造函数调用*。这里有另一个`Rectangle`类，与 Objects 部分中的实现不同。

```java
public class Rectangle {
    private int x, y;
    private int width, height;

    public Rectangle() {
        this(0, 0, 1, 1);
    }
    public Rectangle(int width, int height) {
        this(0, 0, width, height);
    }
    public Rectangle(int x, int y, int width, int height) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
    }
    ...
}

```

该类包含一组构造函数。每个构造函数都初始化矩形的一些或全部成员变量。构造函数为任何未由参数提供初始值的成员变量提供默认值。例如，无参数构造函数在坐标 0,0 处创建一个 1x1 的`Rectangle`。两个参数的构造函数调用四个参数的构造函数，传入宽度和高度，但始终使用 0,0 坐标。与以前一样，编译器根据参数的数量和类型确定要调用的构造函数。

如果存在，调用另一个构造函数必须是构造函数中的第一行。
