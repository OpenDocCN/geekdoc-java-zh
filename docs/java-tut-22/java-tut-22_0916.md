# 枚举类型

> 原文：[`docs.oracle.com/javase/tutorial/reflect/special/enum.html`](https://docs.oracle.com/javase/tutorial/reflect/special/enum.html)

*枚举*是一种语言构造，用于定义类型安全的枚举，当需要固定一组命名值时可以使用。所有枚举隐式扩展 [`java.lang.Enum`](https://docs.oracle.com/javase/8/docs/api/java/lang/Enum.html)。枚举可以包含一个或多个*枚举常量*，这些常量定义了枚举类型的唯一实例。枚举声明定义了一个*枚举类型*，与类非常相似，可以具有字段、方法和构造函数等成员（有一些限制）。

由于枚举是类，反射不需要定义一个显式的`java.lang.reflect.Enum`类。枚举特定的反射 API 只有 [`Class.isEnum()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isEnum--)、[`Class.getEnumConstants()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnumConstants--) 和 [`java.lang.reflect.Field.isEnumConstant()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#isEnumConstant--)。涉及枚举的大多数反射操作与任何其他类或成员相同。例如，枚举常量被实现为枚举上的`public static final`字段。以下部分展示了如何在枚举中使用 [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) 和 [`java.lang.reflect.Field`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html)。

+   检查枚举 说明了如何检索枚举的常量以及任何其他字段、构造函数和方法

+   使用枚举类型获取和设置字段 展示了如何使用枚举常量值设置和获取字段

+   故障排除描述了与枚举相关的常见错误

有关枚举的介绍，请参阅 枚举类型 课程。
