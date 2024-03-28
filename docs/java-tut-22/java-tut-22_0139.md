# 创建和使用包

> 原文：[`docs.oracle.com/javase/tutorial/java/package/packages.html`](https://docs.oracle.com/javase/tutorial/java/package/packages.html)

为了使类型更容易找到和使用，避免命名冲突，并控制访问权限，程序员将相关类型的组合打包成包。

* * *

**定义：** *包* 是提供访问保护和命名空间管理的相关类型的分组。请注意，*类型* 指的是类、接口、枚举和注解类型。枚举和注解类型是类和接口的特殊种类，因此在本课程中，*类型* 经常简称为*类和接口*。

* * *

Java 平台中的类型是通过功能将类打包在一起的各种包的成员：基本类在`java.lang`中，用于读写（输入和输出）的类在`java.io`中，依此类推。你也可以将你的类型放在包中。

假设你编写了一组表示图形对象的类，比如圆、矩形、线条和点。你还编写了一个接口，`Draggable`，如果类可以被鼠标拖动，则实现该接口。

```java
//*in the Draggable.java file*
public interface Draggable {
    ...
}

//*in the Graphic.java file*
public abstract class Graphic {
    ...
}

//*in the Circle.java file*
public class Circle extends Graphic
    implements Draggable {
    . . .
}

//*in the Rectangle.java file*
public class Rectangle extends Graphic
    implements Draggable {
    . . .
}

//*in the Point.java file*
public class Point extends Graphic
    implements Draggable {
    . . .
}

//*in the Line.java file*
public class Line extends Graphic
    implements Draggable {
    . . .
}

```

你应该将这些类和接口打包到一个包中，原因包括以下几点：

+   你和其他程序员可以轻松确定这些类型是相关的。

+   你和其他程序员知道在哪里找到可以提供与图形相关功能的类型。

+   你的类型名称不会与其他包中的类型名称冲突，因为包会创建一个新的命名空间。

+   你可以允许包内的类型彼此之间具有无限制的访问，但仍然限制包外类型的访问。
