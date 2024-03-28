# 向方法或构造函数传递信息

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/arguments.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html)

方法或构造函数的声明声明了该方法或构造函数的参数的数量和类型。例如，以下是一个计算房屋贷款月供的方法，基于贷款金额、利率、贷款期限（期数）和贷款的未来价值：

```java
public double computePayment(
                  double loanAmt,
                  double rate,
                  double futureValue,
                  int numPeriods) {
    double interest = rate / 100.0;
    double partial1 = Math.pow((1 + interest), 
                    - numPeriods);
    double denominator = (1 - partial1) / interest;
    double answer = (-loanAmt / denominator)
                    - ((futureValue * partial1) / denominator);
    return answer;
}

```

该方法有四个参数：贷款金额、利率、未来价值和期数。前三个是双精度浮点数，第四个是整数。这些参数在方法体中被使用，并在运行时将采用传递的参数的值。

* * *

**注意：** *参数*指的是方法声明中的变量列表。*参数*是在调用方法时传递的实际值。当您调用方法时，使用的参数必须与声明的参数在类型和顺序上匹配。

* * *

## 参数类型

您可以为方法或构造函数的参数使用任何数据类型。这包括原始数据类型，如双精度、浮点数和整数，就像您在`computePayment`方法中看到的那样，以及引用数据类型，如对象和数组。

这是一个接受数组作为参数的方法的示例。在这个示例中，该方法创建一个新的`Polygon`对象，并从一个`Point`对象数组中初始化它（假设`Point`是表示 x、y 坐标的类）：

```java
public Polygon polygonFrom(Point[] corners) {
    // method body goes here
}

```

* * *

**注意：**如果您想将一个方法传递给另一个方法，那么请使用 lambda 表达式或方法引用。

* * *

## 任意数量的参数

您可以使用称为*varargs*的构造来传递任意数量的值给方法。当您不知道将传递给方法的特定类型的参数有多少时，可以使用 varargs。这是一种快捷方式，可以手动创建数组（前一个方法可以使用 varargs 而不是数组）。

要使用 varargs，您需要在最后一个参数的类型后面加上省略号（三个点，...），然后是一个空格和参数名。然后该方法可以使用任意数量的该参数调用，包括零个。

```java
public Polygon polygonFrom(Point... corners) {
    int numberOfSides = corners.length;
    double squareOfSide1, lengthOfSide1;
    squareOfSide1 = (corners[1].x - corners[0].x)
                     * (corners[1].x - corners[0].x) 
                     + (corners[1].y - corners[0].y)
                     * (corners[1].y - corners[0].y);
    lengthOfSide1 = Math.sqrt(squareOfSide1);

    // more method body code follows that creates and returns a 
    // polygon connecting the Points
}

```

您可以看到，在方法内部，`corners`被视为数组。该方法可以使用数组或一系列参数调用。方法体中的代码将在任何情况下将参数视为数组。

您最常见地会在打印方法中看到 varargs；例如，这个`printf`方法：

```java
public PrintStream printf(String format, Object... args)

```

允许您打印任意数量的对象。可以这样调用：

```java
System.out.printf("%s: %d, %s%n", name, idnum, address);

```

或者像这样

```java
System.out.printf("%s: %d, %s, %s, %s%n", name, idnum, address, phone, email);

```

或者使用不同数量的参数。

## 参数名称

当您向方法或构造函数声明参数时，为该参数提供一个名称。此名称在方法体内用于引用传入的参数。

参数的名称在其作用域内必须是唯一的。它不能与同一方法或构造函数的另一个参数的名称相同，也不能与方法或构造函数内的局部变量的名称相同。

参数可以与类的字段之一具有相同的名称。如果是这种情况，则说参数遮蔽了字段。字段遮蔽可能会使您的代码难以阅读，并且通常仅在设置特定字段的构造函数和方法中使用。例如，考虑以下 `Circle` 类及其 `setOrigin` 方法：

```java
public class Circle {
    private int x, y, radius;
    public void setOrigin(int x, int y) {
        ...
    }
}

```

`Circle` 类有三个字段：`x`、`y` 和 `radius`。`setOrigin` 方法有两个参数，每个参数的名称与一个字段的名称相同。每个方法参数都会遮蔽与其名称相同的字段。因此，在方法体内使用简单名称 `x` 或 `y` 指的是参数，*而不是*字段。要访问字段，必须使用限定名称。这将在本课程的后面部分“使用 `this` 关键字”中讨论。

## 传递原始数据类型参数

原始参数，如 `int` 或 `double`，是通过值传递给方法的。这意味着对参数值的任何更改仅存在于方法的范围内。当方法返回时，参数消失，对它们的任何更改都将丢失。以下是一个例子：

```java
public class PassPrimitiveByValue {

    public static void main(String[] args) {

        int x = 3;

        // invoke passMethod() with 
        // x as argument
        passMethod(x);

        // print x to see if its 
        // value has changed
        System.out.println("After invoking passMethod, x = " + x);

    }

    // change parameter in passMethod()
    public static void passMethod(int p) {
        p = 10;
    }
}

```

运行此程序时，输出为：

```java
After invoking passMethod, x = 3

```

## 传递引用数据类型参数

引用数据类型参数，如对象，也是通过值传递给方法的。这意味着当方法返回时，传入的引用仍然引用与之前相同的对象。*但是*，如果对象的字段值具有适当的访问级别，则可以在方法中更改对象的字段值。

例如，考虑一个在任意类中移动 `Circle` 对象的方法：

```java
public void moveCircle(Circle circle, int deltaX, int deltaY) {
    // code to move origin of circle to x+deltaX, y+deltaY
    circle.setX(circle.getX() + deltaX);
    circle.setY(circle.getY() + deltaY);

    // code to assign a new reference to circle
    circle = new Circle(0, 0);
}

```

让方法使用这些参数被调用：

```java
moveCircle(myCircle, 23, 56)

```

在方法内部，`circle` 最初指向 `myCircle`。该方法更改了 `circle` 引用的对象（即 `myCircle`）的 x 和 y 坐标分别为 23 和 56。这些更改将在方法返回时保留。然后 `circle` 被赋予一个新的 `Circle` 对象的引用，其中 `x = y = 0`。然而，这种重新赋值并不具有永久性，因为引用是通过值传递的，不能更改。在方法内部，`circle` 指向的对象已经改变，但是，当方法返回时，`myCircle` 仍然引用与调用方法之前相同的 `Circle` 对象。
