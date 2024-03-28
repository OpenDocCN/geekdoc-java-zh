# 构造函数

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/ctor.html`](https://docs.oracle.com/javase/tutorial/reflect/member/ctor.html)

*构造函数* 用于创建一个属于类的实例的对象。通常在调用方法或访问字段之前执行初始化类所需的操作。构造函数不会被继承。

与方法类似，反射提供了 API 来发现和检索类的构造函数，并获取声明信息，如修饰符、参数、注解和抛出的异常。还可以使用指定的构造函数创建类的新实例。在处理构造函数时使用的关键类是[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) 和 [`java.lang.reflect.Constructor`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html)。涵盖了涉及构造函数的常见操作的以下部分：

+   查找构造函数 说明了如何检索具有特定参数的构造函数

+   检索和解析构造函数修饰符 展示了如何获取构造函数声明的修饰符以及有关构造函数的其他信息

+   创建新的类实例 展示了如何通过调用其构造函数实例化对象的实例

+   故障排除 描述了在查找或调用构造函数时可能遇到的常见错误
