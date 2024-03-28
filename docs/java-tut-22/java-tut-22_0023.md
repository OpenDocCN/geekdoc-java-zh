# 原始数据类型

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)

Java 编程语言是静态类型的，这意味着所有变量在使用之前必须先声明。这涉及声明变量的类型和名称，就像你已经看到的那样：

```java
int gear = 1;

```

这样做告诉您的程序，存在一个名为“gear”的字段，保存数值数据，并具有“1”的初始值。变量的数据类型确定它可能包含的值，以及可以对其执行的操作。除了`int`之外，Java 编程语言还支持其他七种*原始数据类型*。原始类型由语言预定义，并由保留关键字命名。原始值不与其他原始值共享状态。Java 编程语言支持的八种原始数据类型是：

+   **byte**: `byte` 数据类型是一个 8 位有符号的二进制补码整数。它的最小值为-128，最大值为 127（包括边界值）。在需要实际节省内存的大数组中，`byte`数据类型可能很有用。它们也可以用于代替`int`，在这种情况下，它们的限制有助于澄清代码；变量的范围受限可以作为一种文档形式。

+   **short**: `short` 数据类型是一个 16 位有符号的二进制补码整数。它的最小值为-32,768，最大值为 32,767（包括边界值）。与`byte`一样，相同的准则适用：在需要实际节省内存的大数组中，可以使用`short`来节省内存。

+   **int**: 默认情况下，`int`数据类型是一个 32 位有符号的二进制补码整数，其最小值为-2³¹，最大值为 2³¹-1。在 Java SE 8 及更高版本中，可以使用`int`数据类型来表示无符号的 32 位整数，其最小值为 0，最大值为 2³²-1。使用 Integer 类将`int`数据类型用作无符号整数。有关更多信息，请参阅“数字类”部分。已向[`Integer`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html)类添加了`compareUnsigned`、`divideUnsigned`等静态方法，以支持无符号整数的算术运算。

