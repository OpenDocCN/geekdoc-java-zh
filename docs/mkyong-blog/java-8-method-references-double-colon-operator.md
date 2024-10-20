# Java 8 方法引用，双冒号(::)运算符

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-method-references-double-colon-operator/>

在 Java 8 中，双冒号(::)操作符被称为方法引用。参考下面的例子:

打印列表的匿名类。

```java
 List<String> list = Arrays.asList("node", "java", "python", "ruby");
list.forEach(new Consumer<String>() {       // anonymous class
    @Override
    public void accept(String str) {
        System.out.println(str);
    }
}); 
```

匿名类-> Lambda 表达式。

```java
 List<String> list = Arrays.asList("node", "java", "python", "ruby");
list.forEach(str -> System.out.println(str)); // lambda 
```

Lambda 表达式->方法引用。

```java
 List<String> list = Arrays.asList("node", "java", "python", "ruby");
list.forEach(System.out::println);          // method references 
```

**匿名类->λ表达式- >方法引用**

**注意**
lambda 表达式或方法引用什么都不做，只是对现有方法的另一种方式调用。通过方法引用，它获得了更好的可读性。

有四种方法引用:

*   对静态方法的引用`ClassName::staticMethodName`
*   引用特定对象的实例方法`Object::instanceMethodName`
*   引用特定类型的任意对象的实例方法`ContainingType::methodName`–
*   对构造函数的引用`ClassName::new`

## 1.静态法

λ表达式。

```java
 (args) -> ClassName.staticMethodName(args) 
```

方法参考。

```java
 ClassName::staticMethodName 
```

1.1 这个例子打印一个字符串列表，方法引用一个静态方法`SimplePrinter::print`。

Java8MethodReference1a.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class Java8MethodReference1a {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("A", "B", "C");

        // anonymous class
        list.forEach(new Consumer<String>() {
            @Override
            public void accept(String x) {
                SimplePrinter.print(x);
            }
        });

        // lambda expression
        list.forEach(x -> SimplePrinter.print(x));

        // method reference
        list.forEach(SimplePrinter::print);

    }

}

class SimplePrinter {
    public static void print(String str) {
        System.out.println(str);
    }
} 
```

1.2 这个例子将一个字符串列表转换成一个整数列表，方法引用一个静态方法`Integer::parseInt`。

Integer.java

```java
 public static int parseInt(String s) throws NumberFormatException {
        return parseInt(s,10);
  } 
```

Java8MethodReference1b.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.List;
import java.util.function.Function;
import java.util.stream.Collectors;

public class Java8MethodReference1b {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("1", "2", "3");

        // anonymous class
        List<Integer> collect1 = list.stream()
                .map(new Function<String, Integer>() {
                    @Override
                    public Integer apply(String s) {
                        return Integer.parseInt(s);
                    }
                })
                .collect(Collectors.toList());

        // lambda expression
        List<Integer> collect2 = list.stream()
                .map(s -> Integer.parseInt(s))
                .collect(Collectors.toList());

        // method reference
        List<Integer> collect3 = list.stream()
                .map(Integer::parseInt)
                .collect(Collectors.toList());

    }

} 
```

1.3 此示例连接两个`Integer`并返回一个`String`。它将方法引用静态方法`IntegerUtils::join`作为参数传递给另一个接受`BiFunction`的方法。

Java8MethodReference1c.java

```java
 package com.mkyong;

import java.util.function.BiFunction;

public class Java8MethodReference1c {

    public static void main(String[] args) {

        // anonymous class
        String result1 = playTwoArgument(1, 2, new BiFunction<Integer, Integer, String>() {
                  @Override
                  public String apply(Integer a, Integer b) {
                      return IntegerUtils.join(a, b);
                  }
              });                                                                   // 3

        // lambda
        String result1 = playTwoArgument(1, 2, (a, b) -> IntegerUtils.join(a, b));  // 3

        // method reference
        String result2 = playTwoArgument(1, 2, IntegerUtils::join);                 // 3

    }

    private static <R> R playTwoArgument(Integer i1, Integer i2,
        BiFunction<Integer, Integer, R> func) {
        return func.apply(i1, i2);
    }

}

class IntegerUtils{

    public static String join(Integer a, Integer b) {
        return String.valueOf(a + b);
    }

} 
```

