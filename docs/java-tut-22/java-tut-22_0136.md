# 泛型的限制。

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/restrictions.html`](https://docs.oracle.com/javase/tutorial/java/generics/restrictions.html)。

要有效地使用 Java 泛型，您必须考虑以下限制：

+   不能用原始类型实例化泛型类型。

+   不能创建类型参数的实例。

+   不能声明其类型为类型参数的静态字段。

+   不能在参数化类型中使用强制类型转换或`instanceof`。

+   不能创建参数化类型的数组。

+   不能创建、捕获或抛出参数化类型的对象。

+   不能重载形式参数类型擦除为相同原始类型的方法。

## 不能用原始类型实例化泛型类型。

考虑以下参数化类型：

```java
class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    // ...
}

```

在创建`Pair`对象时，您不能用原始类型替换类型参数`K`或`V`：

```java
Pair<int, char> p = new Pair<>(8, 'a');  // compile-time error

```

您只能用非原始类型替换类型参数`K`和`V`：

```java
Pair<Integer, Character> p = new Pair<>(8, 'a');

```

请注意，Java 编译器会将`8`自动装箱为`Integer.valueOf(8)`，将`'a'`自动装箱为`Character('a')`：

```java
Pair<Integer, Character> p = new Pair<>(Integer.valueOf(8), new Character('a'));

```

有关自动装箱的更多信息，请参见自动装箱和拆箱中的数字和字符串课程。

## 不能创建类型参数的实例。

你不能创建一个类型参数的实例。例如，以下代码会导致编译时错误：

```java
public static <E> void append(List<E> list) {
    E elem = new E();  // compile-time error
    list.add(elem);
}

```

作为一种解决方法，您可以通过反射创建一个类型参数的对象：

```java
public static <E> void append(List<E> list, Class<E> cls) throws Exception {
    E elem = cls.newInstance();   // OK
    list.add(elem);
}

```

您可以按以下方式调用`append`方法：

```java
List<String> ls = new ArrayList<>();
append(ls, String.class);

```

## 不能声明其类型为类型参数的静态字段。

类的静态字段是所有非静态对象共享的类级变量。因此，不允许类型参数的静态字段。考虑以下类：

```java
public class MobileDevice<T> {
    private static T os;

    // ...
}

```

如果允许类型参数的静态字段，则以下代码将会混淆：

```java
MobileDevice<Smartphone> phone = new MobileDevice<>();
MobileDevice<Pager> pager = new MobileDevice<>();
MobileDevice<TabletPC> pc = new MobileDevice<>();

```

因为静态字段`os`被`phone`、`pager`和`pc`共享，`os`的实际类型是什么？它不能同时是`Smartphone`、`Pager`和`TabletPC`。因此，您不能创建类型参数的静态字段。

## 不能在参数化类型中使用强制类型转换或`instanceof`。

因为 Java 编译器会擦除泛型代码中的所有类型参数，所以无法在运行时验证泛型类型的参数化类型：

```java
public static <E> void rtti(List<E> list) {
    if (list instanceof ArrayList<Integer>) {  // compile-time error
        // ...
    }
}

```

传递给`rtti`方法的参数化类型集合为：

```java
S = { ArrayList<Integer>, ArrayList<String> LinkedList<Character>, ... }

```

运行时不会跟踪类型参数，因此无法区分`ArrayList<Integer>`和`ArrayList<String>`之间的区别。您最多可以使用无界通配符来验证列表是否是`ArrayList`：

```java
public static void rtti(List<?> list) {
    if (list instanceof ArrayList<?>) {  // OK; instanceof requires a reifiable type
        // ...
    }
}

```

通常情况下，除非使用无界通配符进行参数化，否则不能进行参数化类型的强制转换。例如：

```java
List<Integer> li = new ArrayList<>();
List<Number>  ln = (List<Number>) li;  // compile-time error

```

但是，在某些情况下，编译器知道类型参数始终有效并允许强制转换。例如：

```java
List<String> l1 = ...;
ArrayList<String> l2 = (ArrayList<String>)l1;  // OK

```

## 不能创建参数化类型的数组。

你不能创建参数化类型的数组。例如，以下代码无法编译：

```java
List<Integer>[] arrayOfLists = new List<Integer>[2];  // compile-time error

```

以下代码说明了当不同类型插入数组时会发生什么：

```java
Object[] strings = new String[2];
strings[0] = "hi";   // OK
strings[1] = 100;    // An ArrayStoreException is thrown.

```

如果你尝试对一个泛型列表做同样的事情，会出现问题：

```java
Object[] stringLists = new List<String>[2];  // compiler error, but pretend it's allowed
stringLists[0] = new ArrayList<String>();   // OK
stringLists[1] = new ArrayList<Integer>();  // An ArrayStoreException should be thrown,
                                            // but the runtime can't detect it.

```

如果允许参数化列表的数组，上述代码将无法抛出期望的`ArrayStoreException`。

## 无法创建、捕获或抛出参数化类型的对象

一个泛型类不能直接或间接地扩展`Throwable`类。例如，以下类将无法编译：

```java
// Extends Throwable indirectly
class MathException<T> extends Exception { /* ... */ }    // compile-time error

// Extends Throwable directly
class QueueFullException<T> extends Throwable { /* ... */ // compile-time error

```

一个方法无法捕获类型参数的实例：

```java
public static <T extends Exception, J> void execute(List<J> jobs) {
    try {
        for (J job : jobs)
            // ...
    } catch (T e) {   // compile-time error
        // ...
    }
}

```

但是，你可以在`throws`子句中使用类型参数：

```java
class Parser<T extends Exception> {
    public void parse(File file) throws T {     // OK
        // ...
    }
}

```

## 无法重载形式参数类型擦除为相同原始类型的方法

一个类不能有两个在类型擦除后具有相同签名的重载方法。

```java
public class Example {
    public void print(Set<String> strSet) { }
    public void print(Set<Integer> intSet) { }
}

```

这些重载将共享相同的类文件表示，并将生成编译时错误。
