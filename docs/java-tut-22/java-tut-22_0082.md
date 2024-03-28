# 实现一个接口

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/usinginterface.html`](https://docs.oracle.com/javase/tutorial/java/IandI/usinginterface.html)

要声明一个实现接口的类，你需要在类声明中包含一个`implements`子句。你的类可以实现多个接口，因此`implements`关键字后面跟着一个逗号分隔的接口列表。按照惯例，如果有`extends`子句，则`implements`子句跟在其后。

## 一个示例接口，Relatable

考虑一个定义如何比较对象大小的接口。

```java
public interface Relatable {

    // this (object calling isLargerThan)
    // and other must be instances of 
    // the same class returns 1, 0, -1 
    // if this is greater than, 
    // equal to, or less than other
    public int isLargerThan(Relatable other);
}

```

如果你想要比较相似对象的大小，无论它们是什么，实例化它们的类应该实现`Relatable`。

任何类都可以实现`Relatable`，只要有一种方法可以比较从该类实例化的对象的相对“大小”。对于字符串，可以是字符数；对于书籍，可以是页数；对于学生，可以是体重；等等。对于平面几何对象，面积是一个不错的选择（参见下面的`RectanglePlus`类），而对于三维几何对象，体积也可以工作。所有这些类都可以实现`isLargerThan()`方法。

如果你知道一个类实现了`Relatable`，那么你就知道可以比较从该类实例化的对象的大小。

## 实现 Relatable 接口

这里是在创建对象部分中介绍的`Rectangle`类，重写以实现`Relatable`。

```java
public class RectanglePlus 
    implements Relatable {
    public int width = 0;
    public int height = 0;
    public Point origin;

    // four constructors
    public RectanglePlus() {
        origin = new Point(0, 0);
    }
    public RectanglePlus(Point p) {
        origin = p;
    }
    public RectanglePlus(int w, int h) {
        origin = new Point(0, 0);
        width = w;
        height = h;
    }
    public RectanglePlus(Point p, int w, int h) {
        origin = p;
        width = w;
        height = h;
    }

    // a method for moving the rectangle
    public void move(int x, int y) {
        origin.x = x;
        origin.y = y;
    }

    // a method for computing
    // the area of the rectangle
    public int getArea() {
        return width * height;
    }

    // a method required to implement
    // the Relatable interface
    public int isLargerThan(Relatable other) {
        RectanglePlus otherRect 
            = (RectanglePlus)other;
        if (this.getArea() < otherRect.getArea())
            return -1;
        else if (this.getArea() > otherRect.getArea())
            return 1;
        else
            return 0;               
    }
}

```

因为`RectanglePlus`实现了`Relatable`，所以可以比较任意两个`RectanglePlus`对象的大小。

* * *

**注意：** 在`Relatable`接口中定义的`isLargerThan`方法接受一个`Relatable`类型的对象。在前面的示例中加粗显示的代码行将`other`强制转换为`RectanglePlus`实例。类型转换告诉编译器对象的真实类型。直接在`other`实例上调用`getArea`（`other.getArea()`）将无法编译通过，因为编译器不知道`other`实际上是`RectanglePlus`的实例。

* * *
