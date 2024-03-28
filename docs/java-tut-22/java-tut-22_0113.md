# 自动装箱和拆箱

> 原文：[`docs.oracle.com/javase/tutorial/java/data/autoboxing.html`](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)

*自动装箱*是 Java 编译器在原始类型和其对应的对象包装类之间进行的自动转换。例如，将`int`转换为`Integer`，将`double`转换为`Double`等。如果转换反向进行，则称为*拆箱*。

这是自动装箱的最简单示例：

```java
Character ch = 'a';

```

本节中的其余示例使用泛型。如果您还不熟悉泛型的语法，请参阅泛型（更新）课程。

考虑以下代码：

```java
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i += 2)
    li.add(i);

```

尽管您将`int`值作为原始类型而不是`Integer`对象添加到`li`中，但代码仍然可以编译。因为`li`是`Integer`对象的列表，而不是`int`值的列表，您可能会想知道为什么 Java 编译器没有发出编译时错误。编译器不会生成错误，因为它从`i`创建一个`Integer`对象并将该对象添加到`li`中。因此，编译器在运行时将前面的代码转换为以下代码：

```java
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i += 2)
    li.add(Integer.valueOf(i));

```

将原始值（例如`int`）转换为相应包装类（`Integer`）的对象称为自动装箱。当原始值是以下情况时，Java 编译器会应用自动装箱：

+   作为传递给期望相应包装类对象的方法的参数。

+   赋给相应包装类的变量。

考虑以下方法：

```java
public static int sumEven(List<Integer> li) {
    int sum = 0;
    for (Integer i: li)
        if (i % 2 == 0)
            sum += i;
        return sum;
}

```

因为余数（`%`）和一元加号（`+=`）运算符不适用于`Integer`对象，您可能会想知道为什么 Java 编译器在不发出任何错误的情况下编译该方法。编译器不会生成错误，因为它在运行时调用`intValue`方法将`Integer`转换为`int`：

```java
public static int sumEven(List<Integer> li) {
    int sum = 0;
    for (Integer i : li)
        if (i.intValue() % 2 == 0)
            sum += i.intValue();
        return sum;
}

```

将包装类型的对象（`Integer`）转换为其对应的原始类型（`int`）值称为拆箱。当包装类的对象是以下情况时，Java 编译器会应用拆箱：

+   作为传递给期望相应原始类型值的方法的参数。

+   赋给相应原始类型的变量。

``Unboxing``示例展示了这是如何工作的：

```java
import java.util.ArrayList;
import java.util.List;

public class Unboxing {

    public static void main(String[] args) {
        Integer i = new Integer(-8);

        // 1\. Unboxing through method invocation
        int absVal = absoluteValue(i);
        System.out.println("absolute value of " + i + " = " + absVal);

        List<Double> ld = new ArrayList<>();
        ld.add(3.1416);    // Π is autoboxed through method invocation.

        // 2\. Unboxing through assignment
        double pi = ld.get(0);
        System.out.println("pi = " + pi);
    }

    public static int absoluteValue(int i) {
        return (i < 0) ? -i : i;
    }
}

```

该程序打印如下内容：

```java
absolute value of -8 = 8
pi = 3.1416

```

自动装箱和拆箱使开发人员编写更清晰的代码，使其更易于阅读。以下表列出了原始类型及其对应的包装类，这些包装类由 Java 编译器用于自动装箱和拆箱：

| Primitive type | Wrapper class |
| --- | --- |
| boolean | Boolean |
| byte | Byte |
| char | Character |
| float | Float |
| int | Integer |
| long | Long |
| short | Short |
| double | Double |
