# 问题和练习：聚合操作

> 原文：[`docs.oracle.com/javase/tutorial/collections/streams/QandE/questions.html`](https://docs.oracle.com/javase/tutorial/collections/streams/QandE/questions.html)

## 问题

1.  一系列的聚合操作被称为 ___。

1.  每个流水线包含零个或多个 ___ 操作。

1.  每个流水线以一个 ___ 操作结束。

1.  什么样的操作以另一个流作为输出？

1.  描述`forEach`聚合操作与增强的`for`语句或迭代器之间的一个区别。

1.  真或假：流类似于集合，因为它是一个存储元素的数据结构。

1.  在这段代码中识别中间操作和终端操作：

    ```java
    double average = roster
        .stream()
        .filter(p -> p.getGender() == Person.Sex.MALE)
        .mapToInt(Person::getAge)
        .average()
        .getAsDouble();

    ```

1.  代码`p -> p.getGender() == Person.Sex.MALE`是什么的一个例子？

1.  代码`Person::getAge`是什么的一个例子？

1.  将流的内容组合并返回一个值的终端操作被称为什么？

1.  `Stream.reduce`方法和`Stream.collect`方法之间的一个重要区别是什么？

1.  如果你想处理一个包含姓名的流，提取男性姓名，并将它们存储在一个新的`List`中，那么`Stream.reduce`或`Stream.collect`是最合适的操作？

1.  真或假：聚合操作使得可以在非线程安全的集合中实现并行性。

1.  流通常是串行的，除非另有规定。如何请求以并行方式处理流？

## 练习

1.  将以下增强的`for`语句编写为使用 lambda 表达式的流水线。提示：使用`filter`中间操作和`forEach`终端操作。

    ```java
    for (Person p : roster) {
        if (p.getGender() == Person.Sex.MALE) {
            System.out.println(p.getName());
        }
    }

    ```

1.  将以下代码转换为一个使用 lambda 表达式和聚合操作而不是嵌套`for`循环的新实现。提示：创建一个依次调用`filter`、`sorted`和`collect`操作的流水线。

    ```java
    List<Album> favs = new ArrayList<>();
    for (Album a : albums) {
        boolean hasFavorite = false;
        for (Track t : a.tracks) {
            if (t.rating >= 4) {
                hasFavorite = true;
                break;
            }
        }
        if (hasFavorite)
            favs.add(a);
    }
    Collections.sort(favs, new Comparator<Album>() {
                               public int compare(Album a1, Album a2) {
                                   return a1.name.compareTo(a2.name);
                               }});

    ```

检查你的答案。
