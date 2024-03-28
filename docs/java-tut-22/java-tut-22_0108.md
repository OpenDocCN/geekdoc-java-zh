# 在数字和字符串之间进行转换

> 原文：[`docs.oracle.com/javase/tutorial/java/data/converting.html`](https://docs.oracle.com/javase/tutorial/java/data/converting.html)

## 将字符串转换为数字

经常情况下，程序最终会在字符串对象中包含数值数据例如用户输入的值。

包装原始数值类型的`Number`子类（[`Byte`](https://docs.oracle.com/javase/8/docs/api/java/lang/Byte.html)、[`Integer`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html)、[`Double`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html)、[`Float`](https://docs.oracle.com/javase/8/docs/api/java/lang/Float.html)、[`Long`](https://docs.oracle.com/javase/8/docs/api/java/lang/Long.html)和[`Short`](https://docs.oracle.com/javase/8/docs/api/java/lang/Short.html)）每个都提供一个名为`valueOf`的类方法，将字符串转换为该类型的对象。以下是一个示例，`ValueOfDemo`，从命令行获取两个字符串，将它们转换为数字，并对这些值执行算术运算：

```java

public class ValueOfDemo {
    public static void main(String[] args) {

        // this program requires two 
        // arguments on the command line 
        if (args.length == 2) {
            // convert strings to numbers
            float a = (Float.valueOf(args[0])).floatValue(); 
            float b = (Float.valueOf(args[1])).floatValue();

            // do some arithmetic
            System.out.println("a + b = " +
                               (a + b));
            System.out.println("a - b = " +
                               (a - b));
            System.out.println("a * b = " +
                               (a * b));
            System.out.println("a / b = " +
                               (a / b));
            System.out.println("a % b = " +
                               (a % b));
        } else {
            System.out.println("This program " +
                "requires two command-line arguments.");
        }
    }
}

```

当您使用`4.5`和`87.2`作为命令行参数时，程序的输出如下：

```java
a + b = 91.7
a - b = -82.7
a * b = 392.4
a / b = 0.0516055
a % b = 4.5

```

* * *

**注意：**包装原始数值类型的每个`Number`子类还提供一个`parseXXXX()`方法（例如，`parseFloat()`），可用于将字符串转换为原始数值。由于返回的是原始类型而不是对象，因此`parseFloat()`方法比`valueOf()`方法更直接。例如，在`ValueOfDemo`程序中，我们可以使用：

```java
float a = Float.parseFloat(args[0]);
float b = Float.parseFloat(args[1]);

```

* * *

## 将数字转换为字符串

有时您需要将数字转换为字符串，因为您需要在其字符串形式上进行操作。有几种简单的方法可以将数字转换为字符串：

```java
int i;
// Concatenate "i" with an empty string; conversion is handled for you.
String s1 = "" + i;

```

或者

```java
// The valueOf class method.
String s2 = String.valueOf(i);

```

每个`Number`子类都包括一个类方法`toString()`，将其原始类型转换为字符串。例如：

```java
int i;
double d;
String s3 = Integer.toString(i); 
String s4 = Double.toString(d); 

```

`ToStringDemo`示例使用`toString`方法将数字转换为字符串。然后程序使用一些字符串方法来计算小数点前后的数字位数：

```java

public class ToStringDemo {

    public static void main(String[] args) {
        double d = 858.48;
        String s = Double.toString(d);

        int dot = s.indexOf('.');

        System.out.println(dot + " digits " +
            "before decimal point.");
        System.out.println( (s.length() - dot - 1) +
            " digits after decimal point.");
    }
}

```

该程序的输出为：

```java
3 digits before decimal point.
2 digits after decimal point.

```
