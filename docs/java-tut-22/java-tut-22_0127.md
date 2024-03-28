# 下界通配符

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/lowerBounded.html`](https://docs.oracle.com/javase/tutorial/java/generics/lowerBounded.html)

上界通配符 部分显示，上界通配符将未知类型限制为特定类型或该类型的子类型，并使用 `extends` 关键字表示。类似地，*下界* 通配符将未知类型限制为特定类型或该类型的 *超类型*。

使用通配符字符（'`?`'）表示下界通配符，后跟 `super` 关键字，再跟着其 *下界*：`<? super A>`。

* * *

**注意：** 你可以为通配符指定上界，也可以指定下界，但不能同时指定两者。

* * *

假设你想编写一个将 `Integer` 对象放入列表的方法。为了最大限度地提高灵活性，你希望该方法适用于 `List<Integer>`、`List<Number>` 和 `List<Object>` — 任何可以容纳 `Integer` 值的东西。

要编写适用于 `Integer` 列表和 `Integer` 的超类型（如 `Integer`、`Number` 和 `Object`）的方法，你应该指定 `List<? super Integer>`。术语 `List<Integer>` 比 `List<? super Integer>` 更具限制性，因为前者仅匹配类型为 `Integer` 的列表，而后者匹配任何是 `Integer` 超类型的列表。

以下代码将数字 1 到 10 添加到列表的末尾：

```java
public static void addNumbers(List<? super Integer> list) {
    for (int i = 1; i <= 10; i++) {
        list.add(i);
    }
}

```

通配符使用指南 部分提供了何时使用上界通配符和何时使用下界通配符的指导。
