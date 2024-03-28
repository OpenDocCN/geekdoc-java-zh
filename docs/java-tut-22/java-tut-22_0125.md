# 上界通配符

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/upperBounded.html`](https://docs.oracle.com/javase/tutorial/java/generics/upperBounded.html)

您可以使用上界通配符来放宽对变量的限制。例如，假设您想编写一个适用于`List<Integer>`、`List<Double>`和`List<Number>`的方法；您可以通过使用上界通配符来实现这一点。

要声明上界通配符，请使用通配符字符（'`?`'），后跟`extends`关键字，再跟其*上界*。请注意，在此上下文中，`extends`的含义是广义上的，既可以表示"extends"（如类）也可以表示"implements"（如接口）。

要编写适用于`Number`及其子类型（如`Integer`、`Double`和`Float`）的列表的方法，您应指定`List<? extends Number>`。术语`List<Number>`比`List<? extends Number>`更为严格，因为前者仅匹配类型为`Number`的列表，而后者匹配类型为`Number`或其任何子类的列表。

考虑以下`process`方法：

```java
public static void process(List<? extends Foo> list) { /* ... */ }

```

上界通配符`<? extends Foo>`，其中`Foo`是任何类型，匹配`Foo`和`Foo`的任何子类型。`process`方法可以将列表元素作为类型`Foo`访问：

```java
public static void process(List<? extends Foo> list) {
    for (Foo elem : list) {
        // ...
    }
}

```

在`foreach`子句中，`elem`变量遍历列表中的每个元素。现在可以在`elem`上使用`Foo`类中定义的任何方法。

`sumOfList`方法返回列表中数字的总和：

```java
public static double sumOfList(List<? extends Number> list) {
    double s = 0.0;
    for (Number n : list)
        s += n.doubleValue();
    return s;
}

```

使用`Integer`对象列表的以下代码打印`sum = 6.0`：

```java
List<Integer> li = Arrays.asList(1, 2, 3);
System.out.println("sum = " + sumOfList(li));

```

一组`Double`值可以使用相同的`sumOfList`方法。以下代码打印`sum = 7.0`：

```java
List<Double> ld = Arrays.asList(1.2, 2.3, 3.5);
System.out.println("sum = " + sumOfList(ld));

```
