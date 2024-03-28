# 集合接口

> 原文：[`docs.oracle.com/javase/tutorial/collections/interfaces/set.html`](https://docs.oracle.com/javase/tutorial/collections/interfaces/set.html)

一个[`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)是一个[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)，不能包含重复元素。它模拟了数学集合的抽象。`Set`接口*仅*包含从`Collection`继承的方法，并添加了禁止重复元素的限制。`Set`还对`equals`和`hashCode`操作的行为添加了更强的约定，允许有意义地比较`Set`实例，即使它们的实现类型不同。如果两个`Set`实例包含相同的元素，则它们相等。

Java 平台包含三种通用的`Set`实现：`HashSet`、`TreeSet`和`LinkedHashSet`。[`HashSet`](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html)将其元素存储在哈希表中，是性能最佳的实现；但是它不保证迭代顺序。[`TreeSet`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html)将其元素存储在红黑树中，根据值对其元素进行排序；它比`HashSet`慢得多。[`LinkedHashSet`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashSet.html)作为一个哈希表实现，其中有一个通过它的链表运行的链表，根据它们插入到集合中的顺序（插入顺序）对其元素进行排序。`LinkedHashSet`通过略微更高的成本使其客户免受`HashSet`提供的未指定、通常混乱的排序。

这是一个简单但有用的`Set`习语。假设你有一个`Collection`，`c`，你想创建另一个包含相同元素但消除所有重复项的`Collection`。以下一行代码就可以搞定。

```java
Collection<Type> noDups = new HashSet<Type>(c);

```

它通过创建一个`Set`（根据定义，不能包含重复项），最初包含`c`中的所有元素。它使用在集合接口部分描述的标准转换构造函数。

或者，如果使用 JDK 8 或更高版本，您可以使用聚合操作轻松地收集到一个`Set`中：

```java
c.stream()
.collect(Collectors.toSet()); // no duplicates

```

这是一个稍微更长的示例，将一个`Collection`中的名称累积到一个`TreeSet`中：

```java
Set<String> set = people.stream()
.map(Person::getName)
.collect(Collectors.toCollection(TreeSet::new));

```

以下是第一个习语的一个次要变体，保留原始集合的顺序，同时删除重复元素：

```java
Collection<Type> noDups = new LinkedHashSet<Type>(c);

```

以下是一个通用方法，封装了前面的习语，返回一个与传入的相同泛型类型的`Set`。

```java
public static <E> Set<E> removeDups(Collection<E> c) {
    return new LinkedHashSet<E>(c);
}

```

## 集合接口基本操作

`size`操作返回`Set`中元素的数量（其*基数*）。`isEmpty`方法执行您认为它会执行的操作。`add`方法将指定的元素添加到`Set`中（如果尚未存在）并返回一个指示元素是否已添加的布尔值。类似地，`remove`方法从`Set`中移除指定的元素（如果存在）并返回一个指示元素是否存在的布尔值。`iterator`方法返回`Set`上的`Iterator`。

以下`程序`打印出其参数列表中的所有不同单词。提供了两个版本的此程序。第一个使用了 JDK 8 聚合操作。第二个使用了`for-each`结构。

使用 JDK 8 聚合操作：

```java
import java.util.*;
import java.util.stream.*;

public class FindDups {
    public static void main(String[] args) {
        Set<String> distinctWords = Arrays.asList(args).stream()
		.collect(Collectors.toSet()); 
        System.out.println(distinctWords.size()+ 
                           " distinct words: " + 
                           distinctWords);
    }
}

```

使用`for-each`结构：

```java
import java.util.*;

public class FindDups {
    public static void main(String[] args) {
        Set<String> s = new HashSet<String>();
        for (String a : args)
               s.add(a);
        System.out.println(s.size() + " distinct words: " + s);
    }
}

```

现在运行程序的任一版本。

```java
java FindDups i came i saw i left

```

生成以下输出：

```java
4 distinct words: [left, came, saw, i]

```

请注意，代码总是通过其接口类型（`Set`）而不是其实现类型来引用`Collection`。这是一种*强烈*推荐的编程实践，因为它使您可以通过仅更改构造函数来灵活地更改实现。如果用于存储集合的任何变量或用于传递集合的参数声明为`Collection`的实现类型而不是其接口类型，那么为了更改其实现类型，*所有*这些变量和参数都必须更改。

此外，并不能保证生成的程序将正常工作。如果程序使用原始实现类型中存在但新实现类型中不存在的任何非标准操作，则程序将失败。仅通过其接口引用集合可以防止您使用任何非标准操作。

前面示例中`Set`的实现类型是`HashSet`，它不保证`Set`中元素的顺序。如果希望程序按字母顺序打印单词列表，只需将`Set`的实现类型从`HashSet`更改为`TreeSet`。进行这个微不足道的一行更改会导致前面示例中的命令行生成以下输出。

```java
java FindDups i came i saw i left

4 distinct words: [came, i, left, saw]

```

## 设置接口批量操作

批量操作特别适用于`Set`；应用时，它们执行标准的集合代数操作。假设`s1`和`s2`是集合。以下是批量操作的作用：

+   `s1.containsAll(s2)`  如果`s2`是`S1`的**子集**，则返回`true`。（如果集合`s1`包含`S2`中的所有元素，则`s2`是`s1`的子集。）

+   `s1.addAll(s2)`  将`s1`转换为`s1`和`s2`的**并集**。（两个集合的并集是包含在任一集合中的所有元素的集合。）

+   `s1.retainAll(s2)`  将`s1`转换为`s1`和`s2`的交集。（两个集合的交集是仅包含两个集合共有元素的集合。）

+   `s1.removeAll(s2)`  将`s1`转换为`s1`和`s2`的（非对称的）差集。（例如，`s1`减去`s2`的差集是包含`s1`中所有元素但不在`s2`中的元素的集合。）

要以*非破坏性*（不修改任何集合）计算两个集合的并集、交集或差集，调用者必须在调用适当的批量操作之前复制一个集合。以下是生成的习语。

```java
Set<Type> union = new HashSet<Type>(s1);
union.addAll(s2);

Set<Type> intersection = new HashSet<Type>(s1);
intersection.retainAll(s2);

Set<Type> difference = new HashSet<Type>(s1);
difference.removeAll(s2);

```

在前述习语中，结果`Set`的实现类型是`HashSet`，正如前面提到的，这是 Java 平台上最好的通用`Set`实现。然而，任何通用的`Set`实现都可以替代。

让我们重新审视`FindDups`程序。假设你想知道参数列表中哪些单词只出现一次，哪些出现多次，但你不想重复打印任何重复的单词。这个效果可以通过生成两个集合来实现  一个包含参数列表中的每个单词，另一个只包含重复的单词。只出现一次的单词是这两个集合的差集，我们知道如何计算。`生成的程序`如下所示。

```java

import java.util.*;

public class FindDups2 {
    public static void main(String[] args) {
        Set<String> uniques = new HashSet<String>();
        Set<String> dups    = new HashSet<String>();

        for (String a : args)
            if (!uniques.add(a))
                dups.add(a);

        // Destructive set-difference
        uniques.removeAll(dups);

        System.out.println("Unique words:    " + uniques);
        System.out.println("Duplicate words: " + dups);
    }
}

```

当使用先前使用的相同参数列表运行时（`i came i saw i left`），程序产生以下输出。

```java
Unique words:    [left, saw, came]
Duplicate words: [i]

```

一个不太常见的集合代数操作是*对称差集*  包含在两个指定集合中的元素，但不在两者中都包含的元素的集合。以下代码以非破坏性方式计算两个集合的对称差集。

```java
Set<Type> symmetricDiff = new HashSet<Type>(s1);
symmetricDiff.addAll(s2);
Set<Type> tmp = new HashSet<Type>(s1);
tmp.retainAll(s2);
symmetricDiff.removeAll(tmp);

```

## 集合接口数组操作

数组操作对`Set`并没有特殊处理，超出了它们对任何其他`Collection`的处理。这些操作在集合接口部分有描述。
