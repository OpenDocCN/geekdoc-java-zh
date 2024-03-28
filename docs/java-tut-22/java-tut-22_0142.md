# 使用包成员

> 原文：[`docs.oracle.com/javase/tutorial/java/package/usepkgs.html`](https://docs.oracle.com/javase/tutorial/java/package/usepkgs.html)

组成包的类型被称为*包成员*。

要从其包外部使用`public`包成员，你必须执行以下操作之一：

+   通过其完全限定名称引用成员

+   导入包成员

+   导入成员的整个包

每种情况都适用于不同的情况，如下面的部分所解释的。

## 通过其限定名称引用包成员

到目前为止，在本教程中的大多数示例都通过其简单名称引用类型，如`Rectangle`和`StackOfInts`。如果你编写的代码与该成员在同一个包中，或者已经导入了该成员，你可以使用包成员的简单名称。

但是，如果你尝试使用来自不同包的成员，并且该包尚未被导入，你必须使用成员的完全限定名称，其中包括包名称。以下是在前面示例中声明的`graphics`包中的`Rectangle`类的完全限定名称。

```java
graphics.Rectangle

```

你可以使用这个限定名称来创建`graphics.Rectangle`的实例：

```java
graphics.Rectangle myRect = new graphics.Rectangle();

```

对于不经常使用的限定名称是可以的。然而，当一个名称被重复使用时，反复输入名称变得乏味，代码变得难以阅读。作为替代方案，你可以*导入*成员或其包，然后使用其简单名称。

## 导入包成员

要将特定成员导入当前文件，请在文件开头放置一个`import`语句，在任何类型定义之前，但在`package`语句之后（如果有的话）。以下是如何从前一节中创建的`graphics`包中导入`Rectangle`类。

```java
import graphics.Rectangle;

```

现在你可以通过其简单名称引用`Rectangle`类。

```java
Rectangle myRectangle = new Rectangle();

```

如果你只从`graphics`包中使用少量成员，这种方法很有效。但如果你从一个包中使用许多类型，你应该导入整个包。

## 导入整个包

要导入特定包中包含的所有类型，请使用带有星号`(*)`通配符的`import`语句。

```java
import graphics.*;

```

现在你可以通过其简单名称引用`graphics`包中的任何类或接口。

```java
Circle myCircle = new Circle();
Rectangle myRectangle = new Rectangle();

```

`import`语句中的星号只能用于指定包中的所有类，如下所示。它不能用于匹配包中的一部分类。例如，以下内容不匹配以`A`开头的`graphics`包中的所有类。

```java
// *does not work*
import graphics.A*;

```

相反，它会生成编译器错误。通常情况下，使用`import`语句只导入单个包成员或整个包。

* * *

**注意：** 另一种不太常见的 `import` 形式允许你导入封闭类的公共嵌套类。例如，如果 `graphics.Rectangle` 类包含有用的嵌套类，比如 `Rectangle.DoubleWide` 和 `Rectangle.Square`，你可以通过以下*两个*语句导入 `Rectangle` 及其嵌套类。

```java
import graphics.Rectangle;
import graphics.Rectangle.*;

```

请注意，第二个导入语句*不会*导入 `Rectangle`。

另一种不太常见的 `import` 形式，即*静态导入语句*，将在本节末尾讨论。

* * *

为了方便起见，Java 编译器会自动为每个源文件导入两个完整的包：（1）`java.lang` 包和（2）当前包（当前文件的包）。

## 包的表面层次结构

起初，包看起来是分层的，但实际上并非如此。例如，Java API 包括一个 `java.awt` 包，一个 `java.awt.color` 包，一个 `java.awt.font` 包，以及许多以 `java.awt` 开头的其他包。然而，`java.awt.color` 包、`java.awt.font` 包和其他 `java.awt.xxxx` 包*不包含*在 `java.awt` 包中。前缀 `java.awt`（Java 抽象窗口工具包）用于一些相关的包，以明确显示它们之间的关系，而不是表示包含关系。

导入 `java.awt.*` 导入了 `java.awt` 包中的所有类型，但*不会导入* `java.awt.color`、`java.awt.font` 或任何其他 `java.awt.xxxx` 包。如果你计划使用 `java.awt.color` 中的类以及 `java.awt` 中的类，你必须导入这两个包及其所有文件：

```java
import java.awt.*;
import java.awt.color.*;

```

## 名称歧义

如果一个包中的成员与另一个包中的成员同名，并且两个包都被导入，你必须通过其限定名称引用每个成员。例如，`graphics` 包定义了一个名为 `Rectangle` 的类。`java.awt` 包也包含一个 `Rectangle` 类。如果 `graphics` 和 `java.awt` 都被导入，以下内容是模棱两可的。

```java
Rectangle rect;

```

在这种情况下，你必须使用成员的完全限定名称来指示你想要的确切 `Rectangle` 类。例如，

```java
graphics.Rectangle rect;

```

## 静态导入语句

有些情况下，你需要频繁访问一个或两个类的静态 final 字段（常量）和静态方法。反复添加这些类的名称可能会导致代码混乱。*静态导入*语句为你提供了一种导入你想要使用的常量和静态方法的方式，这样你就不需要为它们的类名添加前缀。

`java.lang.Math` 类定义了 `PI` 常量和许多静态方法，包括用于计算正弦、余弦、正切、平方根、最大值、最小值、指数等的方法。例如，

```java
public static final double PI 
    = 3.141592653589793;
public static double cos(double a)
{
    ...
}

```

通常，要从另一个类中使用这些对象，你需要添加类名前缀，如下所示。

```java
double r = Math.cos(Math.PI * theta);

```

使用静态导入语句可以导入 java.lang.Math 的静态成员，这样就不需要在类名`Math`前加前缀了。`Math`的静态成员可以单独导入：

```java
import static java.lang.Math.PI;

```

或者作为一个组：

```java
import static java.lang.Math.*;

```

一旦它们被导入，静态成员可以无需限定地使用。例如，前面的代码片段将变为：

```java
double r = cos(PI * theta);

```

显然，你可以编写自己的类，其中包含你经常使用的常量和静态方法，然后使用静态导入语句。例如，

```java
import static mypackage.MyConstants.*;

```

* * *

**注意：** 静态导入要非常谨慎使用。过度使用静态导入会导致代码难以阅读和维护，因为代码读者无法知道哪个类定义了特定的静态对象。正确使用静态导入可以通过消除类名重复使代码更易读。

* * *