## 2.对特定对象的实例方法的引用

λ表达式。

```java
 (args) -> object.instanceMethodName(args) 
```

方法参考。

```java
 object::instanceMethodName 
```

2.1 这个例子按照薪水对一个`Employee`列表进行排序。我们可以引用一个特定对象`ComparatorProvider`的实例方法`compareBySalary`。

Java8MethodReference2

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.List;

public class Java8MethodReference2 {

    public static void main(String[] args) {

        List<Employee> list = Arrays.asList(
                new Employee("mkyong", 38, BigDecimal.valueOf(3800)),
                new Employee("zilap", 5, BigDecimal.valueOf(100)),
                new Employee("ali", 25, BigDecimal.valueOf(2500)),
                new Employee("unknown", 99, BigDecimal.valueOf(9999)));

        // anonymous class
        /*list.sort(new Comparator<Employee>() {
            @Override
            public int compare(Employee o1, Employee o2) {
                return provider.compareBySalary(o1, o2);
            }
        });*/

        ComparatorProvider provider = new ComparatorProvider();

        // lambda
        // list.sort((o1, o2) -> provider.compareBySalary(o1, o2));

        // method reference
        list.sort(provider::compareBySalary);

        list.forEach(x -> System.out.println(x));

    }

}

class ComparatorProvider {

    public int compareByAge(Employee o1, Employee o2) {
        return o1.getAge().compareTo(o2.getAge());
    }

    public int compareByName(Employee o1, Employee o2) {
        return o1.getName().compareTo(o2.getName());
    }

    public int compareBySalary(Employee o1, Employee o2) {
        return o1.getAge().compareTo(o2.getAge());
    }

} 
```

Employee.java

```java
 package com.mkyong;

import java.math.BigDecimal;

public class Employee {

    String name;
    Integer age;
    BigDecimal salary;

    // generated by IDE, getters, setters, constructor, toString
} 
```

输出

```java
 Employee{name='zilap', age=5, salary=100}
Employee{name='ali', age=25, salary=2500}
Employee{name='mkyong', age=38, salary=3800}
Employee{name='unknown', age=99, salary=9999} 
```

## 3.对特定类型的任意对象的实例方法的引用。

这种说法有点混乱，不需要什么解释，请看下面的例子:

λ表达式。

```java
 // arg0 is the first argument
(arg0, rest_of_args) -> arg0.methodName(rest_of_args)

// example, assume a and b are String
(a, b) -> a.compareToIgnoreCase(b) 
```

方法参考。

```java
 // first argument type
arg0_Type::methodName

// arg0 is type of ClassName
ClassName::methodName

// example, a is type of String
String::compareToIgnoreCase 
```

对于`(String a, String b)`，其中`a`和`b`为任意名称，`String`为其任意类型。这个例子使用了对特定类型`String`的任意对象`a`(第一个参数)的实例方法`compareToIgnoreCase`的方法引用。

3.1 查看本[方法参考](http://web.archive.org/web/20220801141236/https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html)中的正式示例

```java
 String[] stringArray = { "Barbara", "James", "Mary", "John",
                "Patricia", "Robert", "Michael", "Linda" };
  Arrays.sort(stringArray, String::compareToIgnoreCase); 
```

我们传递了一个方法引用`String::compareToIgnoreCase`作为`Arrays.sort`的比较器。

**讲解**
复习`Arrays.sort`方法签名:

```java
 public static <T> void sort(T[] a, Comparator<? super T> c) {
} 
```

在上面的例子中，`Arrays.sort`期望一个`Comparator<String>`。`Comparator`是一个函数接口，它的抽象方法`compare`匹配`BiFunction<String, String, Integer>`，它带两个参数`String`并返回一个`int`。

Comparator.java

```java
 @FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);  // this matches BiFunction<String, String, Integer>
} 
```

查看`BiFunction`方法签名:

BiFunction.java

```java
 @FunctionalInterface
