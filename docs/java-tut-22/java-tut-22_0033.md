# 表达式、语句和块

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/expressions.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/expressions.html)

现在你已经了解了变量和运算符，是时候学习 *表达式*、*语句* 和 *块* 了。运算符可以用于构建计算值的表达式；表达式是语句的核心组件；语句可以分组成块。

## 表达式

一个 *表达式* 是由变量、运算符和方法调用构成的构造，根据语言的语法构造，计算为单个值。你已经看到了表达式的示例，如下所示：

```java
int **cadence = 0**;
**anArray[0] = 100**;
System.out.println(**"Element 1 at index 0: " + anArray[0]**);

int **result = 1 + 2**; // result is now 3
if (**value1 == value2**) 
    System.out.println(**"value1 == value2"**);

```

表达式返回的值的数据类型取决于表达式中使用的元素。表达式 `cadence = 0` 返回一个 `int`，因为赋值运算符返回与其左操作数相同数据类型的值；在这种情况下，`cadence` 是一个 `int`。从其他表达式中可以看到，表达式也可以返回其他类型的值，比如 `boolean` 或 `String`。

Java 编程语言允许你从各种较小的表达式构建复合表达式，只要表达式的一部分所需的数据类型与另一部分的数据类型匹配。以下是一个复合表达式的示例：

```java

1 * 2 * 3

```

在这个特定的例子中，表达式的计算顺序并不重要，因为乘法的结果与顺序无关；无论你如何应用乘法，结果始终相同。然而，并非所有表达式都是如此。例如，下面的表达式根据你是先执行加法还是除法操作而得出不同的结果：

```java
x + y / 100    // ambiguous

```

你可以使用平衡的括号（( 和 )）来明确指定表达式的计算方式。例如，为了使前面的表达式不含糊，你可以写成以下形式：

```java

(x + y) / 100  // unambiguous, recommended

```

如果你不明确指定操作的执行顺序，操作的顺序将由表达式中使用的运算符分配的优先级决定。具有更高优先级的运算符首先计算。例如，除法运算符的优先级高于加法运算符。因此，以下两个语句是等价的：

```java
x + y / 100 

x + (y / 100) // unambiguous, recommended

```

在编写复合表达式时，要明确指出哪些运算符应该首先计算，并用括号表示。这种做法使代码更易于阅读和维护。

## 语句

语句大致相当于自然语言中的句子。*语句* 形成一个完整的执行单元。以下类型的表达式可以通过在表达式末尾加上分号 (`;`) 来转换为语句。

+   赋值表达式

+   任何使用 `++` 或 `--` 的情况

+   方法调用

+   对象创建表达式

这些语句被称为 *表达语句*。以下是一些表达语句的例子。

```java
// assignment statement
aValue = 8933.234;
// increment statement
aValue++;
// method invocation statement
System.out.println("Hello World!");
// object creation statement
Bicycle myBike = new Bicycle();

```

除了表达语句外，还有另外两种语句：*声明语句* 和 *控制流语句*。*声明语句* 用于声明一个变量。你已经看过很多声明语句的例子了：

```java
// declaration statement
double aValue = 8933.234;

```

最后，*控制流语句* 调节语句执行的顺序。你将在下一节学习有关控制流语句的内容，控制流语句。

## 块

一个 *块* 是在平衡大括号之间的零个或多个语句组成的组，可以在允许单个语句的任何地方使用。下面的例子，`BlockDemo`，演示了块的使用：

```java
class BlockDemo {
     public static void main(String[] args) {
          boolean condition = true;
          if (condition) { // begin block 1
               System.out.println("Condition is true.");
          } // end block one
          else { // begin block 2
               System.out.println("Condition is false.");
          } // end block 2
     }
}

```