+   **long**: `long` 数据类型是一个 64 位的二进制补码整数。有符号的`long`的最小值为-2⁶³，最大值为 2⁶³-1。在 Java SE 8 及更高版本中，可以使用`long`数据类型来表示无符号的 64 位长整数，其最小值为 0，最大值为 2⁶⁴-1。当需要比`int`提供的值范围更广时，请使用此数据类型。[`Long`](https://docs.oracle.com/javase/8/docs/api/java/lang/Long.html)类还包含诸如`compareUnsigned`、`divideUnsigned`等方法，以支持无符号长整数的算术运算。

+   **float**：`float`数据类型是一个单精度 32 位 IEEE 754 浮点数。其值的范围超出了本讨论的范围，但在 Java 语言规范的[浮点类型、格式和值](https://docs.oracle.com/javase/specs/jls/se7/html/jls-4.html#jls-4.2.3)部分中有规定。与对`byte`和`short`的建议一样，如果需要在大量浮点数数组中节省内存，则使用`float`（而不是`double`）。这种数据类型永远不应该用于精确值，比如货币。对于这种情况，你将需要使用[java.math.BigDecimal](https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html)类。数字和字符串介绍了`BigDecimal`和 Java 平台提供的其他有用类。

+   **double**：`double`数据类型是一个双精度 64 位 IEEE 754 浮点数。其值的范围超出了本讨论的范围，但在 Java 语言规范的[浮点类型、格式和值](https://docs.oracle.com/javase/specs/jls/se7/html/jls-4.html#jls-4.2.3)部分中有规定。对于十进制值，这种数据类型通常是默认选择。如上所述，这种数据类型永远不应该用于精确值，比如货币。

+   **boolean**：`boolean`数据类型只有两个可能的值：`true`和`false`。使用这种数据类型来跟踪真/假条件的简单标志。这种数据类型表示一位信息，但其“大小”并没有明确定义。

+   **char**：`char`数据类型是一个单个的 16 位 Unicode 字符。它的最小值为`'\u0000'`（或 0），最大值为`'\uffff'`（或 65,535，包括在内）。

除了上面列出的八种基本数据类型外，Java 编程语言还通过[java.lang.String](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)类提供了对字符字符串的特殊支持。将字符字符串放在双引号中将自动创建一个新的`String`对象；例如，`String s = "this is a string";`。`String`对象是*不可变*的，这意味着一旦创建，它们的值就不能被更改。`String`类在技术上不是一个基本数据类型，但考虑到语言对它的特殊支持，你可能会倾向于将其视为基本数据类型。你将在简单数据对象中了解更多关于`String`类的信息。

## 默认值

在声明字段时，不总是需要为其赋值。声明但未初始化的字段将由编译器设置为一个合理的默认值。一般来说，这个默认值将是零或`null`，取决于数据类型。然而，依赖这些默认值通常被认为是不良的编程风格。

以下图表总结了上述数据类型的默认值。

| **数据类型** | **默认值（对于字段）** |
| --- | --- |
| byte | 0 |
| short | 0 |
| int | 0 |
| long | 0L |
| float | 0.0f |
| double | 0.0d |
| char | '\u0000' |
| String（或任何对象） | null |
| boolean | false |

局部变量略有不同；编译器永远不会为未初始化的局部变量分配默认值。如果无法在声明变量的地方初始化局部变量，请确保在尝试使用之前为其赋值。访问未初始化的局部变量将导致编译时错误。

### 字面值

您可能已经注意到，在初始化原始类型变量时不使用 `new` 关键字。原始类型是内置到语言中的特殊数据类型；它们不是从类创建的对象。*字面值* 是固定值的源代码表示形式；字面值直接在您的代码中表示，无需计算。如下所示，可以将字面值分配给原始类型变量：

```java
boolean result = true;
char capitalC = 'C';
byte b = 100;
short s = 10000;
int i = 100000;

```

#### 整数字面值

如果整数字面值以字母 `L` 或 `l` 结尾，则其类型为 `long`；否则为 `int`。建议使用大写字母 `L`，因为小写字母 `l` 很难与数字 `1` 区分。

整型 `byte`、`short`、`int` 和 `long` 的值可以从 `int` 字面值创建。超出 `int` 范围的 `long` 类型值可以从 `long` 字面值创建。整数字面值可以用以下数字系统表示：

+   十进制：基数为 10，其数字由数字 0 到 9 组成；这是您每天使用的数字系统

+   十六进制：基数为 16，其数字由数字 0 到 9 和字母 A 到 F 组成

+   二进制：基数为 2，其数字由数字 0 和 1 组成（您可以在 Java SE 7 及更高版本中创建二进制字面值）

对于通用编程，十进制系统可能是您唯一使用的数字系统。但是，如果需要使用其他数字系统，以下示例显示了正确的语法。前缀 `0x` 表示十六进制，`0b` 表示二进制：

```java
// The number 26, in decimal
int decVal = 26;
//  The number 26, in hexadecimal
int hexVal = 0x1a;
// The number 26, in binary
int binVal = 0b11010;

```

#### 浮点字面值

浮点字面值如果以字母 `F` 或 `f` 结尾，则其类型为 `float`；否则其类型为 `double`，并且可以选择以字母 `D` 或 `d` 结尾。

浮点类型（`float` 和 `double`）也可以使用 E 或 e（科学计数法）、F 或 f（32 位浮点字面值）和 D 或 d（64 位双精度字面值；这是默认值，按照惯例省略）来表示。

```java
double d1 = 123.4;
// same value as d1, but in scientific notation
double d2 = 1.234e2;
float f1  = 123.4f;

```

#### 字符和字符串字面值

`char`和`String`类型的字面量可以包含任何 Unicode（UTF-16）字符。如果您的编辑器和文件系统允许，您可以直接在代码中使用这些字符。如果不允许，您可以使用"Unicode 转义"，例如`'\u0108'`（带抑扬符的大写 C），或`"S\u00ED Se\u00F1or"`（西班牙语中的 Sí Señor）。始终使用'单引号'表示`char`字面量，使用"双引号"表示`String`字面量。Unicode 转义序列可以在程序的其他地方使用（例如，在字段名称中），不仅仅在`char`或`String`字面量中。

Java 编程语言还支持一些特殊的转义序列，用于`char`和`String`字面量：`\b`（退格）、`\t`（制表符）、`\n`（换行符）、`\f`（换页符）、`\r`（回车符）、`\"`（双引号）、`\'`（单引号）和`\\`（反斜杠）。

还有一个特殊的`null`字面量，可以用作任何引用类型的值。`null`可以赋给任何变量，除了原始类型的变量。对于`null`值，除了测试其是否存在外，几乎无法做任何操作。因此，在程序中，`null`通常用作指示某个对象不可用的标记。

最后，还有一种特殊的字面量称为*类字面量*，通过取类型名称并附加"`.class"`形成；例如，`String.class`。这指的是代表类型本身的对象（类型为`Class`）。

## 在数字字面量中使用下划线字符

在 Java SE 7 及更高版本中，任何数量的下划线字符（`_`）可以出现在数字字面量中的任何位置。此功能使您能够，例如，将数字字面量中的数字分组，从而提高代码的可读性。

例如，如果您的代码包含许多位数的数字，您可以使用下划线字符将数字分组为三位一组，类似于您如何使用逗号或空格等标点符号作为分隔符。

以下示例展示了您可以在数字字面量中使用下划线的其他方式：

```java
long creditCardNumber = 1234_5678_9012_3456L;
long socialSecurityNumber = 999_99_9999L;
float pi =  3.14_15F;
long hexBytes = 0xFF_EC_DE_5E;
long hexWords = 0xCAFE_BABE;
long maxLong = 0x7fff_ffff_ffff_ffffL;
byte nybbles = 0b0010_0101;
long bytes = 0b11010010_01101001_10010100_10010010;

```

您只能在数字之间放置下划线；不能在以下位置放置下划线：

+   在数字的开头或结尾

+   邻近浮点字面量中的小数点

+   在`F`或`L`后缀之前

+   在期望出现一串数字的位置

以下示例演示了数字字面量中有效和无效的下划线放置方式（已突出显示）：

```java
// Invalid: cannot put underscores
// adjacent to a decimal point
float pi1 = 3_.1415F;
// Invalid: cannot put underscores 
// adjacent to a decimal point
float pi2 = 3._1415F;
// Invalid: cannot put underscores 
// prior to an L suffix
long socialSecurityNumber1 = 999_99_9999_L;

// OK (decimal literal)
int x1 = 5_2;
// Invalid: cannot put underscores
// At the end of a literal
int x2 = 52_;
// OK (decimal literal)
int x3 = 5_______2;

// Invalid: cannot put underscores
// in the 0x radix prefix
int x4 = 0_x52;
// Invalid: cannot put underscores
// at the beginning of a number
int x5 = 0x_52;
// OK (hexadecimal literal)
int x6 = 0x5_2; 
// Invalid: cannot put underscores
// at the end of a number
int x7 = 0x52_;

```
