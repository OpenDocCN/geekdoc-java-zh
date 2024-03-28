# 通配符

> 原文：[`docs.oracle.com/javase/tutorial/extra/generics/wildcards.html`](https://docs.oracle.com/javase/tutorial/extra/generics/wildcards.html)

考虑编写一个打印集合中所有元素的例程的问题。以下是你可能在语言的旧版本（即 5.0 版本之前）中编写的方式：

```java
void printCollection(Collection c) {
    Iterator i = c.iterator();
    for (k = 0; k < c.size(); k++) {
        System.out.println(i.next());
    }
}

```

这是一个使用泛型（和新的`for`循环语法）编写的天真尝试：

```java
void printCollection(Collection<Object> c) {
    for (Object e : c) {
        System.out.println(e);
    }
}

```

问题在于这个新版本比旧版本要不实用得多。而旧代码可以使用任何类型的集合作为参数调用，新代码只接受`Collection<Object>`，正如我们刚刚证明的，这**不是**所有种类集合的超类型！

那么所有种类的集合的**超类型**是什么呢？它被写作`Collection<?>`（读作"未知类型的集合"），即元素类型匹配任何内容的集合。它被称为**通配符类型**，原因显而易见。我们可以这样写：

```java
void printCollection(Collection<?> c) {
    for (Object e : c) {
        System.out.println(e);
    }
}

```

现在，我们可以使用任何类型的集合调用它。请注意，在`printCollection()`内部，我们仍然可以从`c`中读取元素并将它们赋予类型`Object`。这总是安全的，因为无论集合的实际类型是什么，它都包含对象。但是向其中添加任意对象是不安全的：

```java
Collection<?> c = new ArrayList<String>();
c.add(new Object()); // Compile time error

```

由于我们不知道`c`的元素类型代表什么，我们无法向其添加对象。`add()`方法接受类型为`E`的参数，即集合的元素类型。当实际类型参数为`?`时，它代表某种未知类型。我们传递给`add`的任何参数都必须是这种未知类型的子类型。由于我们不知道那种类型是什么，我们无法传递任何内容。唯一的例外是`null`，它是每种类型的成员。

另一方面，对于给定的`List<?>`，我们**可以**调用`get()`并利用结果。结果类型是一个未知类型，但我们始终知道它是一个对象。因此，将`get()`的结果分配给类型为`Object`的变量或将其作为期望类型为`Object`的参数传递是安全的。

## 有界通配符

考虑一个简单的绘图应用程序，可以绘制矩形和圆形等形状。为了在程序中表示这些形状，你可以定义一个类层次结构，如下所示：

```java
public abstract class Shape {
    public abstract void draw(Canvas c);
}

public class Circle extends Shape {
    private int x, y, radius;
    public void draw(Canvas c) {
        ...
    }
}

public class Rectangle extends Shape {
    private int x, y, width, height;
    public void draw(Canvas c) {
        ...
    }
}

```

这些类可以在画布上绘制：

```java
public class Canvas {
    public void draw(Shape s) {
        s.draw(this);
   }
}

```

任何绘图通常会包含许多形状。假设它们被表示为一个列表，那么在`Canvas`中有一个绘制它们所有的方法会很方便：

```java
public void drawAll(List<Shape> shapes) {
    for (Shape s: shapes) {
        s.draw(this);
   }
}

```

现在，类型规则表明`drawAll()`只能在`Shape`的列表上调用：例如，不能在`List<Circle>`上调用。这很不幸，因为该方法所做的只是从列表中读取形状，所以它同样可以在`List<Circle>`上调用。我们真正想要的是该方法接受**任何**种类的形状列表：

```java
public void drawAll(List<? extends Shape> shapes) {
    ...
}

```

这里有一个很小但非常重要的区别：我们用`List<? **extends** Shape>`替换了类型`List<Shape>`。现在`drawAll()`将接受任何`Shape`的子类列表，因此我们现在可以在`List<Circle>`上调用它。

`List<? **extends** Shape>`是*有界通配符*的一个例子。`?`代表一个未知类型，就像我们之前看到的通配符一样。然而，在这种情况下，我们知道这个未知类型实际上是`Shape`的子类型。（注意：它可以是`Shape`本身，或者某个子类；它不一定要直接扩展`Shape`。）我们说`Shape`是通配符的*上界*。

使用通配符灵活性的代价通常是很高的。这个代价是在方法体中写入`shapes`现在是非法的。例如，下面的写法是不允许的：

```java
public void addRectangle(List<? extends Shape> shapes) {
    // *Compile-time error!*
    shapes.add(0, new Rectangle());
}

```

你应该能够弄清楚为什么上面的代码是不允许的。`shapes.add()`的第二个参数的类型是`? **extends** Shape`-- 一个未知的`Shape`子类型。由于我们不知道它的类型，我们也不知道它是否是`Rectangle`的超类型；它可能是也可能不是这样的超类型，因此在这里传递`Rectangle`是不安全的。

有界通配符正是处理 DMV 将其数据传递给人口普查局的例子所需的。我们的例子假设数据由从名称（表示为字符串）到人员（由`Person`或其子类型，如`Driver`表示的引用类型）的映射表示。`Map<K,V>`是一个接受两个类型参数的泛型类型的例子，表示映射的键和值。

再次注意正式类型参数的命名约定--`K`代表键，`V`代表值。

```java
public class Census {
    public static void addRegistry(Map<String, ? extends Person> registry) {
}
...

Map<String, Driver> allDrivers = ... ;
Census.addRegistry(allDrivers);

```
