# 格式化数字打印输出

> 原文：[`docs.oracle.com/javase/tutorial/java/data/numberformat.html`](https://docs.oracle.com/javase/tutorial/java/data/numberformat.html)

之前你看到了使用`print`和`println`方法将字符串打印到标准输出（`System.out`）的示例。由于所有数字都可以转换为字符串（稍后将会看到），你可以使用这些方法打印任意混合的字符串和数字。然而，Java 编程语言还有其他方法，允许你在包含数字时更好地控制打印输出。

## `printf`和`format`方法

`java.io`包中包含一个`PrintStream`类，其中有两个格式化方法可用于替换`print`和`println`。这些方法，`format`和`printf`，是等效的。你一直在使用的熟悉的`System.out`恰好是一个`PrintStream`对象，因此你可以在`System.out`上调用`PrintStream`方法。因此，在你以前使用`print`或`println`的代码中，你可以使用`format`或`printf`。例如，

```java
System.out.format(.....);

```

这两个[`java.io.PrintStream`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html)方法的语法是相同的：

```java
public PrintStream format(String format, Object... args)

```

其中`format`是一个指定要使用的格式化的字符串，`args`是要使用该格式化打印的变量列表。一个简单的示例可能是

```java
System.out.format("The value of " + "the float variable is " +
     "%f, while the value of the " + "integer variable is %d, " +
     "and the string is %s", floatVar, intVar, stringVar); 

```

第一个参数，`format`，是一个格式字符串，指定了第二个参数`args`中的对象如何被格式化。格式字符串包含普通文本以及*格式说明符*，这些是特殊字符，用于格式化`Object... args`的参数。（`Object... args`的表示法称为*可变参数*，意味着参数的数量可能变化。）

格式说明符以百分号（%）开始，并以*转换器*结束。转换器是指示要格式化的参数类型的字符。在百分号（%）和转换器之间，你可以有可选的标志和说明符。有许多转换器、标志和说明符，这些都在[`java.util.Formatter`](https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html)中有文档记录。

这里是一个基本示例：

```java
int i = 461012;
System.out.format("The value of i is: %d%n", i);

```

`%d`指定单个变量为十进制整数。`%n`是一个与平台无关的换行符。输出为：

```java
The value of i is: 461012

```

`printf`和`format`方法是重载的。每个都有以下语法版本：

```java
public PrintStream format(Locale l, String format, Object... args)

```

例如，要在法国系统中打印数字（在英文浮点数表示中使用逗号代替小数点），你可以使用：

```java
System.out.format(Locale.FRANCE,
    "The value of the float " + "variable is %f, while the " +
    "value of the integer variable " + "is %d, and the string is %s%n", 
    floatVar, intVar, stringVar); 

```

## 一个示例

下表列出了在接下来的示例程序`TestFormat.java`中使用的一些转换器和标志。

在`TestFormat.java`中使用的转换器和标志

| 转换器 | 标志 | 说明 |
| --- | --- | --- |
| d |   | 十进制整数。 |
| f |   | 一个浮点数。 |
| n |   | 适用于运行应用程序的平台的换行符。应始终使用`%n`，而不是`\n`。 |
| tB |   | 日期和时间转换月份的区域设置全名。 |
| td, te |   | 日期和时间转换月份中的 2 位数字。td 根据需要带有前导零，te 则不带。 |
| ty, tY |   | 日期和时间转换ty = 2 位年份，tY = 4 位年份。 |
| tl |   | 日期和时间转换12 小时制钟表中的小时。 |
| tM |   | 日期和时间转换以 2 位数字表示的分钟，必要时带有前导零。 |
| tp |   | 日期和时间转换区域设置特定的上午/下午（小写）。 |
| tm |   | 日期和时间转换月份以 2 位数字表示，必要时带有前导零。 |
| tD |   | 日期和时间转换日期为 %tm%td%ty |
|   | 08 | 宽度为 8 个字符，必要时带有前导零。 |
|   | + | 包括符号，无论是正数还是负数。 |
|   | , | 包括特定于区域设置的分组字符。 |
|   | - | 左对齐。 |
|   | .3 | 小数点后三位。 |
|   | 10.3 | 宽度为 10 个字符，右对齐，小数点后三位。 |

以下程序展示了您可以使用`format`进行的一些格式化。输出在嵌入式注释中用双引号显示：

```java
import java.util.Calendar;
import java.util.Locale;

public class TestFormat {

    public static void main(String[] args) {
      long n = 461012;
      System.out.format("%d%n", n);      //  -->  "461012"
      System.out.format("%08d%n", n);    //  -->  "00461012"
      System.out.format("%+8d%n", n);    //  -->  " +461012"
      System.out.format("%,8d%n", n);    // -->  " 461,012"
      System.out.format("%+,8d%n%n", n); //  -->  "+461,012"

      double pi = Math.PI;

      System.out.format("%f%n", pi);       // -->  "3.141593"
      System.out.format("%.3f%n", pi);     // -->  "3.142"
      System.out.format("%10.3f%n", pi);   // -->  "     3.142"
      System.out.format("%-10.3f%n", pi);  // -->  "3.142"
      System.out.format(Locale.FRANCE,
                        "%-10.4f%n%n", pi); // -->  "3,1416"

      Calendar c = Calendar.getInstance();
      System.out.format("%tB %te, %tY%n", c, c, c); // -->  "May 29, 2006"

      System.out.format("%tl:%tM %tp%n", c, c, c);  // -->  "2:34 am"

      System.out.format("%tD%n", c);    // -->  "05/29/06"
    }
}

```

* * *

**注意：** 本节讨论仅涵盖了`format`和`printf`方法的基础知识。更详细的信息可以在基础教程的`基本 I/O`部分的“格式化”页面中找到。

使用`String.format`创建字符串的方法在字符串中有介绍。

* * *

## DecimalFormat 类

您可以使用[`java.text.DecimalFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/DecimalFormat.html)类来控制前导和尾随零、前缀和后缀、分组（千位）分隔符以及小数点分隔符的显示。`DecimalFormat`在数字格式化方面提供了很大的灵活性，但可能会使您的代码变得更加复杂。

接下来的示例创建了一个`DecimalFormat`对象`myFormatter`，通过将模式字符串传递给`DecimalFormat`构造函数来创建。然后通过`myFormatter`调用从`NumberFormat`继承的`format()`方法它接受一个`double`值作为参数，并以字符串形式返回格式化的数字：

这是一个演示`DecimalFormat`使用的示例程序：

```java

import java.text.*;

public class DecimalFormatDemo {

   static public void customFormat(String pattern, double value ) {
      DecimalFormat myFormatter = new DecimalFormat(pattern);
      String output = myFormatter.format(value);
      System.out.println(value + "  " + pattern + "  " + output);
   }

   static public void main(String[] args) {

      customFormat("###,###.###", 123456.789);
      customFormat("###.##", 123456.789);
      customFormat("000000.000", 123.78);
      customFormat("$###,###.###", 12345.67);  
   }
}

```

输出为：

```java
123456.789  ###,###.###  123,456.789
123456.789  ###.##  123456.79
123.78  000000.000  000123.780
12345.67  $###,###.###  $12,345.67

```

以下表格解释了每行输出。

`DecimalFormat.java` 输出

| 值 | 模式 | 输出 | 解释 |
| --- | --- | --- | --- |
| 123456.789 | ###,###.### | 123,456.789 | 井号（#）表示数字，逗号是分组分隔符的占位符，句点是小数分隔符的占位符。 |
| 123456.789 | ###.## | 123456.79 | `value`小数点右侧有三位数字，但`pattern`只有两位。`format`方法通过四舍五入处理这个问题。 |
| 123.78 | 000000.000 | 000123.780 | `pattern`指定了前导和尾随零，因为使用的是 0 字符而不是井号(#)。 |
| 12345.67 | $###,###.### | $12,345.67 | `pattern`中的第一个字符是美元符号($)。请注意，它紧跟在格式化`output`中最左边的数字之前。 |
