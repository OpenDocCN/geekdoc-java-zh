# 定义方法

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/methods.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/methods.html)

这是一个典型方法声明的示例：

```java
public double calculateAnswer(double wingSpan, int numberOfEngines,
                              double length, double grossTons) {
    //do the calculation here
}

```

方法声明的唯一必需元素是方法的返回类型、名称、一对括号`()`，以及大括号`{}`之间的方法体。

更一般地，方法声明有六个组成部分，顺序如下：

1.  修饰符例如`public`、`private`等，以及其他你将在后面学习的内容。

1.  返回类型方法返回的值的数据类型，或者如果方法不返回值，则为`void`。

1.  方法名称字段名称的规则也适用于方法名称，但约定略有不同。

1.  括号中的参数列表以逗号分隔的输入参数列表，前面是它们的数据类型，用括号`()`括起来。如果没有参数，必须使用空括号。

1.  一个异常列表稍后讨论。

1.  方法体，用大括号括起来方法的代码，包括局部变量的声明，在这里。

修饰符、返回类型和参数将在本课程的后续部分讨论。异常将在后续课程中讨论。

* * *

**定义：** 方法声明的两个组成部分构成了*方法签名*方法的名称和参数类型。

* * *

上面声明的方法的签名是：

```java
calculateAnswer(double, int, double, double)

```

## 命名方法

尽管方法名称可以是任何合法标识符，但代码约定限制了方法名称。按照约定，方法名称应该是小写的动词或以小写动词开头的多词名称，后面跟着形容词、名词等。在多词名称中，第二个及后续单词的第一个字母应大写。以下是一些示例：

```java
run
runFast
getBackground
getFinalData
compareTo
setX
isEmpty

```

通常，一个方法在其类中具有唯一的名称。但是，由于*方法重载*，一个方法可能与其他方法具有相同的名称。

## 方法重载

Java 编程语言支持*方法重载*，并且 Java 可以区分具有不同*方法签名*的方法。这意味着类中的方法如果具有不同的参数列表，则可以具有相同的名称（对此有一些限制，将在标题为“接口和继承”的课程中讨论）。

假设你有一个可以使用书法来绘制各种类型数据（字符串、整数等）的类，并且包含一个用于绘制每种数据类型的方法。为每个方法使用新名称很麻烦例如，`drawString`、`drawInteger`、`drawFloat`等。在 Java 编程语言中，你可以为所有绘制方法使用相同的名称，但对每个方法传递不同的参数列表。因此，数据绘制类可能声明四个名为`draw`的方法，每个方法都有不同的参数列表。

```java
public class DataArtist {
    ...
    public void draw(String s) {
        ...
    }
    public void draw(int i) {
        ...
    }
    public void draw(double f) {
        ...
    }
    public void draw(int i, double f) {
        ...
    }
}

```

过载方法通过传入方法的参数的数量和类型来区分。在代码示例中，`draw(String s)` 和 `draw(int i)` 是不同且独特的方法，因为它们需要不同的参数类型。

你不能声明多个具有相同名称和相同数量及类型参数的方法，因为编译器无法区分它们。

编译器在区分方法时不考虑返回类型，因此即使它们具有不同的返回类型，也不能声明具有相同签名的两个方法。

* * *

**注意：** 过载方法应该谨慎使用，因为它们会使代码变得难以阅读。

* * *
