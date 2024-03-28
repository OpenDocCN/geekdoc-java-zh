# 变量总结

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/variablesummary.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variablesummary.html)

Java 编程语言在其术语中同时使用“字段”和“变量”。实例变量（非静态字段）对于类的每个实例都是唯一的。类变量（静态字段）是用`static`修饰的字段；无论类被实例化多少次，类变量都只有一个副本。局部变量在方法内部存储临时状态。参数是向方法提供额外信息的变量；局部变量和参数始终被分类为“变量”（而不是“字段”）。在命名字段或变量时，有一些规则和约定您应该（或必须）遵循。

八种基本数据类型分别为：byte、short、int、long、float、double、boolean 和 char。[`java.lang.String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) 类表示字符串。编译器会为上述类型的字段分配一个合理的默认值；对于局部变量，永远不会分配默认值。字面值是固定值的源代码表示。数组是一个容器对象，它保存了单一类型的固定数量的值。数组的长度在创建数组时确定，创建后其长度是固定的。
