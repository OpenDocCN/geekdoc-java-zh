# 字段

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/field.html`](https://docs.oracle.com/javase/tutorial/reflect/member/field.html)

一个*字段*是一个具有关联值的类、接口或枚举。[`java.lang.reflect.Field`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html)类中的方法可以检索有关字段的信息，比如它的名称、类型、修饰符和注解。（检查类修饰符和类型章节中的类课程描述了如何检索注解。）还有一些方法可以实现对字段值的动态访问和修改。这些任务在以下章节中介绍：

+   获取字段类型描述了如何获取字段的声明类型和泛型类型

+   检索和解析字段修饰符展示了如何获取字段声明的部分，比如`public`或`transient`

+   获取和设置字段值说明了如何访问字段值

+   故障排除描述了可能导致混淆的一些常见编码错误

当编写一个应用程序，比如一个类浏览器时，找出哪些字段属于特定类可能会很有用。通过调用[`Class.getFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getFields--)来识别类的字段。[`getFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getFields--)方法返回一个包含每个可访问的公共字段的[`Field`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html)对象数组。

如果一个公共字段是以下任一成员，则可以访问它：

+   这个类

+   这个类的一个超类

+   由这个类实现的接口

+   由这个类实现的接口扩展的一个接口

一个字段可以是一个类（实例）字段，比如[`java.io.Reader.lock`](https://docs.oracle.com/javase/8/docs/api/java/io/Reader.html#lock)，一个静态字段，比如[`java.lang.Integer.MAX_VALUE`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#MAX_VALUE)，或一个枚举常量，比如[`java.lang.Thread.State.WAITING`](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.State.html#WAITING)。
