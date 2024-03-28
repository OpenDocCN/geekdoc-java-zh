# 创建和使用类和对象摘要

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/summaryclasses.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/summaryclasses.html)

类声明命名类并在大括号之间封装类体。类名可以由修饰符前置。类体包含类的字段、方法和构造函数。类使用字段来包含状态信息，并使用方法来实现行为。初始化类的新实例的构造函数使用类的名称，并且看起来像没有返回类型的方法。

您可以通过在声明中使用访问修饰符（如`public`）来以相同的方式控制对类和成员的访问。

通过在成员声明中使用`static`关键字来指定类变量或类方法。未声明为`static`的成员隐式地是实例成员。类变量由类的所有实例共享，并且可以通过类名以及实例引用访问。类的实例会获得每个实例变量的自己的副本，必须通过实例引用访问。

通过使用`new`运算符和构造函数，您可以从类创建对象。`new`运算符返回一个对创建的对象的引用。您可以将引用分配给变量或直接使用它。

可以通过使用限定名称来引用在声明它们的类之外的代码可访问的实例变量和方法。实例变量的限定名称如下所示：

```java
*objectReference.variableName*

```

方法的限定名称如下所示：

```java
*objectReference.methodName(argumentList)*

```

或：

```java
*objectReference.methodName()*

```

垃圾收集器会自动清理未使用的对象。如果程序不再持有对对象的引用，则该对象将被视为未使用。您可以通过将持有引用的变量设置为`null`来显式丢弃引用。
