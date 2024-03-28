# 类型推断

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/genTypeInference.html`](https://docs.oracle.com/javase/tutorial/java/generics/genTypeInference.html)

*类型推断*是 Java 编译器根据每个方法调用和相应声明来确定使调用适用的类型参数（或参数）的能力。推断算法确定参数的类型，以及如果可用的话，结果被分配或返回的类型。最后，推断算法尝试找到适用于所有参数的*最具体*类型。

为了说明最后一点，在以下示例中，推断确定传递给`pick`方法的第二个参数的类型为`Serializable`：

```java
static <T> T pick(T a1, T a2) { return a2; }
Serializable s = pick("d", new ArrayList<String>());

```

## 类型推断和泛型方法

泛型方法向您介绍了类型推断，使您能够调用泛型方法，就像调用普通方法一样，而无需在尖括号之间指定类型。考虑以下示例，`BoxDemo`，它需要`Box`类：

```java
public class BoxDemo {

  public static <U> void addBox(U u, 
      java.util.List<Box<U>> boxes) {
    Box<U> box = new Box<>();
    box.set(u);
    boxes.add(box);
  }

  public static <U> void outputBoxes(java.util.List<Box<U>> boxes) {
    int counter = 0;
    for (Box<U> box: boxes) {
      U boxContents = box.get();
      System.out.println("Box #" + counter + " contains [" +
             boxContents.toString() + "]");
      counter++;
    }
  }

  public static void main(String[] args) {
    java.util.ArrayList<Box<Integer>> listOfIntegerBoxes =
      new java.util.ArrayList<>();
    BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(30), listOfIntegerBoxes);
    BoxDemo.outputBoxes(listOfIntegerBoxes);
  }
}

```

以下是此示例的输出：

```java
Box #0 contains [10]
Box #1 contains [20]
Box #2 contains [30]

```

泛型方法`addBox`定义了一个名为`U`的类型参数。通常，Java 编译器可以推断泛型方法调用的类型参数。因此，在大多数情况下，您不必指定它们。例如，要调用泛型方法`addBox`，您可以使用*类型见证*指定类型参数如下：

```java
BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);

```

另外，如果省略类型见证，Java 编译器会自动推断（从方法的参数中）类型参数为`Integer`：

```java
BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);

```

## 类型推断和泛型类的实例化

只要编译器可以从上下文中推断出类型参数，您可以用一组空类型参数（`<>`）替换调用泛型类构造函数所需的类型参数。这一对尖括号非正式地称为菱形。

例如，考虑以下变量声明：

```java
Map<String, List<String>> myMap = new HashMap<String, List<String>>();

```

您可以用一组空类型参数（`<>`）替换构造函数的参数化类型：

```java
Map<String, List<String>> myMap = new HashMap<>();

```

请注意，在泛型类实例化期间利用类型推断，必须使用菱形。在以下示例中，编译器生成了未经检查的转换警告，因为`HashMap()`构造函数引用了`HashMap`原始类型，而不是`Map<String, List<String>>`类型：

```java
Map<String, List<String>> myMap = new HashMap(); // unchecked conversion warning

```

## 泛型和非泛型类的泛型构造函数的类型推断

请注意，构造函数可以是泛型的（换句话说，在泛型和非泛型类中声明自己的形式类型参数）。考虑以下示例：

```java
class MyClass<X> {
  <T> MyClass(T t) {
    // ...
  }
}

```

考虑类`MyClass`的以下实例化：

```java
new MyClass<Integer>("")

```

这个语句创建了参数化类型`MyClass<Integer>`的实例；语句明确为泛型类`MyClass<X>`的形式类型参数`X`指定了类型`Integer`。请注意，这个泛型类的构造函数包含一个形式类型参数`T`。编译器为这个泛型类的构造函数的形式类型参数`T`推断了类型`String`（因为这个构造函数的实际参数是一个`String`对象）。

Java SE 7 之前的编译器能够推断泛型构造函数的实际类型参数，类似于泛型方法。然而，在 Java SE 7 及更高版本中，如果使用菱形（`<>`），编译器可以推断正在实例化的泛型类的实际类型参数。考虑以下例子：

```java
MyClass<Integer> myObject = new MyClass<>("");

```

在这个例子中，编译器为泛型类`MyClass<X>`的形式类型参数`X`推断了类型`Integer`。它为这个泛型类的构造函数的形式类型参数`T`推断了类型`String`。

* * *

**注意：**需要注意的是，推断算法仅使用调用参数、目标类型和可能的明显预期返回类型来推断类型。推断算法不使用程序后面的结果。

* * *

## 目标类型

Java 编译器利用目标类型推断泛型方法调用的类型参数。表达式的*目标类型*是 Java 编译器根据表达式出现的位置所期望的数据类型。考虑声明如下的方法`Collections.emptyList`：

```java
static <T> List<T> emptyList();

```

考虑以下赋值语句：

```java
List<String> listOne = Collections.emptyList();

```

这个语句期望一个`List<String>`的实例；这个数据类型是目标类型。因为方法`emptyList`返回类型为`List<T>`的值，编译器推断类型参数`T`必须是值`String`。这在 Java SE 7 和 8 中都适用。或者，您可以使用类型推断并指定`T`的值如下：

```java
List<String> listOne = Collections.<String>emptyList();

```

然而，在这种情况下并不是必需的。尽管在其他情况下是必需的。考虑以下方法：

```java
void processStringList(List<String> stringList) {
    // process stringList
}

```

假设您想要使用空列表调用方法`processStringList`。在 Java SE 7 中，以下语句不会编译：

```java
processStringList(Collections.emptyList());

```

Java SE 7 编译器生成类似以下的错误消息：

```java
List<Object> cannot be converted to List<String>

```

编译器需要一个类型参数`T`的值，因此它从值`Object`开始。因此，调用`Collections.emptyList`返回一个类型为`List<Object>`的值，这与方法`processStringList`不兼容。因此，在 Java SE 7 中，您必须如下指定类型参数的值：

```java
processStringList(Collections.<String>emptyList());

```

在 Java SE 8 中，这已经不再是必需的。什么是目标类型的概念已经扩展到包括方法参数，比如方法`processStringList`的参数。在这种情况下，`processStringList`需要一个类型为`List<String>`的参数。方法`Collections.emptyList`返回一个`List<T>`的值，因此使用`List<String>`的目标类型，编译器推断类型参数`T`的值为`String`。因此，在 Java SE 8 中，以下语句编译通过：

```java
processStringList(Collections.emptyList());

```

查看目标类型在 Lambda 表达式中获取更多信息。
