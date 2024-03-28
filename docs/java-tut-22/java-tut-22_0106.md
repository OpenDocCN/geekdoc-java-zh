# 字符

> 原文：[`docs.oracle.com/javase/tutorial/java/data/characters.html`](https://docs.oracle.com/javase/tutorial/java/data/characters.html)

大多数情况下，如果您使用单个字符值，您将使用原始的`char`类型。例如：

```java
char ch = 'a'; 
// Unicode for uppercase Greek omega character
char uniChar = '\u03A9';
// an array of chars
char[] charArray = { 'a', 'b', 'c', 'd', 'e' };

```

然而，有时您需要将 char 用作对象例如，作为期望对象的方法参数。Java 编程语言提供了一个*包装器*类，用于将`char`包装在`Character`对象中。`Character`类型的对象包含一个单一字段，其类型为`char`。这个[Character](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html)类还提供了许多有用的类（即静态）方法来操作字符。

您可以使用`Character`构造函数创建一个`Character`对象：

```java
Character ch = new Character('a');

```

Java 编译器在某些情况下也会为您创建一个`Character`对象。例如，如果您将一个原始的`char`传递给一个期望对象的方法，编译器会自动为您将`char`转换为`Character`。这个特性被称为*自动装箱*或者*拆箱*，如果转换是另一种方式的话。有关自动装箱和拆箱的更多信息，请参阅自动装箱和拆箱。

* * *

**注意：** `Character`类是不可变的，一旦创建，`Character`对象就无法更改。

* * *

以下表列出了`Character`类中一些最有用的方法，但并非详尽无遗。要查看此类中所有方法的完整列表（超过 50 个），请参考[java.lang.Character](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html) API 规范。

`Character`类中的有用方法

| 方法 | 描述 |
| --- | --- |
| `boolean isLetter(char ch) boolean isDigit(char ch)` | 确定指定的 char 值是字母还是数字。 |
| `boolean isWhitespace(char ch)` | 确定指定的 char 值是否为空格。 |
| `boolean isUpperCase(char ch) boolean isLowerCase(char ch)` | 确定指定的 char 值是大写还是小写。 |
| `char toUpperCase(char ch) char toLowerCase(char ch)` | 返回指定 char 值的大写或小写形式。 |
| `toString(char ch)` | 返回表示指定字符值的`String`对象  也就是一个字符的字符串。 |

## 转义序列

一个由反斜杠（\）引导的字符是一个*转义序列*，对编译器具有特殊含义。下表显示了 Java 转义序列：

转义序列

| 转义序列 | 描述 |
| --- | --- |
| `\t` | 在文本中插入一个制表符。 |
| `\b` | 在文本中插入一个退格符。 |
| `\n` | 在文本中插入一个换行符。 |
| `\r` | 在文本中插入一个回车符。 |
| `\f` | 在文本中插入一个换页符。 |
| `\'` | 在文本中插入一个单引号字符。 |
| `\"` | 在文本中插入一个双引号字符。 |
| `\\` | 在文本中插入一个反斜杠字符。 |

当在打印语句中遇到转义序列时，编译器会相应地解释它。例如，如果你想在引号内放置引号，你必须使用转义序列 `\"` 来处理内部引号。要打印这个句子

```java
She said "Hello!" to me.

```

你需要写

```java
System.out.println("She said \"Hello!\" to me.");

```