public interface BiFunction<T, U, R> {
      R apply(T t, U u);
} 
```

**延伸阅读——**[Java 8 双函数示例](/web/20220801141236/https://mkyong.com/java8/java-8-bifunction-examples/)

below lambda 为`BiFunction<String,String,Integer>`提供了实现，因此`Arrays.sort`接受 below lambda 表达式作为有效语法。

```java
 (String a, String b) -> a.compareToIgnoreCase(b) // return int

// a is type of String
// method reference
String::compareToIgnoreCase 
```

3.2 让我们看另一个例子。

Java8MethodReference3a.java

```java
 package com.mkyong;

import java.util.function.BiPredicate;
import java.util.function.Function;

public class Java8MethodReference3a {

    public static void main(String[] args) {

        // lambda
        int result = playOneArgument("mkyong", x -> x.length());   // 6

        // method reference
        int result2 = playOneArgument("mkyong", String::length);   // 6

        // lambda
        Boolean result3 = playTwoArgument("mkyong", "y", (a, b) -> a.contains(b)); // true

        // method reference
        Boolean result4 = playTwoArgument("mkyong", "y", String::contains);        // true

        // lambda
        Boolean result5 = playTwoArgument("mkyong", "1", (a, b) -> a.startsWith(b)); // false

        // method reference
        Boolean result6 = playTwoArgument("mkyong", "y", String::startsWith);        // false

        System.out.println(result6);
    }

    static <R> R playOneArgument(String s1, Function<String, R> func) {
        return func.apply(s1);
    }

    static Boolean playTwoArgument(String s1, String s2, BiPredicate<String, String> func) {
        return func.test(s1, s2);
    }

} 
```

3.3 让我们看另一个例子，自定义对象。

Java8MethodReference3b.java

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.math.RoundingMode;
import java.util.function.BiFunction;

public class Java8MethodReference3b {

    public static void main(String[] args) {

        Invoice obj = new Invoice("A001", BigDecimal.valueOf(1.99), 3);

        InvoiceCalculator formula = new InvoiceCalculator();

        // lambda
        BigDecimal result = calculate(formula, obj, (f, o) -> f.normal(o));         // 5.97

        // method reference
        BigDecimal result2 = calculate(formula, obj, InvoiceCalculator::normal);    // 5.97

        // lambda
        BigDecimal result3 = calculate(formula, obj, (f, o) -> f.promotion(o));     // 5.37

        // method reference
        BigDecimal result4 = calculate(formula, obj, InvoiceCalculator::promotion); // 5.37

    }

    static BigDecimal calculate(InvoiceCalculator formula, Invoice s1,
                                BiFunction<InvoiceCalculator, Invoice, BigDecimal> func) {
        return func.apply(formula, s1);
    }

}

class InvoiceCalculator {

    public BigDecimal normal(Invoice obj) {
        return obj.getUnitPrice().multiply(BigDecimal.valueOf(obj.qty));
    }

    public BigDecimal promotion(Invoice obj) {
        return obj.getUnitPrice()
                .multiply(BigDecimal.valueOf(obj.qty))
                .multiply(BigDecimal.valueOf(0.9))
                .setScale(2, RoundingMode.HALF_UP);
    }
}

class Invoice {

    String no;
    BigDecimal unitPrice;
    Integer qty;

    // generated by IDE, setters, gettes, constructor, toString
} 
```

第一个参数是一种类型的`InvoiceCalculator`。因此，我们可以引用特定类型`InvoiceCalculator`的任意对象`f`的实例方法(`normal or promotion`)。

