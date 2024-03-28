# 课程：成员

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/index.html`](https://docs.oracle.com/javase/tutorial/reflect/member/index.html)

反射定义了一个接口[`java.lang.reflect.Member`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Member.html)，该接口由[`java.lang.reflect.Field`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html)、[`java.lang.reflect.Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html)和[`java.lang.reflect.Constructor`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html)实现。这些对象将在本课程中讨论。对于每个成员，本课程将描述相关的 API 以检索声明和类型信息，成员特有的任何操作（例如，设置字段的值或调用方法），以及常见的错误。每个概念将通过代码示例和相关输出进行说明，这些输出近似一些预期的反射用法。

* * *

**注意：** 根据[*Java 语言规范，Java SE 7 版*](https://docs.oracle.com/javase/specs/jls/se7/html/index.html)，类的*成员*是类主体的继承组件，包括字段、方法、嵌套类、接口和枚举类型。由于构造函数不会被继承，因此它们不是成员。这与[`java.lang.reflect.Member`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Member.html)的实现类有所不同。

* * *

## 字段

字段具有类型和值。[`java.lang.reflect.Field`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html)类提供了用于访问类型信息以及在给定对象上设置和获取字段值的方法。

+   获取字段类型 描述了如何获取字段的声明类型和泛型类型

+   检索和解析字段修饰符 展示了如何获取字段声明的部分，如`public`或`transient`

+   获取和设置字段值 说明了如何访问字段的值

+   故障排除 描述了可能导致混淆的一些常见编码错误

## 方法

方法具有返回值、参数，并可能抛出异常。[`java.lang.reflect.Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html)类提供了用于获取参数和返回值的类型信息的方法。它还可以用于在给定对象上调用方法。

+   获取方法类型信息 展示了如何枚举类中声明的方法并获取类型信息

+   获取方法参数的名称 展示了如何检索方法或构造函数的参数的名称和其他信息

+   检索和解析方法修饰符描述了如何访问和解码与方法相关的修饰符和其他信息

+   调用方法说明了如何执行一个方法并获得其返回值

+   故障排除涵盖了在查找或调用方法时遇到的常见错误

## 构造函数

构造函数的反射 API 在[`java.lang.reflect.Constructor`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html)中定义，与方法的 API 类似，但有两个主要例外：首先，构造函数没有返回值；其次，调用构造函数会为给定类创建一个新的对象实例。

+   查找构造函数说明了如何检索具有特定参数的构造函数

+   检索和解析构造函数修饰符展示了如何获取构造函数声明的修饰符以及有关构造函数的其他信息

+   创建新的类实例展示了如何通过调用其构造函数来实例化一个对象的实例

+   故障排除描述了在查找或调用构造函数时可能遇到的常见错误
