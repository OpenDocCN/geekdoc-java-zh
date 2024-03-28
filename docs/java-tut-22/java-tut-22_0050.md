# 对象

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/objects.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/objects.html)

一个典型的 Java 程序会创建许多对象，正如您所知，这些对象通过调用方法进行交互。通过这些对象之间的交互，程序可以执行各种任务，比如实现 GUI、运行动画，或者在网络上传输和接收信息。一旦一个对象完成了它被创建的工作，它的资源就会被回收以供其他对象使用。

这里有一个小程序，名为`CreateObjectDemo`，它创建了三个对象：一个`Point`对象和两个`Rectangle`对象。您需要这三个源文件来编译这个程序。

```java

public class CreateObjectDemo {

    public static void main(String[] args) {

        // Declare and create a point object and two rectangle objects.
        Point originOne = new Point(23, 94);
        Rectangle rectOne = new Rectangle(originOne, 100, 200);
        Rectangle rectTwo = new Rectangle(50, 100);

        // display rectOne's width, height, and area
        System.out.println("Width of rectOne: " + rectOne.width);
        System.out.println("Height of rectOne: " + rectOne.height);
        System.out.println("Area of rectOne: " + rectOne.getArea());

        // set rectTwo's position
        rectTwo.origin = originOne;

        // display rectTwo's position
        System.out.println("X Position of rectTwo: " + rectTwo.origin.x);
        System.out.println("Y Position of rectTwo: " + rectTwo.origin.y);

        // move rectTwo and display its new position
        rectTwo.move(40, 72);
        System.out.println("X Position of rectTwo: " + rectTwo.origin.x);
        System.out.println("Y Position of rectTwo: " + rectTwo.origin.y);
    }
}

```

该程序创建、操作并显示有关各种对象的信息。以下是输出：

```java
Width of rectOne: 100
Height of rectOne: 200
Area of rectOne: 20000
X Position of rectTwo: 23
Y Position of rectTwo: 94
X Position of rectTwo: 40
Y Position of rectTwo: 72

```

以下三个部分使用上述示例来描述程序中对象的生命周期。通过它们，您将学习如何编写代码来在您自己的程序中创建和使用对象。您还将了解系统在对象的生命周期结束时如何清理。