```java
 (f, o) -> f.normal(o))
(f, o) -> f.promotion(o))

InvoiceCalculator::normal
InvoiceCalculator::promotion 
```

明白了吗？没有更多的例子🙂

## 4.对构造函数的引用。

λ表达式。

```java
 (args) -> new ClassName(args) 
```

方法参考。

```java
 ClassName::new 
```

4.1 对默认构造函数的引用。

Java8MethodReference4a.java

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.util.HashMap;
import java.util.Map;
import java.util.function.Supplier;

public class Java8MethodReference4a {

    public static void main(String[] args) {

        // lambda
        Supplier<Map> obj1 = () -> new HashMap();   // default HashMap() constructor
        Map map1 = obj1.get();

        // method reference
        Supplier<Map> obj2 = HashMap::new;
        Map map2 = obj2.get();

        // lambda
        Supplier<Invoice> obj3 = () -> new Invoice(); // default Invoice() constructor
        Invoice invoice1 = obj3.get();

        // method reference
        Supplier<Invoice> obj4 = Invoice::new;
        Invoice invoice2 = obj4.get();

    }

}

class Invoice {

    String no;
    BigDecimal unitPrice;
    Integer qty;

    public Invoice() {
    }

    //... generated by IDE
} 
```

4.2 对接受参数的构造函数的引用—`Invoice(BigDecimal unitPrice)`

Java8MethodReference4b.java

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class Java8MethodReference4b {

    public static void main(String[] args) {

        List<BigDecimal> list = Arrays.asList(
                BigDecimal.valueOf(9.99),
                BigDecimal.valueOf(2.99),
                BigDecimal.valueOf(8.99));

        // lambda
        // List<Invoice> invoices = fakeInvoice(list, (price) -> new Invoice(price));

        // method reference
        List<Invoice> invoices = fakeInvoice(list, Invoice::new);

        invoices.forEach(System.out::println);
    }

    static List<Invoice> fakeInvoice(List<BigDecimal> list, Function<BigDecimal, Invoice> func) {
        List<Invoice> result = new ArrayList<>();

        for (BigDecimal amount : list) {
            result.add(func.apply(amount));
        }
        return result;
    }

}

class Invoice {

    String no;
    BigDecimal unitPrice;
    Integer qty;

    public Invoice(BigDecimal unitPrice) {
        this.unitPrice = unitPrice;
    }

    //... generated by IDE
} 
```

输出

```java
 Invoice{no='null', unitPrice=9.99, qty=null}
Invoice{no='null', unitPrice=2.99, qty=null}
Invoice{no='null', unitPrice=8.99, qty=null} 
```

完成了。

## 参考

*   [Java 8 方法参考:如何使用](http://web.archive.org/web/20220801141236/https://www.codementor.io/@eh3rrera/using-java-8-method-reference-du10866vx)
*   [了解 Java 8 方法引用](http://web.archive.org/web/20220801141236/http://moandjiezana.com/blog/2014/understanding-method-references/)
*   [λ表达式的翻译](http://web.archive.org/web/20220801141236/http://cr.openjdk.java.net/~briangoetz/lambda/lambda-translation.html)
*   [Java 教程–方法参考](http://web.archive.org/web/20220801141236/https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html)
*   [Java 8 教程](/web/20220801141236/https://mkyong.com/tutorials/java-8-tutorials/)
*   [Java 8 函数示例](/web/20220801141236/https://mkyong.com/java8/java-8-function-examples/)
*   [Java 8 双功能示例](/web/20220801141236/https://mkyong.com/java8/java-8-bifunction-examples/)
*   [Java 8 谓词示例](/web/20220801141236/https://mkyong.com/java8/java-8-predicate-examples/)
*   [Java 8 双预测示例](/web/20220801141236/https://mkyong.com/java8/java-8-bipredicate-examples/)
*   [Java 8 供应商示例](/web/20220801141236/https://mkyong.com/java8/java-8-supplier-examples/)

<input type="hidden" id="mkyong-current-postId" value="15538">