# 可序列化对象

> 原文：[`docs.oracle.com/javase/tutorial/jndi/objects/serial.html`](https://docs.oracle.com/javase/tutorial/jndi/objects/serial.html)

*序列化* 一个对象意味着将其状态转换为字节流，以便可以将字节流还原为对象的副本。如果其类或其任何超类实现了 `java.io.Serializable` 接口或其子接口 `java.io.Externalizable`，则 Java 对象是 *可序列化* 的。*反序列化* 是将对象的序列化形式转换回对象的过程。

例如，`java.awt.Button` 类实现了 `Serializable` 接口，因此您可以序列化一个 `java.awt.Button` 对象并将该序列化状态存储在文件中。稍后，您可以读取序列化状态并将其反序列化为一个 `java.awt.Button` 对象。

Java 平台规定了可序列化对象序列化的默认方式。一个（Java）类可以覆盖这种默认序列化，并定义其自己的序列化对象的方式。[对象序列化规范](https://docs.oracle.com/javase/8/docs/technotes/guides/serialization/index.html)详细描述了对象序列化。

当对象被序列化时，用于标识其类的信息被记录在序列化流中。然而，类的定义（"类文件"）本身并未记录。反序列化对象的系统有责任确定如何定位和加载必要的类文件。例如，一个 Java 应用程序可能在其类路径中包含一个包含序列化对象的类文件的 JAR 文件，或者通过使用存储在目录中的信息加载类定义，如本课程后面解释的那样。

## 绑定可序列化对象

如果底层服务提供程序支持该操作，您可以将可序列化对象存储在目录中，就像 Oracle 的 LDAP 服务提供程序一样。

以下示例调用 [`Context.bind`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#bind-javax.naming.Name-java.lang.Object-) 将 AWT 按钮绑定到名称 `"cn=Button"`。要将属性与新绑定关联起来，您可以使用 [`DirContext.bind`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#bind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-)。要覆盖现有绑定，使用 [`Context.rebind`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#rebind-javax.naming.Name-java.lang.Object-) 和 [`DirContext.rebind`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#rebind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-)。

```java
// Create the object to be bound
Button b = new Button("Push me");

// Perform the bind
ctx.bind("cn=Button", b);

```

您可以使用 [`Context.lookup`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#lookup-javax.naming.Name-) 读取对象，如下所示。

```java
// Check that it is bound
Button b2 = (Button)ctx.lookup("cn=Button");
System.out.println(b2);

```

运行 `此示例` 会产生以下输出。

```java
# java SerObj
java.awt.Button[button0,0,0,0x0,invalid,label=Push me]

```

## 指定代码库

* * *

**注意：** 这里描述的程序适用于在遵循[RFC 2713](http://www.ietf.org/rfc/rfc2713.txt)中定义的模式的目录服务中绑定可序列化对象。这些程序可能不适用于支持将可序列化对象与指定代码库绑定的其他命名和目录服务。

* * *

当一个序列化对象像前面的例子中所示绑定在目录中时，从目录中读取序列化对象的应用程序必须能够访问必要的类定义以对对象进行反序列化。

或者，你可以在目录中记录一个*代码库*，无论是在绑定对象时还是随后通过使用[`DirContext.modifyAttributes`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#modifyAttributes-javax.naming.Name-int-javax.naming.directory.Attributes-)添加属性。你可以使用任何属性来记录这个代码库，并让你的应用程序从目录中读取该属性并适当使用它。或者你可以使用在中指定的`"javaCodebase"`属性。在后一种情况下，Oracle 的 LDAP 服务提供者将自动使用该属性根据需要加载类定义。`"javaCodebase"`应包含代码库目录或 JAR 文件的 URL。如果代码库包含多个 URL，则每个 URL 必须用空格字符分隔。

以下示例类似于绑定`java.awt.Button`的示例。不同之处在于它使用了一个用户定义的`Serializable`类，``Flower``，并提供了一个包含`Flower`类定义位置的`"javaCodebase"`属性。以下是执行绑定的代码。

```java
String codebase = ...;

// Create the object to be bound
Flower f = new Flower("rose", "pink");

// Perform the bind and specify the codebase
ctx.bind("cn=Flower", f, new BasicAttributes("javaCodebase", codebase));

```

当你运行`这个例子`时，你必须提供`Flower.class`类文件安装位置的 URL。例如，如果`Flower.class`安装在 Web 服务器`web1`上，目录为`example/classes`，那么你可以按照以下方式运行这个例子。

```java
# java SerObjWithCodebase http://web1/example/classes/
pink rose

```

之后，你可以从类路径中删除`Flower.class`并运行任何查找或列出此对象的程序，而无需直接引用`Flower`类。如果你的程序直接引用`Flower`，那么你必须使其类文件可供编译和执行。
