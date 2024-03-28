# Map 接口

> 原文：[`docs.oracle.com/javase/tutorial/collections/interfaces/map.html`](https://docs.oracle.com/javase/tutorial/collections/interfaces/map.html)

一个[`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)是一个将键映射到值的对象。地图不能包含重复的键：每个键最多可以映射到一个值。它模拟了数学*函数*抽象。`Map`接口包括基本操作的方法（如`put`，`get`，`remove`，`containsKey`，`containsValue`，`size`和`empty`），批量操作（如`putAll`和`clear`），以及集合视图（如`keySet`，`entrySet`和`values`）。

Java 平台包含三种通用的`Map`实现：[`HashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)，[`TreeMap`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)，和[`LinkedHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html)。它们的行为和性能与`HashSet`，`TreeSet`和`LinkedHashSet`完全类似，如 Set 接口部分所述。

本页的其余部分详细讨论了`Map`接口。但首先，这里有一些使用 JDK 8 聚合操作收集到`Map`的更多示例。在面向对象编程中，对现实世界对象进行建模是一项常见任务，因此合理地认为一些程序可能会，例如，按部门分组员工：

```java
// Group employees by department
Map<Department, List<Employee>> byDept = employees.stream()
.collect(Collectors.groupingBy(Employee::getDepartment));

```

或者按部门计算所有工资的总和：

```java
// Compute sum of salaries by department
Map<Department, Integer> totalByDept = employees.stream()
.collect(Collectors.groupingBy(Employee::getDepartment,
Collectors.summingInt(Employee::getSalary)));

```

或者按及格或不及格分组学生：

```java
// Partition students into passing and failing
Map<Boolean, List<Student>> passingFailing = students.stream()
.collect(Collectors.partitioningBy(s -> s.getGrade()>= PASS_THRESHOLD)); 

```

您还可以按城市分组人员：

```java
// Classify Person objects by city
Map<String, List<Person>> peopleByCity
         = personStream.collect(Collectors.groupingBy(Person::getCity));

```

或者甚至级联两个收集器，按州和城市对人员进行分类：

```java
// Cascade Collectors 
Map<String, Map<String, List<Person>>> peopleByStateAndCity
  = personStream.collect(Collectors.groupingBy(Person::getState,
  Collectors.groupingBy(Person::getCity)))

```

再次强调，这些只是如何使用新的 JDK 8 API 的一些示例。有关 lambda 表达式和聚合操作的深入覆盖，请参阅名为聚合操作的课程。

## Map 接口基本操作

`Map`的基本操作（`put`，`get`，`containsKey`，`containsValue`，`size`和`isEmpty`）的行为与`Hashtable`中的对应操作完全相同。`以下程序`生成其参数列表中找到的单词的频率表。频率表将每个单词映射到其在参数列表中出现的次数。

```java
import java.util.*;

public class Freq {
    public static void main(String[] args) {
        Map<String, Integer> m = new HashMap<String, Integer>();

        // Initialize frequency table from command line
        for (String a : args) {
            Integer freq = m.get(a);
            m.put(a, (freq == null) ? 1 : freq + 1);
        }

        System.out.println(m.size() + " distinct words:");
        System.out.println(m);
    }
}

```

关于此程序唯一棘手的事情是`put`语句的第二个参数。该参数是一个条件表达式，如果单词以前从未见过，则将频率设置为一，或者如果单词已经被看到，则将频率设置为当前值加一。尝试使用以下命令运行此程序：

```java
java Freq if it is to be it is up to me to delegate

```

该程序产生以下输出。

```java
8 distinct words:
{to=3, delegate=1, be=1, it=2, up=1, if=1, me=1, is=2}

```

假设您希望按字母顺序查看频率表。您只需将`Map`的实现类型从`HashMap`更改为`TreeMap`即可。通过进行这四个字符的更改，程序将从相同的命令行生成以下输出。

```java
8 distinct words:
{be=1, delegate=1, if=1, is=2, it=2, me=1, to=3, up=1}

```

类似地，您可以通过将映射的实现类型更改为`LinkedHashMap`，使程序按照单词首次出现在命令行上的顺序打印频率表。这样做会产生以下输出。

```java
8 distinct words:
{if=1, it=2, is=2, to=3, be=1, up=1, me=1, delegate=1}

```

这种灵活性提供了一个接口为基础的框架强大的例证。

像[`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)和[`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)接口一样，`Map`加强了对`equals`和`hashCode`方法的要求，以便可以比较两个`Map`对象的逻辑相等性，而不考虑它们的实现类型。如果两个`Map`实例表示相同的键-值映射，则它们是相等的。

按照惯例，所有通用的`Map`实现都提供了接受`Map`对象并初始化新`Map`以包含指定`Map`中所有键-值映射的构造函数。这种标准的`Map`转换构造函数与标准的`Collection`构造函数完全类似：它允许调用者创建一个所需实现类型的`Map`，最初包含另一个`Map`中的所有映射，而不管另一个`Map`的实现类型如何。例如，假设您有一个名为`m`的`Map`。以下一行代码创建一个新的`HashMap`，最初包含与`m`相同的所有键-值映射。

```java
Map<K, V> copy = new HashMap<K, V>(m);

```

## Map 接口的批量操作

`clear`操作确切地做了你认为它能做的事情：它从`Map`中删除所有映射。`putAll`操作是`Map`接口的`Collection`接口的`addAll`操作的类比。除了明显的将一个`Map`倒入另一个`Map`的用途外，它还有第二个更微妙的用途。假设一个`Map`用于表示属性-值对的集合；`putAll`操作与`Map`转换构造函数结合使用，提供了一种实现带有默认值的属性映射创建的简洁方法。以下是演示这种技术的静态工厂方法。

```java
static <K, V> Map<K, V> newAttributeMap(Map<K, V>defaults, Map<K, V> overrides) {
    Map<K, V> result = new HashMap<K, V>(defaults);
    result.putAll(overrides);
    return result;
}

```

## 集合视图

`Collection`视图方法允许将`Map`以这三种方式视为`Collection`：

+   `keySet`  包含在`Map`中的键的`Set`。

+   `values`  包含在`Map`中的值的`Collection`。这个`Collection`不是`Set`，因为多个键可以映射到相同的值。

+   `entrySet`  包含在`Map`中的键-值对的`Set`。`Map`接口提供了一个称为`Map.Entry`的小嵌套接口，这个`Set`中的元素的类型。

`Collection`视图提供了遍历`Map`的*唯一*方法。这个例子演示了使用`for-each`结构遍历`Map`中键的标准习语：

```java
for (KeyType key : m.keySet())
    System.out.println(key);

```

并使用`iterator`：

```java
// Filter a map based on some 
// property of its keys.
for (Iterator<Type> it = m.keySet().iterator(); it.hasNext(); )
    if (it.next().isBogus())
        it.remove();

```

遍历值的习语是类似的。以下是遍历键-值对的习语。

```java
for (Map.Entry<KeyType, ValType> e : m.entrySet())
    System.out.println(e.getKey() + ": " + e.getValue());

```

起初，许多人担心这些习语可能会很慢，因为每次调用 `Collection` 视图操作时，`Map` 都必须创建一个新的 `Collection` 实例。放心：`Map` 每次被要求返回给定 `Collection` 视图时都可以返回相同的对象。这正是 `java.util` 中所有 `Map` 实现所做的。

对于所有三个 `Collection` 视图，调用 `Iterator` 的 `remove` 操作会从支持的 `Map` 中删除关联的条目，假设支持的 `Map` 一开始支持元素的移除。这由前面的过滤习语所说明。

使用 `entrySet` 视图，还可以在迭代期间通过调用 `Map.Entry` 的 `setValue` 方法更改与键关联的值（同样，假设 `Map` 一开始支持值的修改）。请注意，这些是*唯一*安全的在迭代期间修改 `Map` 的方式；如果在迭代进行中以任何其他方式修改底层 `Map`，行为是未指定的。

`Collection` 视图支持元素的各种形式的移除  `remove`、`removeAll`、`retainAll` 和 `clear` 操作，以及 `Iterator.remove` 操作。（再次强调，这假设支持的 `Map` 支持元素的移除。）

`Collection` 视图*不支持*在任何情况下添加元素。对于 `keySet` 和 `values` 视图来说是没有意义的，对于 `entrySet` 视图来说也是不必要的，因为支持的 `Map` 的 `put` 和 `putAll` 方法提供了相同的功能。

## 集合视图的花式用法：Map 代数

当应用于 `Collection` 视图时，批量操作（`containsAll`、`removeAll` 和 `retainAll`）是令人惊讶的强大工具。首先，假设你想知道一个 `Map` 是否是另一个的子映射  也就是说，第一个 `Map` 是否包含第二个中的所有键值映射。以下习语可以实现这一点。

```java
if (m1.entrySet().containsAll(m2.entrySet())) {
    ...
}

```

类似地，假设你想知道两个 `Map` 对象是否包含相同键的所有映射。

```java
if (m1.keySet().equals(m2.keySet())) {
    ...
}

```

假设你有一个代表属性-值对集合的 `Map`，以及两个表示必需属性和可允许属性的 `Set`（可允许属性包括必需属性）。以下代码段确定属性映射是否符合这些约束，并在不符合时打印详细的错误消息。

```java
static <K, V> boolean validate(Map<K, V> attrMap, Set<K> requiredAttrs, Set<K>permittedAttrs) {
    boolean valid = true;
    Set<K> attrs = attrMap.keySet();

    if (! attrs.containsAll(requiredAttrs)) {
        Set<K> missing = new HashSet<K>(requiredAttrs);
        missing.removeAll(attrs);
        System.out.println("Missing attributes: " + missing);
        valid = false;
    }
    if (! permittedAttrs.containsAll(attrs)) {
        Set<K> illegal = new HashSet<K>(attrs);
        illegal.removeAll(permittedAttrs);
        System.out.println("Illegal attributes: " + illegal);
        valid = false;
    }
    return valid;
}

```

假设你想知道两个 `Map` 对象共有的所有键。

```java
Set<KeyType>commonKeys = new HashSet<KeyType>(m1.keySet());
commonKeys.retainAll(m2.keySet());

```

类似的习语可以获得共同的值。

到目前为止，所提出的所有习语都是非破坏性的；也就是说，它们不会修改支持的 `Map`。以下是一些会修改的。假设你想要删除一个 `Map` 与另一个 `Map` 共有的所有键值对。

```java
m1.entrySet().removeAll(m2.entrySet());

```

假设你想要从一个 `Map` 中删除所有在另一个中具有映射的键。

```java
m1.keySet().removeAll(m2.keySet());

```

当你开始在同一批量操作中混合键和值时会发生什么？假设你有一个`Map`，`managers`，将公司中的每个员工映射到员工的经理。我们故意对键和值对象的类型保持模糊。只要它们相同就可以。现在假设你想知道所有“个人贡献者”（非经理）是谁。下面的代码片段会告诉你确切想要知道的内容。

```java
Set<Employee> individualContributors = new HashSet<Employee>(managers.keySet());
individualContributors.removeAll(managers.values());

```

假设你想解雇所有直接向某个经理 Simon 汇报的员工。

```java
Employee simon = ... ;
managers.values().removeAll(Collections.singleton(simon));

```

请注意，这种习惯用法使用了`Collections.singleton`，这是一个返回具有单个指定元素的不可变`Set`的静态工厂方法。

一旦你完成了这个操作，你可能会有一堆员工，他们的经理不再为公司工作（如果 Simon 的直接下属本身是经理的话）。下面的代码将告诉你哪些员工的经理不再为公司工作。

```java
Map<Employee, Employee> m = new HashMap<Employee, Employee>(managers);
m.values().removeAll(managers.keySet());
Set<Employee> slackers = m.keySet();

```

这个例子有点棘手。首先，它创建了`Map`的临时副本，并从临时副本中删除所有值为原始`Map`中键的条目。请记住，原始`Map`为每个员工都有一个条目。因此，临时`Map`中剩余的条目包括所有原始`Map`中值不再是员工的条目。因此，临时副本中的键恰好代表我们正在寻找的员工。

这一节中包含的习语还有很多，但列出它们所有将是不切实际和乏味的。一旦你掌握了它，当你需要时想出正确的习语并不那么困难。

## 多重映射

*Multimap*类似于`Map`，但它可以将每个键映射到多个值。Java 集合框架不包括用于多重映射的接口，因为它们并不常用。使用值为`List`实例的`Map`作为多重映射是一件相当简单的事情。下一个代码示例演示了这种技术，它读取一个包含每行一个单词（全部小写）的单词列表，并打印出符合大小标准的所有变位词组。*变位词组*是一组单词，所有这些单词都包含完全相同的字母，但顺序不同。程序在命令行上接受两个参数：（1）字典文件的名称和（2）要打印出的变位词组的最小大小。不打印包含少于指定最小值的单词的变位词组。

有一个标准的技巧用于找到变位词组：对于字典中的每个单词，将单词中的字母按字母顺序排列（即，重新排列单词的字母以字母顺序）并将条目放入一个多重映射中，将按字母顺序排列的单词映射到原始单词。例如，单词*bad*会导致将*abd*映射到*bad*的条目放入多重映射中。稍加思考就会发现，任何给定键映射到的所有单词形成一个变位词组。遍历多重映射中的键并打印出符合大小约束的每个变位词组是一件简单的事情。

`以下程序`是这种技术的一个直接实现。

```java
import java.util.*;
import java.io.*;

public class Anagrams {
    public static void main(String[] args) {
        int minGroupSize = Integer.parseInt(args[1]);

        // Read words from file and put into a simulated multimap
        Map<String, List<String>> m = new HashMap<String, List<String>>();

        try {
            Scanner s = new Scanner(new File(args[0]));
            while (s.hasNext()) {
                String word = s.next();
                String alpha = alphabetize(word);
                List<String> l = m.get(alpha);
                if (l == null)
                    m.put(alpha, l=new ArrayList<String>());
                l.add(word);
            }
        } catch (IOException e) {
            System.err.println(e);
            System.exit(1);
        }

        // Print all permutation groups above size threshold
        for (List<String> l : m.values())
            if (l.size() >= minGroupSize)
                System.out.println(l.size() + ": " + l);
    }

    private static String alphabetize(String s) {
        char[] a = s.toCharArray();
        Arrays.sort(a);
        return new String(a);
    }
}

```

在一个包含 173,000 个单词的字典文件上运行此程序，最小变位词组大小为八时，会产生以下输出。

```java
9: [estrin, inerts, insert, inters, niters, nitres, sinter,
     triens, trines]
8: [lapse, leaps, pales, peals, pleas, salep, sepal, spale]
8: [aspers, parses, passer, prases, repass, spares, sparse,
     spears]
10: [least, setal, slate, stale, steal, stela, taels, tales,
      teals, tesla]
8: [enters, nester, renest, rentes, resent, tenser, ternes,
     treens]
8: [arles, earls, lares, laser, lears, rales, reals, seral]
8: [earings, erasing, gainers, reagins, regains, reginas,
     searing, seringa]
8: [peris, piers, pries, prise, ripes, speir, spier, spire]
12: [apers, apres, asper, pares, parse, pears, prase, presa,
      rapes, reaps, spare, spear]
11: [alerts, alters, artels, estral, laster, ratels, salter,
      slater, staler, stelar, talers]
9: [capers, crapes, escarp, pacers, parsec, recaps, scrape,
     secpar, spacer]
9: [palest, palets, pastel, petals, plates, pleats, septal,
     staple, tepals]
9: [anestri, antsier, nastier, ratines, retains, retinas,
     retsina, stainer, stearin]
8: [ates, east, eats, etas, sate, seat, seta, teas]
8: [carets, cartes, caster, caters, crates, reacts, recast,
     traces]

```

这些单词中有许多看起来有点虚假，但这不是程序的错；它们在字典文件中。这是我们使用的`字典文件`。它是从公共领域的 ENABLE 基准参考词汇列表派生而来。
