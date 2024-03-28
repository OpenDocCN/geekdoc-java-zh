# 课程：数组和枚举类型

> 原文：[`docs.oracle.com/javase/tutorial/reflect/special/index.html`](https://docs.oracle.com/javase/tutorial/reflect/special/index.html)

从 Java 虚拟机的角度看，数组和枚举类型（或枚举）只是类。许多 [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) 中的方法可以用于它们。反射为数组和枚举提供了一些特定的 API。本课程使用一系列代码示例来描述如何区分这些对象与其他类，并对其进行操作。还将检查各种错误。

## 数组

数组有一个组件类型和一个长度（长度不是类型的一部分）。数组可以整体操作，也可以逐个组件操作。反射为后者提供了 [`java.lang.reflect.Array`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html) 类。

+   识别数组类型 描述了如何确定类成员是否是数组类型的字段

+   创建新数组 演示了如何创建具有简单和复杂组件类型的新数组实例

+   获取和设置数组及其组件 展示了如何访问数组类型的字段以及单独访问数组元素

+   故障排除 包括常见错误和编程误解

## 枚举类型

在反射代码中，枚举类型与普通类非常相似。[`Class.isEnum()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isEnum--) 可以告诉一个 [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) 是否表示一个 `enum`。[`Class.getEnumConstants()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnumConstants--) 可以检索在枚举中定义的枚举常量。[`java.lang.reflect.Field.isEnumConstant()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#isEnumConstant--) 表示一个字段是否是一个枚举类型。

+   检查枚举 演示了如何检索枚举的常量以及任何其他字段、构造函数和方法

+   使用枚举类型获取和设置字段 展示了如何设置和获取具有枚举常量值的字段

+   故障排除 描述了与枚举相关的常见错误
