# 数组

> 原文：[`docs.oracle.com/javase/tutorial/reflect/special/array.html`](https://docs.oracle.com/javase/tutorial/reflect/special/array.html)

一个*数组*是引用类型的对象，包含固定数量的相同类型的组件；数组的长度是不可变的。创建数组的实例需要知道长度和组件类型。每个组件可以是原始类型（如`byte`、`int`或`double`），引用类型（如[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)、[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)或[`java.nio.CharBuffer`](https://docs.oracle.com/javase/8/docs/api/java/nio/CharBuffer.html)），或者是数组。多维数组实际上只是包含数组类型组件的数组。

数组在 Java 虚拟机中实现。数组上的唯一方法是从[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)继承的方法。数组的长度不是其类型的一部分；数组有一个`length`字段，可以通过[`java.lang.reflect.Array.getLength()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#getLength-java.lang.Object-)访问。

反射提供了访问数组类型和数组组件类型、创建新数组以及检索和设置数组组件值的方法。以下各节包括对数组上常见操作的示例：

+   识别数组类型描述了如何确定类成员是否是数组类型的字段

+   创建新数组演示了如何创建具有简单和复杂组件类型的新数组实例

+   获取和设置数组及其组件展示了如何访问数组类型的字段以及单独访问数组元素

+   故障排除涵盖了常见错误和编程误解

所有这些操作都通过[`java.lang.reflect.Array`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html)中的`static`方法支持。
