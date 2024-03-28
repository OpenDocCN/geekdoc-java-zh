# Bean 持久性

> 原文：[`docs.oracle.com/javase/tutorial/javabeans/advanced/persistence.html`](https://docs.oracle.com/javase/tutorial/javabeans/advanced/persistence.html)

当 bean 的属性、字段和状态信息被保存到存储中并从中检索时，bean 具有持久性的属性。组件模型提供了一种持久性机制，使组件的状态能够被存储在非易失性位置以供以后检索。

使持久性成为可能的机制称为 *序列化*。对象序列化意味着将对象转换为数据流并将其写入存储。任何使用该 bean 的小程序、应用程序或工具都可以通过反序列化来“重建”它。然后对象将恢复到其原始状态。

例如，Java 应用程序可以在 Microsoft Windows 机器上序列化一个 Frame 窗口，然后将序列化文件通过电子邮件发送到 Solaris 机器，然后 Java 应用程序可以将 Frame 窗口恢复到在 Microsoft Windows 机器上存在的确切状态。

任何使用该 bean 的小程序、应用程序或工具都可以通过 *反序列化* 来“重建”它。

所有的 bean 都必须持久化。为了持久化，您的 bean 必须通过实现 [`java.io.Serializable`](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html)（在 API 参考文档中）接口或 [`java.io.Externalizable`](https://docs.oracle.com/javase/8/docs/api/java/io/Externalizable.html)（在 API 参考文档中）接口来支持序列化。这些接口为您提供了自动序列化和定制序列化的选择。如果类的继承层次结构中的任何类实现了 `Serializable` 或 `Externalizable`，那么该类就是可序列化的。

## 可序列化的类

只要该类或父类实现了 `java.io.Serializable` 接口，任何类都可以序列化。支持序列化的示例类包括 `Component`、`String`、`Date`、`Vector` 和 `Hashtable`。因此，`Component` 类的任何子类，包括 `Applet`，都可以被序列化。不支持序列化的显著类包括 `Image`、`Thread`、`Socket` 和 `InputStream`。尝试序列化这些类型的对象将导致 `NotSerializableException`。

Java 对象序列化 API 自动将大多数 Serializable 对象的字段序列化到存储流中。这包括原始类型、数组和字符串。API 不会序列化或反序列化被标记为 transient 或 static 的字段。

## 控制序列化

您可以控制 bean 所经历的序列化级别。控制序列化的三种方式是：

+   通过 `Serializable` 接口实现的自动序列化。Java 序列化软件将整个对象序列化，除了 transient 和 static 字段。

+   定制序列化。通过使用 `transient`（或 `static`）修饰符标记要排除的字段，可以选择性地排除不想序列化的字段。

+   自定义文件格式，由`Externalizable`接口及其两个方法实现。Beans 以特定文件格式编写。

## 默认序列化：Serializable 接口

`Serializable`接口通过使用 Java 对象序列化工具提供自动序列化。`Serializable`不声明任何方法；它充当标记，告诉对象序列化工具你的 bean 类是可序列化的。将你的类标记为`Serializable`意味着你告诉 Java 虚拟机（JVM）你已经确保你的类将与默认序列化一起使用。以下是与`Serializable`接口一起使用的一些重要点：

+   实现`Serializable`的类必须能够访问超类的*无参数构造函数*。当对象从`.ser`文件中“重建”时，将调用此构造函数。

+   如果在超类中已经实现了`Serializable`，那么你不需要在你的类中实现它。

+   所有字段（除了静态和瞬态字段）都会被序列化。使用`transient`修饰符指定不想序列化的字段，并指定不可序列化的类。

## 使用`transient`关键字进行选择性序列化

要在`Serializable`对象中排除字段的序列化，请使用`transient`修饰符标记字段。

```java
transient int status;

```

默认序列化不会序列化`transient`和`static`字段。

## 选择性序列化：writeObject 和 readObject

如果你的可序列化类包含以下两种方法之一（签名必须完全相同），则默认序列化将不会发生。

```java
private void writeObject(java.io.ObjectOutputStream out)
    throws IOException;
private void readObject(java.io.ObjectInputStream in)
    throws IOException, ClassNotFoundException;

```

通过编写自己的`writeObject`和`readObject`方法的实现，可以控制更复杂对象的序列化。当需要对默认序列化无法处理的对象进行序列化，或者需要向序列化流添加不是对象数据成员的数据时，请实现`writeObject`。实现`readObject`以重建使用`writeObject`写入的数据流。

## Externalizable 接口

当需要完全控制 bean 的序列化时（例如，写入和读取特定文件格式时），请使用`Externalizable`接口。要使用`Externalizable`接口，你需要实现两个方法：`readExternal`和`writeExternal`。实现`Externalizable`的类必须有一个无参数构造函数。
