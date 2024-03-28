# 泛型类型

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/types.html`](https://docs.oracle.com/javase/tutorial/java/generics/types.html)

*泛型类型*是一个参数化类型的泛型类或接口。下面的 `Box` 类将被修改以演示这个概念。

## 一个简单的 Box 类

首先看一下一个操作任意类型对象的非泛型 `Box` 类。它只需要提供两个方法：`set`，用于向盒子中添加对象，和 `get`，用于检索对象：

```java
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}

```

由于它的方法接受或返回一个 `Object`，你可以自由传入任何你想要的东西，只要不是原始类型之一。在编译时无法验证类的使用方式。代码的一部分可能将一个 `Integer` 放入盒子中，并期望从中获取 `Integer`，而代码的另一部分可能错误地传入一个 `String`，导致运行时错误。

## Box 类的泛型版本

一个*泛型类*的定义格式如下：

```java
class name<T1, T2, ..., Tn> { /* ... */ }

```

类型参数部分，由尖括号（`<>`）界定，跟在类名后面。它指定了*类型参数*（也称为*类型变量*）`T1`、`T2`、... 和 `Tn`。

要将 `Box` 类更新为使用泛型，你需要通过将代码 "`public class Box`" 更改为 "`public class Box<T>`" 来创建一个*泛型类型声明*。这引入了类型变量 `T`，可以在类内部的任何地方使用。

有了这个改变，`Box` 类变成了：

```java
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}

```

正如你所看到的，所有 `Object` 的出现都被 `T` 替换。类型变量可以是你指定的任何**非原始**类型：任何类类型、任何接口类型、任何数组类型，甚至是另一个类型变量。

这种技术也可以应用于创建泛型接口。

## 类型参数命名约定

按照惯例，类型参数的名称是单个大写字母。这与你已经了解的变量命名约定形成鲜明对比，而且有充分的理由：没有这个约定，很难区分类型变量和普通类或接口名称。

最常用的类型参数名称有：

+   E - Element（Java 集合框架广泛使用）

+   K - Key

+   N - Number

+   T - Type

+   V - Value

+   S,U,V 等 - 第二、第三、第四种类型

你将在 Java SE API 和本课程的其余部分中看到这些名称的使用。

## 调用和实例化泛型类型

要在代码中引用泛型 `Box` 类，你必须执行一个*泛型类型调用*，将 `T` 替换为某个具体值，比如 `Integer`：

```java
Box<Integer> integerBox;

```

你可以将泛型类型调用看作类似于普通方法调用，但是不是向方法传递参数，而是向 `Box` 类本身传递一个*类型参数* — 在本例中是 `Integer`。

* * *

**类型参数和类型参数术语：** 许多开发人员将“类型参数”和“类型参数”这两个术语互换使用，但这两个术语并不相同。在编码时，为了创建参数化类型，需要提供类型参数。因此，在`Foo<T>`中，`T`是类型参数，而在`Foo<String> f`中的`String`是类型参数。本课程在使用这些术语时遵守此定义。

* * *

与任何其他变量声明一样，此代码实际上并不创建新的`Box`对象。它只是声明`integerBox`将保存对“`Box` of `Integer`”的引用，这就是`Box<Integer>`的含义。

通常将泛型类型的调用称为*参数化类型*。

要实例化此类，像往常一样使用`new`关键字，但是在类名和括号之间放置`<Integer>`：

```java
Box<Integer> integerBox = new Box<Integer>();

```

## 钻石

在 Java SE 7 及更高版本中，只要编译器可以从上下文中确定或推断出类型参数，就可以用空类型参数集（<>）替换调用泛型类构造函数所需的类型参数。这一对尖括号<>非正式地称为*钻石*。例如，您可以使用以下语句创建`Box<Integer>`的实例：

```java
Box<Integer> integerBox = new Box<>();

```

有关钻石符号和类型推断的更多信息，请参见类型推断。

## 多个类型参数

如前所述，泛型类可以具有多个类型参数。例如，实现泛型`Pair`接口的泛型`OrderedPair`类：

```java
public interface Pair<K, V> {
    public K getKey();
    public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
	this.key = key;
	this.value = value;
    }

    public K getKey()	{ return key; }
    public V getValue() { return value; }
}

```

以下语句创建了`OrderedPair`类的两个实例：

```java
Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
Pair<String, String>  p2 = new OrderedPair<String, String>("hello", "world");

```

代码`new OrderedPair<String, Integer>`实例化`K`为`String`，`V`为`Integer`。因此，`OrderedPair`的构造函数的参数类型分别为`String`和`Integer`。由于自动装箱，将`String`和`int`传递给类是有效的。

如钻石中所述，因为 Java 编译器可以从声明`OrderedPair<String, Integer>`中推断出`K`和`V`类型，所以可以使用钻石符号缩短这些语句：

```java
OrderedPair<String, Integer> p1 = new OrderedPair<>("Even", 8);
OrderedPair<String, String>  p2 = new OrderedPair<>("hello", "world");

```

要创建泛型接口，遵循与创建泛型类相同的约定。

## 参数化类型

您还可以用参数化类型（即`List<String>`）替换类型参数（即`K`或`V`）。例如，使用`OrderedPair<K, V>`示例：

```java
OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<Integer>(...));

```
