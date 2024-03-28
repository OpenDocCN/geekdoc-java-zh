# 教训：算法

> 原文：[`docs.oracle.com/javase/tutorial/collections/algorithms/index.html`](https://docs.oracle.com/javase/tutorial/collections/algorithms/index.html)

这里描述的*多态算法*是 Java 平台提供的可重用功能块。所有这些功能块都来自[`Collections`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html)类，并且都采用静态方法的形式，其第一个参数是要执行操作的集合。Java 平台提供的绝大多数算法都是在[`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)实例上操作的，但其中有一些是在任意[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)实例上操作的。本节简要描述了以下算法：

+   排序

+   洗牌

+   常规数据操作

+   搜索

+   组合

+   查找极值

## 排序

`sort`算法重新排列一个`List`，使其元素按照一种排序关系升序排列。提供了两种形式的操作。简单形式接受一个`List`，并根据其元素的*自然排序*对其进行排序。如果您对自然排序的概念不熟悉，请阅读对象排序部分。

`sort`操作使用了一个稍微优化的*归并排序*算法，速度快且稳定：

+   **快速**：它保证在`n log(n)`时间内运行，并在几乎排序好的列表上运行得更快。经验测试表明它与高度优化的快速排序一样快。快速排序通常被认为比归并排序更快，但不稳定且不能保证`n log(n)`性能。

+   **稳定**：它不会重新排序相等的元素。这一点对于在不同属性上重复对同一列表进行排序很重要。如果邮件程序的用户按邮件日期对收件箱进行排序，然后按发件人对其进行排序，用户自然期望来自同一发件人的现在连续的邮件列表仍然按邮件日期排序。只有第二次排序是稳定的才能保证这一点。

以下`简单程序`按字典顺序打印出其参数。

```java

import java.util.*;

public class Sort {
    public static void main(String[] args) {
        List<String> list = Arrays.asList(args);
        Collections.sort(list);
        System.out.println(list);
    }
}

```

让我们运行程序。

```java
% java Sort i walk the line

```

产生了以下输出。

```java
[i, line, the, walk]

```

该程序仅用于向您展示算法确实像看起来那样易于使用。

第二种`sort`形式除了`List`之外还需要一个[`Comparator`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)，并使用`Comparator`对元素进行排序。假设你想要按照我们之前示例中的字谜分组的大小倒序打印出来，即最大的字谜组首先显示。接下来的示例展示了如何借助`sort`方法的第二种形式实现这一目标。

请记住，变位词组以`List`实例的形式存储在`Map`中的值中。修改后的打印代码通过`Map`的值视图迭代，将通过最小大小测试的每个`List`放入一个`List`的`List`中。然后，该代码对此`List`进行排序，使用一个期望`List`实例的`Comparator`，并实现逆大小排序。最后，该代码对排序后的`List`进行迭代，打印其元素（变位词组）。以下代码替换了`Anagrams`示例中`main`方法末尾的打印代码。

```java
// Make a List of all anagram groups above size threshold.
List<List<String>> winners = new ArrayList<List<String>>();
for (List<String> l : m.values())
    if (l.size() >= minGroupSize)
        winners.add(l);

// Sort anagram groups according to size
Collections.sort(winners, new Comparator<List<String>>() {
    public int compare(List<String> o1, List<String> o2) {
        return o2.size() - o1.size();
    }});

// Print anagram groups.
for (List<String> l : winners)
    System.out.println(l.size() + ": " + l);

```

在与 The Map Interface 部分相同的字典上运行`程序`，具有相同的最小变位词组大小（八），产生以下输出。

```java

12: [apers, apres, asper, pares, parse, pears, prase,
       presa, rapes, reaps, spare, spear]
11: [alerts, alters, artels, estral, laster, ratels,
       salter, slater, staler, stelar, talers]
10: [least, setal, slate, stale, steal, stela, taels,
       tales, teals, tesla]
9: [estrin, inerts, insert, inters, niters, nitres,
       sinter, triens, trines]
9: [capers, crapes, escarp, pacers, parsec, recaps,
       scrape, secpar, spacer]
9: [palest, palets, pastel, petals, plates, pleats,
       septal, staple, tepals]
9: [anestri, antsier, nastier, ratines, retains, retinas,
       retsina, stainer, stearin]
8: [lapse, leaps, pales, peals, pleas, salep, sepal, spale]
8: [aspers, parses, passer, prases, repass, spares,
       sparse, spears]
8: [enters, nester, renest, rentes, resent, tenser,
       ternes,��treens]
8: [arles, earls, lares, laser, lears, rales, reals, seral]
8: [earings, erasing, gainers, reagins, regains, reginas,
       searing, seringa]
8: [peris, piers, pries, prise, ripes, speir, spier, spire]
8: [ates, east, eats, etas, sate, seat, seta, teas]
8: [carets, cartes, caster, caters, crates, reacts,
       recast,��traces]

```

## 洗牌

`shuffle`算法与`sort`的作用相反，破坏了`List`中可能存在的任何顺序痕迹。换句话说，该算法根据来自随机源的输入重新排序`List`，以使所有可能的排列以相等的概率发生，假设有一个公平的随机源。该算法在实现游戏机会时非常有用。例如，它可以用来洗牌代表一副牌的`Card`对象的`List`。此外，它还用于生成测试用例。

此操作有两种形式：一种接受一个`List`并使用默认的随机源，另一种要求调用者提供一个[Random](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html)对象作为随机源。该算法的代码被用作`List`部分的示例。

## 常规数据操作

`Collections`类提供了五种用于在`List`对象上进行常规数据操作的算法，所有这些算法都非常简单：

+   `reverse`  颠倒`List`中元素的顺序。

+   `fill`  用指定值覆盖`List`中的每个元素。此操作对重新初始化`List`非常有用。

+   `copy`  接受两个参数，目标`List`和源`List`，并将源的元素复制到目标中，覆盖其内容。目标`List`的长度必须至少与源相同。如果目标更长，则目标中剩余的元素不受影响。

+   `swap`  交换`List`中指定位置的元素。

+   `addAll`  将所有指定的元素添加到`Collection`中。要添加的元素可以逐个指定，也可以作为数组指定。

## 搜索

`binarySearch`算法在排序的`List`中搜索指定的元素。该算法有两种形式。第一种接受一个`List`和一个要搜索的元素（"搜索键"）。这种形式假定`List`根据其元素的自然顺序按升序排序。第二种形式除了`List`和搜索键外还接受一个`Comparator`，并假定`List`根据指定的`Comparator`按升序排序。在调用`binarySearch`之前，可以使用`sort`算法对`List`进行排序。

两种形式的返回值相同。如果`List`包含搜索键，则返回其索引。如果没有，则返回值为`(-(插入点) - 1)`，其中插入点是将值插入`List`的位置，或者大于该值的第一个元素的索引，或者`list.size()`如果`List`中的所有元素都小于指定值。这个确实丑陋的公式保证了返回值仅当找到搜索键时为`>= 0`。这基本上是一个将布尔值`(found)`和整数值`(index)`组合成单个`int`返回值的技巧。

下面的习语可用于`binarySearch`操作的两种形式，查找指定的搜索键，并在适当位置插入它（如果尚未存在）。

```java
int pos = Collections.binarySearch(list, key);
if (pos < 0)
   l.add(-pos-1, key);

```

## 组成

频率和不相交算法测试一个或多个`Collections`的组成方面：

+   `frequency`  计算指定元素在指定集合中出现的次数

+   `disjoint`  确定两个`Collections`是否不相交；即它们是否没有共同元素。

## 查找极值

`min`和`max`算法分别返回指定`Collection`中包含的最小和最大元素。这两个操作有两种形式。简单形式只接受一个`Collection`，根据元素的自然顺序返回最小（或最大）元素。第二种形式除了`Collection`外还接受一个`Comparator`，根据指定的`Comparator`返回最小（或最大）元素。
