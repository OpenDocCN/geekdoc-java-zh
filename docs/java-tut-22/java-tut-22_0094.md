# 作为超类的对象

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/objectclass.html`](https://docs.oracle.com/javase/tutorial/java/IandI/objectclass.html)

[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)类，位于`java.lang`包中，位于类层次结构树的顶部。每个类都是`Object`类的后代，直接或间接的。你使用或编写的每个类都继承了`Object`的实例方法。你不需要使用这些方法中的任何一个，但是如果选择这样做，可能需要用特定于你的类的代码重写它们。本节讨论的从`Object`继承的方法有：

+   `protected Object clone() throws CloneNotSupportedException`

    创建并返回此对象的副本。

+   `public boolean equals(Object obj)`

    指示某个其他对象是否"等于"这个对象。

+   `protected void finalize() throws Throwable`

    垃圾回收器在对象上调用的方法

    集合确定没有更多引用指向该对象

+   `public final Class getClass()`

    返回对象的运行时类。

+   `public int hashCode()`

    为对象返回一个哈希码值。

+   `public String toString()`

    返回对象的字符串表示形式。

`Object`的`notify`、`notifyAll`和`wait`方法在程序中独立运行的线程的活动同步中起着作用，这将在后面的课程中讨论，这里不会涉及。这些方法有五个：

+   `public final void notify()`

+   `public final void notifyAll()`

+   `public final void wait()`

+   `public final void wait(long timeout)`

+   `public final void wait(long timeout, int nanos)`

* * *

**注意：** 这些方法中有一些微妙的方面，特别是`clone`方法。

* * *

## clone()方法

如果一个类或其父类实现了`Cloneable`接口，你可以使用`clone()`方法从现有对象创建一个副本。要创建一个克隆，你需要编写：

```java
*aCloneableObject*.clone();

```

`Object`的这个方法的实现会检查调用`clone()`的对象是否实现了`Cloneable`接口。如果对象没有实现，该方法会抛出一个`CloneNotSupportedException`异常。异常处理将在后面的课程中介绍。目前，你需要知道`clone()`必须声明为

```java
protected Object clone() throws CloneNotSupportedException

```

或：

```java
public Object clone() throws CloneNotSupportedException

```

如果你要编写一个`clone()`方法来覆盖`Object`中的方法。

如果调用`clone()`的对象确实实现了`Cloneable`接口，`Object`的`clone()`方法的实现会创建一个与原始对象相同类的对象，并初始化新对象的成员变量为与原始对象对应的成员变量相同的值。

使你的类可克隆的最简单方法是在类的声明中添加`implements Cloneable`，然后你的对象可以调用`clone()`方法。

对于一些类，`Object` 的 `clone()` 方法的默认行为就很好用。然而，如果一个对象包含对外部对象的引用，比如 `ObjExternal`，你可能需要重写 `clone()` 来获得正确的行为。否则，一个对象对 `ObjExternal` 的更改也会在其克隆中可见。这意味着原始对象和其克隆不是独立的为了解耦它们，你必须重写 `clone()`，使其克隆对象*和* `ObjExternal`。然后原始对象引用 `ObjExternal`，而克隆引用 `ObjExternal` 的克隆，这样对象和其克隆就是真正独立的。

## `equals()` 方法

`equals()` 方法用于比较两个对象是否相等，如果它们相等则返回 `true`。`Object` 类中提供的 `equals()` 方法使用身份运算符 (`==`) 来确定两个对象是否相等。对于基本数据类型，这会给出正确的结果。然而，对于对象来说，这并不适用。`Object` 提供的 `equals()` 方法测试对象的*引用*是否相等也就是说，如果比较的对象是完全相同的对象。

要测试两个对象是否在*等价性*意义上相等（包含相同的信息），你必须重写 `equals()` 方法。下面是一个重写 `equals()` 的 `Book` 类的示例：

```java
public class Book {
    String ISBN;

    public String getISBN() { 
        return ISBN;
    }

    public boolean equals(Object obj) {
        if (obj instanceof Book)
            return ISBN.equals((Book)obj.getISBN()); 
        else
            return false;
    }
}

```

考虑下面这段代码，用于测试 `Book` 类的两个实例是否相等：

```java
// Swing Tutorial, 2nd edition
Book firstBook  = new Book("0201914670");
Book secondBook = new Book("0201914670");
if (firstBook.equals(secondBook)) {
    System.out.println("objects are equal");
} else {
    System.out.println("objects are not equal");
}

```

即使 `firstBook` 和 `secondBook` 引用了两个不同的对象，这个程序会显示 `objects are equal`。它们被认为是相等的，因为比较的对象包含相同的 ISBN 号码。

如果身份运算符对于你的类不合适，你应该始终重写 `equals()` 方法。

* * *

**注意：**如果你重写了 `equals()`，你必须同时重写 `hashCode()`。

* * *

## `finalize()` 方法

`Object` 类提供了一个回调方法 `finalize()`，当对象变为垃圾时*可能会*被调用。`Object` 的 `finalize()` 实现什么也不做你可以重写 `finalize()` 来进行清理，比如释放资源。

`finalize()` 方法*可能会*被系统自动调用，但是何时调用，甚至是否调用都是不确定的。因此，不要依赖这个方法来清理资源。例如，如果在执行 I/O 操作后没有关闭文件描述符，并且期望 `finalize()` 为您关闭它们，那么可能会耗尽文件描述符。相反，使用 `try`-with 资源语句来自动关闭应用程序的资源。参见 The try-with-resources Statement 和 [Finalization and Weak, Soft, and Phantom References](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/considerations.html#sthref63) 中的 *Java Platform, Standard Edition HotSpot Virtual Machine Garbage Collection Tuning Guide*。

## getClass() 方法

你不能重写 `getClass`。

`getClass()`方法返回一个`Class`对象，该对象有一些方法可以获取关于类的信息，比如它的名称（`getSimpleName()`）、它的父类（`getSuperclass()`）和它实现的接口（`getInterfaces()`）。例如，下面的方法获取并显示对象的类名：

```java
void printClassName(Object obj) {
    System.out.println("The object's" + " class is " +
        obj.getClass().getSimpleName());
}

```

[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)类，位于`java.lang`包中，有大量的方法（超过 50 个）。例如，你可以测试类是否为注解（`isAnnotation()`）、接口（`isInterface()`）或枚举（`isEnum()`）。你可以查看对象的字段（`getFields()`）或方法（`getMethods()`），等等。

## `hashCode()`方法

`hashCode()`返回的值是对象的哈希码，是由哈希算法生成的整数值。

根据定义，如果两个对象相等，它们的哈希码*也必须*相等。如果你重写了`equals()`方法，改变了两个对象相等的方式，那么`Object`的`hashCode()`实现就不再有效。因此，如果你重写了`equals()`方法，你也必须重写`hashCode()`方法。

## `toString()`方法

你应该始终考虑在你的类中重写`toString()`方法。

`Object`的`toString()`方法返回对象的`String`表示，对于调试非常有用。对象的`String`表示完全取决于对象本身，这就是为什么你需要在你的类中重写`toString()`。

使用`toString()`和`System.out.println()`可以显示对象的文本表示，比如`Book`的实例：

```java
System.out.println(firstBook.toString());

```

对于正确重写的`toString()`方法，会打印出有用的信息，就像这样：

```java
ISBN: 0201914670; The Swing Tutorial; A Guide to Constructing GUIs, 2nd Edition

```
