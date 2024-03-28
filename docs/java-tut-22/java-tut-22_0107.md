# 字符串

> 原文：[`docs.oracle.com/javase/tutorial/java/data/strings.html`](https://docs.oracle.com/javase/tutorial/java/data/strings.html)

在 Java 编程中广泛使用的字符串是一系列字符。在 Java 编程语言中，字符串是对象。

Java 平台提供了[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)类来创建和操作字符串。

## 创建字符串

创建字符串的最直接方法是写：

```java
String greeting = "Hello world!";

```

在这种情况下，`Hello world!`是一个*字符串字面值*——代码中用双引号括起来的一系列字符。每当编译器在您的代码中遇到字符串字面值时，它会创建一个带有其值的`String`对象——在本例中为`Hello world!`。

与任何其他对象一样，您可以使用`new`关键字和构造函数创建`String`对象。`String`类有十三个构造函数，允许您使用不同的来源（如字符数组）提供字符串的初始值：

```java
char[] helloArray = { 'h', 'e', 'l', 'l', 'o', '.' };
String helloString = new String(helloArray);
System.out.println(helloString);

```

此代码片段的最后一行显示`hello`。

* * *

**注意：**`String`类是不可变的，因此一旦创建了`String`对象，就无法更改。`String`类有许多方法，其中一些将在下面讨论，看起来修改了字符串。由于字符串是不可变的，这些方法实际上是创建并返回一个包含操作结果的新字符串。

* * *

## 字符串长度

用于获取有关对象信息的方法称为*访问器方法*。您可以与字符串一起使用的一个访问器方法是`length()`方法，它返回字符串对象中包含的字符数。执行以下两行代码后，`len`等于 17：

```java
String palindrome = "Dot saw I was Tod";
int len = palindrome.length();

```

*回文*是一个对称的单词或句子——它正向和反向拼写相同，忽略大小写和标点符号。这是一个简短且低效的程序来反转一个回文字符串。它调用了`String`方法`charAt(i)`，该方法返回字符串中第 i 个字符，从 0 开始计数。

```java

public class StringDemo {
    public static void main(String[] args) {
        String palindrome = "Dot saw I was Tod";
        int len = palindrome.length();
        char[] tempCharArray = new char[len];
        char[] charArray = new char[len];

        // put original string in an 
        // array of chars
        for (int i = 0; i < len; i++) {
            tempCharArray[i] = 
                palindrome.charAt(i);
        } 

        // reverse array of chars
        for (int j = 0; j < len; j++) {
            charArray[j] =
                tempCharArray[len - 1 - j];
        }

        String reversePalindrome =
            new String(charArray);
        System.out.println(reversePalindrome);
    }
}

```

运行程序会产生以下输出：

```java
doT saw I was toD

```

为了实现字符串反转，程序必须将字符串转换为字符数组（第一个`for`循环），将数组反转为第二个数组（第二个`for`循环），然后再转换回字符串。[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)类包括一个方法，`getChars()`，用于将字符串或字符串的一部分转换为字符数组，以便我们可以用以下代码替换上面程序中的第一个`for`循环

```java
palindrome.getChars(0, len, tempCharArray, 0);

```

## 字符串连接

`String`类包括一个用于连接两个字符串的方法：

```java
string1.concat(string2); 

```

这将返回一个新字符串，其中在字符串 1 的末尾添加了字符串 2。

您还可以使用`concat()`方法与字符串字面值，如：

```java
"My name is ".concat("Rumplestiltskin");

```

字符串更常用地使用`+`运算符连接，如

```java
"Hello," + " world" + "!"

```

这导致

```java
"Hello, world!"

```

`+`运算符广泛用于`print`语句中。例如：

```java
String string1 = "saw I was ";
System.out.println("Dot " + string1 + "Tod");

```

打印

```java
Dot saw I was Tod

```

这样的连接可以是任何对象的混合。对于每个不是`String`的对象，都会调用它的`toString()`方法将其转换为`String`。

* * *

**注意：** Java 编程语言不允许文字字符串跨越多行出现在源文件中，因此您必须在多行字符串的每一行末尾使用`+`连接运算符。例如：

```java
String quote = 
    "Now is the time for all good " +
    "men to come to the aid of their country.";

```

使用`+`连接运算符在行之间断开字符串，在`print`语句中再次非常常见。

* * *

## 创建格式化字符串

您已经看到了使用`printf()`和`format()`方法打印带有格式化数字的输出。`String`类有一个等效的类方法`format()`，它返回一个`String`对象而不是一个`PrintStream`对象。

使用`String`的静态`format()`方法允许您创建一个格式化的字符串，您可以重复使用，而不是一次性的打印语句。例如，而不是

```java
System.out.printf("The value of the float " +
                  "variable is %f, while " +
                  "the value of the " + 
                  "integer variable is %d, " +
                  "and the string is %s", 
                  floatVar, intVar, stringVar); 

```

你可以写

```java
String fs;
fs = String.format("The value of the float " +
                   "variable is %f, while " +
                   "the value of the " + 
                   "integer variable is %d, " +
                   " and the string is %s",
                   floatVar, intVar, stringVar);
System.out.println(fs);

```
