# 方法

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/method.html`](https://docs.oracle.com/javase/tutorial/reflect/member/method.html)

*方法* 包含可调用的可执行代码。方法是继承的，在非反射代码中，编译器强制执行重载、覆盖和隐藏等行为。相比之下，反射代码使得方法选择可以限制在特定类中而不考虑其超类。可以访问超类方法，但可以确定它们的声明类；这在没有反射的情况下是不可能通过编程方式发现的，这是许多微妙错误的根源。

[`java.lang.reflect.Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html) 类提供了访问有关方法修饰符、返回类型、参数、注解和抛出异常的信息的 API。它还可以用于调用方法。以下部分涵盖了这些主题：

+   获取方法类型信息展示了如何枚举在类中声明的方法并获取类型信息

+   获取方法参数的名称展示了如何检索方法或构造函数的参数的名称和其他信息

+   检索和解析方法修饰符描述了如何访问和解码与方法相关的修饰符和其他信息

+   调用方法演示了如何执行一个方法并获取其返回值

+   故障排除 涵盖了在查找或调用方法时遇到的常见错误
