# 课程：类

> 原文：[`docs.oracle.com/javase/tutorial/reflect/class/index.html`](https://docs.oracle.com/javase/tutorial/reflect/class/index.html)

每种类型都是引用类型或基本类型。类、枚举和数组（它们都继承自[`java.lang.Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)）以及接口都是引用类型。引用类型的示例包括[`java.lang.String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)、所有基本类型的包装类，如[`java.lang.Double`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html)、接口[`java.io.Serializable`](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html)和枚举[`javax.swing.SortOrder`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SortOrder.html)。基本类型有一组固定的类型：`boolean`、`byte`、`short`、`int`、`long`、`char`、`float`和`double`。

对于每种类型的对象，Java 虚拟机实例化一个不可变的[`java.lang.Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)实例，该实例提供了用于检查对象的运行时属性的方法，包括其成员和类型信息。[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)还提供了创建新类和对象的能力。最重要的是，它是所有反射 API 的入口点。本课程涵盖了涉及类的最常用的反射操作：

+   检索类对象描述了获取[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)的方法。

+   检查类修饰符和类型展示了如何访问类声明信息。

+   发现类成员说明了如何列出类中的构造函数、字段、方法和嵌套类。

+   故障排除描述了在使用[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)时遇到的常见错误。
