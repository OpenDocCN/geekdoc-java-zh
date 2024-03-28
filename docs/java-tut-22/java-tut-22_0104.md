# 数字总结

> 原文：[`docs.oracle.com/javase/tutorial/java/data/numbersummary.html`](https://docs.oracle.com/javase/tutorial/java/data/numbersummary.html)

您可以使用包装类之一 - `Byte`、`Double`、`Float`、`Integer`、`Long` 或 `Short` - 将原始类型的数字包装在对象中。在必要时，Java 编译器会自动为您包装（装箱）原始类型，并在必要时对其进行拆箱。

`Number` 类包括常量和有用的类方法。`MIN_VALUE` 和 `MAX_VALUE` 常量包含该类型对象可以包含的最小值和最大值。`byteValue`、`shortValue` 和类似方法将一个数值类型转换为另一个数值类型。`valueOf` 方法将字符串转换为数字，`toString` 方法将数字转换为字符串。

要格式化包含数字的字符串以进行输出，可以使用 `PrintStream` 类中的 `printf()` 或 `format()` 方法。或者，您可以使用 `NumberFormat` 类使用模式自定义数字格式。

`Math` 类包含各种执行数学函数的类方法，包括指数、对数和三角函数方法。`Math` 还包括基本算术函数，如绝对值和四舍五入，以及用于生成随机数的 `random()` 方法。
