# 方法引用

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html)

你可以使用 lambda 表达式来创建匿名方法。然而，有时候 lambda 表达式仅仅是调用一个已存在的方法。在这种情况下，通过名称引用现有方法通常更清晰。方法引用使你能够做到这一点；它们是紧凑、易读的 lambda 表达式，用于已经有名称的方法。

再次考虑在 lambda 表达式部分讨论的`Person`类：

```java
public class Person {

    // ...

    LocalDate birthday;

    public int getAge() {
        // ...
    }

    public LocalDate getBirthday() {
        return birthday;
    }   

    public static int compareByAge(Person a, Person b) {
        return a.birthday.compareTo(b.birthday);
    }

    // ...
}
```

假设你的社交网络应用的成员被包含在一个数组中，并且你想按年龄对数组进行排序。你可以使用以下代码（在示例`MethodReferencesTest`中找到本节描述的代码片段）：

```java
Person[] rosterAsArray = roster.toArray(new Person[roster.size()]);

class PersonAgeComparator implements Comparator<Person> {
    public int compare(Person a, Person b) {
        return a.getBirthday().compareTo(b.getBirthday());
    }
}

Arrays.sort(rosterAsArray, new PersonAgeComparator());
```

此次调用`sort`的方法签名如下：

```java
static <T> void sort(T[] a, Comparator<? super T> c)
```

注意`Comparator`接口是一个函数式接口。因此，你可以使用 lambda 表达式来代替定义并创建一个实现`Comparator`的类的新实例：

```java
Arrays.sort(rosterAsArray,
    (Person a, Person b) -> {
        return a.getBirthday().compareTo(b.getBirthday());
    }
);
```

然而，比较两个`Person`实例的出生日期的方法`Person.compareByAge`已经存在。你可以在 lambda 表达式的主体中调用这个方法：

```java
Arrays.sort(rosterAsArray,
    (a, b) -> Person.compareByAge(a, b)
);
```

因为这个 lambda 表达式调用了一个已存在的方法，你可以使用方法引用代替 lambda 表达式：

```java
Arrays.sort(rosterAsArray, Person::compareByAge);
```

方法引用`Person::compareByAge`在语义上与 lambda 表达式`(a, b) -> Person.compareByAge(a, b)`相同。它们各自具有以下特征：

+   其形式参数列表是从`Comparator<Person>.compare`复制的，即`(Person, Person)`。

+   其主体调用方法`Person.compareByAge`。

## 方法引用的种类

有四种方法引用的种类：

| 种类 | 语法 | 示例 |
| --- | --- | --- |
| 引用静态方法 | `*ContainingClass*::*staticMethodName*` | `Person::compareByAge` `MethodReferencesExamples::appendStrings` |
| 引用特定对象的实例方法 | `*containingObject*::*instanceMethodName*` | `myComparisonProvider::compareByName` `myApp::appendStrings2` |
| 引用特定类型的任意对象的实例方法 | `*ContainingType*::*methodName*` | `String::compareToIgnoreCase` `String::concat` |
| 引用构造函数 | `*ClassName*::new` | `HashSet::new` |

以下示例，`MethodReferencesExamples`，包含了前三种方法引用的示例：

```java

import java.util.function.BiFunction;

public class MethodReferencesExamples {

    public static <T> T mergeThings(T a, T b, BiFunction<T, T, T> merger) {
        return merger.apply(a, b);
    }

    public static String appendStrings(String a, String b) {
        return a + b;
    }

    public String appendStrings2(String a, String b) {
        return a + b;
    }

    public static void main(String[] args) {

        MethodReferencesExamples myApp = new MethodReferencesExamples();

        // Calling the method mergeThings with a lambda expression
        System.out.println(MethodReferencesExamples.
            mergeThings("Hello ", "World!", (a, b) -> a + b));

        // Reference to a static method
        System.out.println(MethodReferencesExamples.
            mergeThings("Hello ", "World!", MethodReferencesExamples::appendStrings));

        // Reference to an instance method of a particular object        
        System.out.println(MethodReferencesExamples.
            mergeThings("Hello ", "World!", myApp::appendStrings2));

        // Reference to an instance method of an arbitrary object of a
        // particular type
        System.out.println(MethodReferencesExamples.
            mergeThings("Hello ", "World!", String::concat));
    }
}

```

所有的`System.out.println()`语句都打印相同的内容：`Hello World!`

[`BiFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/BiFunction.html)是[`java.util.function`](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)包中许多函数接口之一。`BiFunction`函数接口可以表示接受两个参数并产生结果的 lambda 表达式或方法引用。

### 静态方法引用

方法引用`Person::compareByAge`和`MethodReferencesExamples::appendStrings`是对静态方法的引用。

### 引用特定对象的实例方法

下面是引用特定对象实例方法的示例：

```java
class ComparisonProvider {
    public int compareByName(Person a, Person b) {
        return a.getName().compareTo(b.getName());
    }

    public int compareByAge(Person a, Person b) {
        return a.getBirthday().compareTo(b.getBirthday());
    }
}
ComparisonProvider myComparisonProvider = new ComparisonProvider();
Arrays.sort(rosterAsArray, myComparisonProvider::compareByName);
```

方法引用`myComparisonProvider::compareByName`调用`myComparisonProvider`对象的`compareByName`方法。JRE 推断方法类型参数，本例中为`(Person, Person)`。

类似地，方法引用`myApp::appendStrings2`将调用`myApp`对象的`appendStrings2`方法。JRE 推断方法类型参数，本例中为`(String, String)`。

### 引用特定类型任意对象的实例方法

下面是一个引用特定类型任意对象的实例方法的示例：

```java
String[] stringArray = { "Barbara", "James", "Mary", "John",
    "Patricia", "Robert", "Michael", "Linda" };
Arrays.sort(stringArray, String::compareToIgnoreCase);
```

方法引用`String::compareToIgnoreCase`的等效 lambda 表达式将具有形式参数列表`(String a, String b)`，其中`a`和`b`是用于更好描述此示例的任意名称。方法引用将调用`a.compareToIgnoreCase(b)`方法。

类似地，方法引用`String::concat`将调用`a.concat(b)`方法。

### 构造函数引用

你可以通过使用名称`new`来引用构造函数，与引用静态方法的方式相同。以下方法将元素从一个集合复制到另一个集合：

```java
public static <T, SOURCE extends Collection<T>, DEST extends Collection<T>>
    DEST transferElements(
        SOURCE sourceCollection,
        Supplier<DEST> collectionFactory) {

    DEST result = collectionFactory.get();
    for (T t : sourceCollection) {
        result.add(t);
    }
    return result;
}
```

函数接口`Supplier`包含一个名为`get`的方法，不接受参数并返回一个对象。因此，你可以使用 lambda 表达式调用方法`transferElements`，如下所示：

```java
Set<Person> rosterSetLambda =
    transferElements(roster, () -> { return new HashSet<>(); });
```

你可以使用构造函数引用来替代 lambda 表达式，如下所示：

```java
Set<Person> rosterSet = transferElements(roster, HashSet::new);
```

Java 编译器推断你想要创建一个包含类型为`Person`的元素的`HashSet`集合。或者，你可以按照以下方式指定：

```java
Set<Person> rosterSet = transferElements(roster, HashSet<Person>::new);
```
