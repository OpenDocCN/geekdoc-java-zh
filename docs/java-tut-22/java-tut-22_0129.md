# 通配符捕获和辅助方法

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/capture.html`](https://docs.oracle.com/javase/tutorial/java/generics/capture.html)

在某些情况下，编译器会推断通配符的类型。例如，一个列表可能被定义为`List<?>`，但在评估表达式时，编译器会从代码中推断出特定的类型。这种情况被称为*通配符捕获*。

对于大多数情况，你不需要担心通配符捕获，除非你看到一个包含短语“capture of”的错误消息。

当编译时，`WildcardError`示例会产生一个捕获错误：

```java
import java.util.List;

public class WildcardError {

    void foo(List<?> i) {
        i.set(0, i.get(0));
    }
}

```

在这个例子中，编译器将`i`输入参数处理为`Object`类型。当`foo`方法调用[List.set(int, E)](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#set-int-E-)时，编译器无法确认要插入列表的对象类型，从而产生错误。当出现这种类型的错误时，通常意味着编译器认为你正在将错误的类型赋给变量。泛型被添加到 Java 语言中的原因就是为了在编译时强制执行类型安全。

`WildcardError`示例在 Oracle 的 JDK 7 `javac`实现编译时生成以下错误：

```java
WildcardError.java:6: error: method set in interface List<E> cannot be applied to given types;
    i.set(0, i.get(0));
     ^
  required: int,CAP#1
  found: int,Object
  reason: actual argument Object cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Object from capture of ?
1 error

```

在这个例子中，代码正在尝试执行一个安全的操作，那么你如何解决编译器错误呢？你可以通过编写一个*私有辅助方法*来修复它，该方法捕获通配符。在这种情况下，你可以通过创建私有辅助方法`fooHelper`来解决问题，如`WildcardFixed`中所示：

```java
public class WildcardFixed {

    void foo(List<?> i) {
        fooHelper(i);
    }

    // Helper method created so that the wildcard can be captured
    // through type inference.
    private <T> void fooHelper(List<T> l) {
        l.set(0, l.get(0));
    }

}

```

多亏了辅助方法，编译器使用推断确定`T`是`CAP#1`，即捕获变量，在调用中。现在的示例成功编译。

按照惯例，辅助方法通常被命名为`*originalMethodName*Helper`。

现在考虑一个更复杂的例子，`WildcardErrorBad`：

```java
import java.util.List;

public class WildcardErrorBad {

    void swapFirst(List<? extends Number> l1, List<? extends Number> l2) {
      Number temp = l1.get(0);
      l1.set(0, l2.get(0)); // expected a CAP#1 extends Number,
                            // got a CAP#2 extends Number;
                            // same bound, but different types
      l2.set(0, temp);	    // expected a CAP#1 extends Number,
                            // got a Number
    }
}

```

在这个例子中，代码正在尝试一个不安全的操作。例如，考虑`swapFirst`方法的以下调用：

```java
List<Integer> li = Arrays.asList(1, 2, 3);
List<Double>  ld = Arrays.asList(10.10, 20.20, 30.30);
swapFirst(li, ld);

```

虽然`List<Integer>`和`List<Double>`都满足`List<? extends Number>`的条件，但从`Integer`值列表中取一个项目并尝试将其放入`Double`值列表中显然是不正确的。

使用 Oracle 的 JDK `javac`编译器编译代码会产生以下错误：

```java
WildcardErrorBad.java:7: error: method set in interface List<E> cannot be applied to given types;
      l1.set(0, l2.get(0)); // expected a CAP#1 extends Number,
        ^
  required: int,CAP#1
  found: int,Number
  reason: actual argument Number cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Number from capture of ? extends Number
WildcardErrorBad.java:10: error: method set in interface List<E> cannot be applied to given types;
      l2.set(0, temp);      // expected a CAP#1 extends Number,
        ^
  required: int,CAP#1
  found: int,Number
  reason: actual argument Number cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Number from capture of ? extends Number
WildcardErrorBad.java:15: error: method set in interface List<E> cannot be applied to given types;
        i.set(0, i.get(0));
         ^
  required: int,CAP#1
  found: int,Object
  reason: actual argument Object cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Object from capture of ?
3 errors

```

没有辅助方法可以解决这个问题，因为代码本质上是错误的：从`Integer`值列表中取一个项目并尝试将其放入`Double`值列表中显然是不正确的。
