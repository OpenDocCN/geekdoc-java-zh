# 非可实例化类型

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/nonReifiableVarargsType.html`](https://docs.oracle.com/javase/tutorial/java/generics/nonReifiableVarargsType.html)

章节 类型擦除 讨论了编译器删除与类型参数和类型参数相关的信息的过程。类型擦除对于具有非可实例化类型的可变参数（也称为 *varargs*）方法有相关后果。有关可变参数方法的更多信息，请参见 传递信息给方法或构造函数 中的 任意数量的参数 章节。

本页涵盖以下主题：

+   非可实例化类型

+   堆污染

+   具有非可实例化形式参数的可变参数方法的潜在漏洞

+   防止具有非可实例化形式参数的可变参数方法产生警告

## 非可实例化类型

*可实例化* 类型是一种在运行时完全可用的类型信息的类型。这包括原始类型、非泛型类型、原始类型和未绑定通配符的调用。

*非可实例化类型* 是在编译时通过类型擦除删除了信息的类型 —— 未定义为未限定通配符的泛型类型的调用。非可实例化类型在运行时不具备所有信息。非可实例化类型的示例包括 `List<String>` 和 `List<Number>`；JVM 无法在运行时区分这些类型。如 泛型的限制 所示，有一些情况下不能使用非可实例化类型：例如，在 `instanceof` 表达式中，或作为数组中的元素。

## 堆污染

*堆污染* 发生在参数化类型的变量引用不是该参数化类型的对象时。如果程序执行了一些操作导致在编译时产生未经检查的警告，则会出现这种情况。如果在编译时（在编译时类型检查规则的限制范围内）或在运行时无法验证涉及参数化类型的操作的正确性（例如，强制转换或方法调用），则会生成 *未经检查的警告*。例如，当混合使用原始类型和参数化类型，或执行未经检查的强制转换时，就会发生堆污染。

在正常情况下，当所有代码同时编译时，编译器会发出未经检查的警告，以引起您对潜在的堆污染的注意。如果您分别编译代码的各个部分，很难检测到堆污染的潜在风险。如果确保您的代码在没有警告的情况下编译通过，那么就不会发生堆污染。

## 具有非可实例化形式参数的可变参数方法的潜在漏洞

包含可变参数输入参数的泛型方法可能导致堆污染。

考虑以下`ArrayBuilder`类：

```java
public class ArrayBuilder {

  public static <T> void addToList (List<T> listArg, T... elements) {
    for (T x : elements) {
      listArg.add(x);
    }
  }

  public static void faultyMethod(List<String>... l) {
    Object[] objectArray = l;     // Valid
    objectArray[0] = Arrays.asList(42);
    String s = l[0].get(0);       // ClassCastException thrown here
  }

}

```

以下示例`HeapPollutionExample`使用了`ArrayBuiler`类：

```java
public class HeapPollutionExample {

  public static void main(String[] args) {

    List<String> stringListA = new ArrayList<String>();
    List<String> stringListB = new ArrayList<String>();

    ArrayBuilder.addToList(stringListA, "Seven", "Eight", "Nine");
    ArrayBuilder.addToList(stringListB, "Ten", "Eleven", "Twelve");
    List<List<String>> listOfStringLists =
      new ArrayList<List<String>>();
    ArrayBuilder.addToList(listOfStringLists,
      stringListA, stringListB);

    ArrayBuilder.faultyMethod(Arrays.asList("Hello!"), Arrays.asList("World!"));
  }
}

```

编译时，`ArrayBuilder.addToList`方法的定义会产生以下警告：

```java
warning: [varargs] Possible heap pollution from parameterized vararg type T

```

当编译器遇到可变参数方法时，它将可变参数形式参数转换为数组。然而，Java 编程语言不允许创建参数化类型的数组。在方法`ArrayBuilder.addToList`中，编译器将可变参数形式参数`T... elements`转换为形式参数`T[] elements`，一个数组。然而，由于类型擦除，编译器将可变参数形式参数转换为`Object[] elements`。因此，存在堆污染的可能性。

以下语句将可变参数形式参数`l`赋给`Object`数组`objectArgs`：

```java
Object[] objectArray = l;

```

这个语句可能会引入堆污染。一个与可变参数形式参数`l`的参数化类型不匹配的值可以赋给变量`objectArray`，从而可以赋给`l`。然而，在这个语句中，编译器并不生成未经检查的警告。编译器在将可变参数形式参数`List<String>... l`翻译为形式参数`List[] l`时已经生成了警告。这个语句是有效的；变量`l`的类型是`List[]`，它是`Object[]`的子类型。

因此，如果您将任何类型的`List`对象分配给`objectArray`数组的任何数组组件，编译器不会发出警告或错误，如下所示：

```java
objectArray[0] = Arrays.asList(42);

```

这个语句将包含一个类型为`Integer`的对象的`List`对象分配给`objectArray`数组的第一个数组组件。

假设您使用以下语句调用`ArrayBuilder.faultyMethod`：

```java
ArrayBuilder.faultyMethod(Arrays.asList("Hello!"), Arrays.asList("World!"));

```

在运行时，JVM 在以下语句处抛出`ClassCastException`：

```java
// ClassCastException thrown here
String s = l[0].get(0);

```

存储在变量`l`的第一个数组组件中的对象的类型是`List<Integer>`，但这个语句期望的是类型为`List<String>`的对象。

## 防止具有非可重复形式参数的可变参数方法产生警告

如果您声明一个具有参数化类型参数的可变参数方法，并确保方法体不会因为对可变参数形式参数的不当处理而抛出`ClassCastException`或其他类似异常，您可以通过在静态和非构造方法声明中添加以下注解来防止编译器为这些类型的可变参数方法生成警告：

```java
@SafeVarargs

```

`@SafeVarargs`注解是方法契约的一部分；此注解断言方法的实现不会不当处理可变参数形式参数。

也可以通过在方法声明中添加以下内容来抑制此类警告，尽管这种做法不太理想：

```java
@SuppressWarnings({"unchecked", "varargs"})

```

然而，这种方法并不会抑制从方法调用点生成的警告。如果你对`@SuppressWarnings`语法不熟悉，请参见 Annotations。
