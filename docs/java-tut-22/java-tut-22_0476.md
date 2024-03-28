# 使用和创建 DataFlavor

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/dataflavor.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/dataflavor.html)

[`DataFlavor`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html)类允许你指定数据的内容类型。在从`importData`方法中获取数据时，你需要指定一个`DataFlavor`。有几种预定义的类型供你使用：

+   [`imageFlavor`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#imageFlavor)代表`java.awt.Image`格式的数据。在拖动图像数据时使用。

+   [`stringFlavor`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#stringFlavor)代表最基本的文本形式数据 — `java.lang.String`。这是大多数应用程序中最常用的数据类型。

+   [`javaFileListFlavor`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#javaFileListFlavor)代表以`java.util.List`格式表示的`java.io.File`对象。这对于拖动文件的应用程序非常有用，比如在顶级拖放课程中讨论的`TopLevelTransferHandler`示例。

对于大多数应用程序，这就是你需要了解的关于数据类型的全部内容。然而，如果你需要除了这些预定义类型之外的类型，你可以创建自己的类型。如果你创建了一个自定义组件并希望它参与数据传输，你将需要创建一个自定义数据类型。指定数据类型的构造函数是[`DataFlavor(Class, String)`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#DataFlavor-java.lang.Class-java.lang.String-)。例如，为`java.util.ArrayList`类创建一个数据类型：

```java
new DataFlavor(ArrayList.class, "ArrayList");

```

要为整数数组创建一个数据类型：

```java
new DataFlavor(int[].class, "Integer Array");

```

使用这种机制传输数据使用了`Object`序列化，因此你用于传输数据的类必须实现`Serializable`接口，以及与之一起序列化的任何内容。如果不是所有内容都是可序列化的，你将在拖放或复制到剪贴板时看到`NotSerializableException`。

使用`DataFlavor(Class, String)`构造函数创建数据类型允许你在应用程序之间传输数据，包括本地应用程序。如果你想创建一个只在应用程序内部传输数据的数据类型，可以使用[``javaJVMLocalObjectMimeType``](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#javaJVMLocalObjectMimeType)和[``DataFlavor(String)``](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#DataFlavor-java.lang.String-)构造函数。例如，要指定一个从`JColorChooser`仅在你的应用程序内传输颜色的数据类型，你可以使用这段代码：

```java
String colorType = DataFlavor.javaJVMLocalObjectMimeType +
                   ";class=java.awt.Color";
DataFlavor colorFlavor = new DataFlavor(colorType);

```

为一个只在你的应用程序中起作用的`ArrayList`创建一个数据类型：

```java
new DataFlavor(DataFlavor.javaJVMLocalObjectMimeType +
               ";class=java.util.ArrayList");

```

要为整数数组创建一个数据类型：

```java
new DataFlavor(DataFlavor.javaJVMLocalObjectMimeType +
               ";class=\"" + int[].class.getName() + "\"");

```

包含特殊字符的 MIME 类型，比如**[**或**;**，必须将这些字符用引号括起来。

一个`Transferable`可以被实现以支持多种风味。例如，你可以同时使用本地和序列化风味，或者你可以同时使用两种形式的相同数据，比如`ArrayList`和整数数组风味，或者你可以创建一个接受不同类型数据的`TransferHandler`，比如颜色和文本。

当你创建一个`DataFlavors`数组以从`Transferable`的[`getTransferDataFlavors`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/Transferable.html#getTransferDataFlavors--)方法返回时，风味应按照首选顺序插入，最首选的风味应出现在数组的元素 0 位置。一般来说，首选顺序是从最丰富或最复杂的数据形式到简单集合的形式 — 最有可能被其他对象理解的形式。
