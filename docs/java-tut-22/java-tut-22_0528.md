# 对象排序

> 原文：[`docs.oracle.com/javase/tutorial/collections/interfaces/order.html`](https://docs.oracle.com/javase/tutorial/collections/interfaces/order.html)

一个`List` `l`可以按以下方式排序。

```java
Collections.sort(l);

```

如果`List`包含`String`元素，则将按字母顺序对其进行排序。如果包含`Date`元素，则将按时间顺序对其进行排序。这是如何发生的呢？`String`和`Date`都实现了[`Comparable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)`接口。`Comparable`实现为一个类提供了*自然排序*，允许该类的对象自动排序。下表总结了一些更重要的实现`Comparable`接口的 Java 平台类。

**实现 Comparable 接口的类**

| 类 | 自然排序 |
| --- | --- |
| `Byte` | 有符号数值 |
| `Character` | 无符号数值 |
| `Long` | 有符号数值 |
| `Integer` | 有符号数值 |
| `Short` | 有符号数值 |
| `Double` | 有符号数值 |
| `Float` | 有符号数值 |
| `BigInteger` | 有符号数值 |
| `BigDecimal` | 有符号数值 |
| `Boolean` | `Boolean.FALSE < Boolean.TRUE` |
| `File` | 基于路径名的系统相关字典序 |
| `String` | 字典序 |
| `Date` | 时间顺序 |
| `CollationKey` | 区域特定字典序 |

如果你尝试对一个不实现`Comparable`接口的列表进行排序，`Collections.sort(list)`将抛出一个[`ClassCastException`](https://docs.oracle.com/javase/8/docs/api/java/lang/ClassCastException.html)。同样，如果你尝试使用`comparator`对无法相互比较的列表进行排序，`Collections.sort(list, comparator)`将抛出`ClassCastException`。可以相互比较的元素被称为*可相互比较的*。尽管不同类型的元素可能是可相互比较的，但这里列出的类中没有一个允许跨类比较。

如果你只想对可比较元素的列表进行排序或创建排序的集合，那么关于`Comparable`接口，这就是你真正需要知道的全部内容。如果你想要实现自己的`Comparable`类型，那么下一节将对你感兴趣。

## 编写自己的可比较类型

`Comparable`接口包含以下方法。

```java
public interface Comparable<T> {
    public int compareTo(T o);
}

```

`compareTo`方法比较接收对象与指定对象，并根据接收对象是小于、等于还是大于指定对象返回负整数、0 或正整数。如果指定对象无法与接收对象比较，则该方法会抛出`ClassCastException`。

下面的类代表一个人的名字，实现了`Comparable`。`examples/Name.java`

```java
import java.util.*;

public class Name implements Comparable<Name> {
    private final String firstName, lastName;

    public Name(String firstName, String lastName) {
        if (firstName == null || lastName == null)
            throw new NullPointerException();
        this.firstName = firstName;
        this.lastName = lastName;
    }

    public String firstName() { return firstName; }
    public String lastName()  { return lastName;  }

    public boolean equals(Object o) {
        if (!(o instanceof Name))
            return false;
        Name n = (Name) o;
        return n.firstName.equals(firstName) && n.lastName.equals(lastName);
    }

    public int hashCode() {
        return 31*firstName.hashCode() + lastName.hashCode();
    }

    public String toString() {
	return firstName + " " + lastName;
    }

    public int compareTo(Name n) {
        int lastCmp = lastName.compareTo(n.lastName);
        return (lastCmp != 0 ? lastCmp : firstName.compareTo(n.firstName));
    }
}

```

为了保持前面的示例简洁，该类有一定的限制：它不支持中间名，要求同时有名和姓，而且在任何方面都没有国际化。尽管如此，它阐明了以下重要点：

+   `Name` 对象是*不可变的*。其他条件相同的情况下，不可变类型是最好的选择，特别是对于将用作 `Set` 中的元素或 `Map` 中的键的对象。如果在集合中修改它们的元素或键，这些集合将会中断。

+   构造函数检查其参数是否为 `null`。这确保所有的 `Name` 对象都是格式良好的，以便其他方法永远不会抛出 `NullPointerException`。

+   `hashCode` 方法被重新定义。这对于重新定义 `equals` 方法的任何类都是必不可少的。（相等的对象必须具有相等的哈希码。）

+   `equals` 方法在指定对象为 `null` 或不合适类型时返回 `false`。`compareTo` 方法在这些情况下会抛出运行时异常。这两种行为都是各自方法的一般契约所要求的。

+   `toString` 方法已被重新定义，以便以人类可读的形式打印 `Name`。这总是一个好主意，特别是对于将要放入集合中的对象。各种集合类型的 `toString` 方法依赖于它们的元素、键和值的 `toString` 方法。

由于本节是关于元素排序的，让我们再谈一下 `Name` 的 `compareTo` 方法。它实现了标准的名称排序算法，其中姓氏优先于名字。这正是你在自然排序中想要的。如果自然排序是不自然的，那将会非常令人困惑！

看一下 `compareTo` 的实现方式，因为它是相当典型的。首先，你比较对象的最重要部分（在这种情况下是姓）。通常情况下，你可以直接使用部分类型的自然排序。在这种情况下，部分是一个 `String`，自然（词典）排序正是所需的。如果比较结果不是零，表示相等，你就完成了：只需返回结果。如果最重要的部分相等，你继续比较下一个最重要的部分。在这种情况下，只有两个部分——名和姓。如果有更多的部分，你会按照明显的方式继续，比较部分直到找到两个不相等的部分或者你正在比较最不重要的部分，此时你会返回比较的结果。

为了展示它是如何工作的，这里是一个构建名称列表并对其进行排序的程序。

```java
import java.util.*;

public class NameSort {
    public static void main(String[] args) {
        Name nameArray[] = {
            new Name("John", "Smith"),
            new Name("Karl", "Ng"),
            new Name("Jeff", "Smith"),
            new Name("Tom", "Rich")
        };

        List<Name> names = Arrays.asList(nameArray);
        Collections.sort(names);
        System.out.println(names);
    }
}

```

如果你运行这个程序，这是它打印的内容。

```java
[Karl Ng, Tom Rich, Jeff Smith, John Smith]

```

`compareTo`方法的行为有四个限制，我们现在不会详细讨论，因为它们相当技术性和乏味，并且最好留在 API 文档中。所有实现`Comparable`的类都必须遵守这些限制，因此如果您正在编写实现它的类，请阅读`Comparable`的文档。尝试对违反这些限制的对象列表进行排序会导致未定义的行为。从技术上讲，这些限制确保自然排序是实现它的类的对象上的*全序*；这是确保排序是明确定义的必要条件。

## 比较器

如果您想按照除自然排序之外的顺序对一些对象进行排序怎么办？或者如果您想对一些不实现`Comparable`接口的对象进行排序怎么办？要做到这两点，您需要提供一个[`Comparator`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)  一个封装排序的对象。与`Comparable`接口一样，`Comparator`接口由一个方法组成。

```java
public interface Comparator<T> {
    int compare(T o1, T o2);
}

```

`compare`方法比较其两个参数，根据第一个参数是否小于、等于或大于第二个参数返回负整数、0 或正整数。如果任一参数的类型对于`Comparator`不合适，则`compare`方法会抛出`ClassCastException`。

大部分关于`Comparable`的内容也适用于`Comparator`。编写`compare`方法几乎与编写`compareTo`方法相同，只是前者将两个对象作为参数传递。`compare`方法必须遵守与`Comparable`的`compareTo`方法相同的四个技术限制，原因是`Comparator`必须对其比较的对象引入一个全序。

假设您有一个名为`Employee`的类，如下所示。

```java
public class Employee implements Comparable<Employee> {
    public Name name()     { ... }
    public int number()    { ... }
    public Date hireDate() { ... }
       ...
}

```

假设`Employee`实例的自然排序是根据员工姓名（如前面的示例中定义的）的`Name`排序。不幸的是，老板要求按资历顺序列出员工名单。这意味着我们需要做一些工作，但不多。以下程序将生成所需的列表。

```java
import java.util.*;
public class EmpSort {
    static final Comparator<Employee> SENIORITY_ORDER = 
                                        new Comparator<Employee>() {
            public int compare(Employee e1, Employee e2) {
                return e2.hireDate().compareTo(e1.hireDate());
            }
    };

    // Employee database
    static final Collection<Employee> employees = ... ;

    public static void main(String[] args) {
        List<Employee> e = new ArrayList<Employee>(employees);
        Collections.sort(e, SENIORITY_ORDER);
        System.out.println(e);
    }
}

```

程序中的`Comparator`相当简单。它依赖于应用于`hireDate`访问器方法返回的值的`Date`的自然排序。请注意，`Comparator`将其第二个参数的入职日期传递给其第一个参数，而不是反过来。原因是最近入职的员工资历最低；按照入职日期排序会将列表按照逆资历顺序排列。有时人们用来实现这种效果的另一种技术是保持参数顺序，但对比较结果取反。

```java
// Don't do this!!
return -r1.hireDate().compareTo(r2.hireDate());

```

你应该始终使用前一种技术而不是后一种，因为后一种不能保证有效。原因是`compareTo`方法如果其参数小于调用它的对象，则可以返回任何负的`int`。有一个负的`int`在取反后仍然是负的，尽管这看起来很奇怪。

```java
-Integer.MIN_VALUE == Integer.MIN_VALUE

```

前面程序中的`Comparator`用于对`List`进行排序很好，但它有一个缺陷：它不能用于对已排序的集合（如`TreeSet`）进行排序，因为它生成的排序与`equals`不兼容。这意味着这个`Comparator`将把`equals`方法不认为相等的对象等同起来。特别是，任何在同一天入职的两名员工将被视为相等。当你对`List`进行排序时，这并不重要；但当你使用`Comparator`对已排序的集合进行排序时，这是致命的。如果你使用这个`Comparator`将多名在同一天入职的员工插入`TreeSet`，只有第一个会被添加到集合中；第二个将被视为重复元素并被忽略。

要解决这个问题，只需微调`Comparator`，使其生成一个与`equals`兼容的排序。换句话说，调整它使得当使用`compare`进行比较时，只有那些在使用`equals`进行比较时也被视为相等的元素才被视为相等。做法是执行一个两部分比较（如对`Name`），其中第一部分是我们感兴趣的部分——在这种情况下是入职日期——第二部分是一个唯一标识对象的属性。在这里，员工编号是显而易见的属性。这是产生的`Comparator`。

```java
static final Comparator<Employee> SENIORITY_ORDER = 
                                        new Comparator<Employee>() {
    public int compare(Employee e1, Employee e2) {
        int dateCmp = e2.hireDate().compareTo(e1.hireDate());
        if (dateCmp != 0)
            return dateCmp;

        return (e1.number() < e2.number() ? -1 :
               (e1.number() == e2.number() ? 0 : 1));
    }
};

```

最后一点：你可能会想要用更简单的方式替换`Comparator`中的最后一个`return`语句：

```java
return e1.number() - e2.number();

```

不要这样做，除非你*绝对确定*没有人会有负的员工编号！这个技巧通常不起作用，因为有符号整数类型不足以表示任意两个有符号整数的差值。如果`i`是一个很大的正整数，而`j`是一个很大的负整数，`i - j`会溢出并返回一个负整数。所得到的`comparator`违反了我们一直谈论的四个技术限制之一（传递性），并产生可怕的、微妙的错误。这不仅仅是理论上的担忧；人们会因此受到伤害。
