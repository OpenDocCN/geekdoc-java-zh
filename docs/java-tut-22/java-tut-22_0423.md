# 如何在对话框中使用模态

> 译文：[`docs.oracle.com/javase/tutorial/uiswing/misc/modality.html`](https://docs.oracle.com/javase/tutorial/uiswing/misc/modality.html)

Java™ SE 6 已解决了在平台早期版本中出现的模态问题。新的模态模型使开发人员能够限定对话框的模态阻止。

在继续使用新的模态模型之前，请查看以下术语：

+   对话框 — 一个带有标题和边框的顶级弹出窗口，通常从用户那里获取某种形式的输入。对话框可以是模态或非模态。有关对话框的更多信息，请参阅对话框概述页面中的如何创建对话框。

+   模态对话框 — 一个会阻止应用程序中其他顶级窗口输入的对话框，除了以该对话框为所有者创建的窗口。模态对话框会捕获窗口焦点，直到它被关闭，通常是响应按钮按下。

+   非模态对话框 — 一个对话框，允许您在显示此对话框时操作其他窗口。

在 Java SE 6 中，模态和非模态对话框的行为已经更改，使其始终出现在所有被阻止的窗口的顶部，而不仅仅是它们的父窗口的顶部。

Java SE 6 支持以下模态类型：

+   **非模态类型** — 非模态对话框在可见时不会阻止任何其他窗口。

+   **文档模态类型** — 文档模态对话框会阻止同一文档的所有窗口，除了其子层次结构的窗口。在此上下文中，文档是共享一个称为文档根的最近祖先窗口的窗口层次结构。

+   **应用程序模态类型** — 应用程序模态对话框会阻止同一应用程序的所有窗口，除了其子层次结构的窗口。如果在浏览器环境中启动了几个小程序，浏览器可以将它们视为单独的应用程序或单个应用程序。此行为取决于实现。

+   **工具包模态类型** — 工具包模态对话框会阻止在同一工具包中运行的所有窗口，除了其子层次结构的窗口。如果启动了几个小程序，它们都将使用相同的工具包。因此，从小程序显示的工具包模态对话框可能会影响其他小程序和嵌入 Java 运行时环境的浏览器实例的所有窗口。

此外，您可以设置模态排除模式：

+   **排除模式** — 任何顶级窗口都可以标记为不被模态对话框阻止。此属性使您可以设置*模态排除*模式。`Dialog.ModalExclusionType`枚举指定可能的模态排除类型。

* * *

**注意：** 新的模态模型不实现系统模态，该模态会在模态对话框激活时阻止显示在桌面上的所有应用程序（包括 Java 应用程序）。

* * *

ModalityDemo 示例演示了上述四种模态类型中的前三种。

![展示不同模态类型的四个框架](img/modalityDemo.png)

*此图已经缩小以适应页面。

点击图像以查看其自然大小。

* * *

**试试这个：**

1.  点击启动按钮以使用[Java™ Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html)运行 ModalityDemo（[下载 JDK 7 或更高版本](http://www.oracle.com/technetwork/java/javase/downloads/index.html)）。或者，要自行编译和运行示例，请参考示例索引。![启动 ModalityDemo 应用程序](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ModalityDemoProject/ModalityDemo.jnlp)

1.  将出现以下对话框：

    +   Book 1（父框架）

    +   Book 2（父框架）

    +   反馈（父框架）

    +   经典（排除的框架）

1.  切换到 Book 1 框架并为该书选择传记标题，然后选择确定。

1.  传记标题将显示在对话框的标题中。在文本字段中输入名称，例如 - “约翰”。

1.  切换到 Book 1 框架并将标题更改为 Funny Tale，然后选择确定。由于输入名称的对话框是*无模态*的，你可以轻松切换到其*父*框架。

    * * *

    **注意：** 无模态对话框标题已更改为 Funny Tale。

    * * *

1.  在无模态对话框中选择确定。

1.  有趣的故事文档模态对话框出现。

1.  在文本字段中输入一些文本。注意它是由你在无模态对话框中输入的名称签名的。

1.  切换到无模态对话框，尝试更改你的名字。你无法这样做，因为文档模态对话框会阻止其父层次结构中的所有窗口。

1.  对于 Book 2 的父框架执行相同的操作序列（步骤 3 - 9）。

1.  尝试切换到不同的对话框。你会注意到你可以切换到经典框架或反馈框架，以及 Book 1 框架或 Book 2 框架的对话框。

1.  切换到反馈父框架。选择评价自己。

1.  确认对话框将会出现。尝试切换到不同的对话框。你只能切换到经典对话框，因为标准确认对话框是一个应用程序模态对话框，它会阻止同一应用程序的所有窗口。然而，你会注意到你可以在经典框架中选择你最喜欢的古典作家。这个框架是通过使用`APPLICATION_EXCLUDE`模态排除类型创建的，它可以防止所有顶层窗口被任何应用程序模态对话框阻止。

* * *

以下代码片段显示了如何创建不同模态类型的对话框：

```java
//The Book 1 parent frame
f1 = new JFrame("Book 1 (parent frame)");

...

//The modeless dialog box
d2 = new JDialog(f1);

...

//The document-modal dialog box
d3 = new JDialog(d2, "", Dialog.ModalityType.DOCUMENT_MODAL);

...

        //The Book2 parent frame
f4 = new JFrame("Book 2 (parent frame)");

...

//The modeless dialog box
d5 = new JDialog(f4);

...

//The document-modality dialog box
d6 = new JDialog(d5, "", Dialog.ModalityType.DOCUMENT_MODAL);

...

//The excluded frame
f7 = new JFrame("Classics (excluded frame)");
f7.setModalityExclusionType(Dialog.ModalExclusionType.APPLICATION_EXCLUDED);

...

//The Feedback parent frame and Confirm Dialog box
f8 = new JFrame("Feedback (parent frame)");
...

JButton b8 = new JButton("Rate yourself");
b8.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        JOptionPane.showConfirmationDialog(null,
                                           "I really like my book",
                                           "Question (application-modal dialog)", 
                                           JOptionPane.Yes_NO_OPTION,
                                           JOptionPane.QUESTION_MESSAGE); 
    }
});

```

在`ModalityDemo.java`文件中找到演示的完整代码。

在 Java SE 6 中，您可以创建一个没有父级的文档模态对话框。因为`Dialog`类是`Window`类的子类，如果它没有所有者，`Dialog`实例会自动成为文档的根。因此，如果这样的对话框是文档模态的，它的阻止范围为空，它的行为就像是一个非模态对话框。

## 模态 API

`JDialog`类的构造函数使您能够创建不同模态类型的对话框。

| 构造函数 | 目的 |
| --- | --- |
| [JDialog(Dialog owner)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-) | 创建一个具有指定`Dialog`所有者但没有标题的非模态对话框。 |
| [JDialog(Dialog owner, boolean modal)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-boolean-) | 创建一个具有指定`Dialog`所有者和模态性的对话框。 |
| [JDialog(Dialog owner, String title)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-java.lang.String-) | 创建一个具有指定`Dialog`所有者和标题的非模态对话框。 |
| [JDialog(Dialog owner, String title, boolean modal)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-java.lang.String-boolean-) | 创建一个具有指定`Dialog`所有者、标题和模态性的对话框。 |
| [JDialog(Dialog owner, String title, boolean modal, GraphicsConfiguration gc)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-java.lang.String-boolean-java.awt.GraphicsConfiguration-) | 创建一个具有指定`Dialog`所有者、标题、模态性和图形配置的对话框。 |
| [JDialog(Frame owner)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Frame-) | 创建一个具有指定`Frame`所有者但没有标题的非模态对话框。如果所有者的值为 null，则会将一个共享的隐藏框架设置为对话框的所有者。 |
| [JDialog(Window owner, String title, Dialog.ModalityType modalityType)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Window-java.lang.String-java.awt.Dialog.ModalityType-) | 使用指定的`Window`所有者、标题和模态性创建对话框框。 |

以下表格列出了从[`java.awt.Dialog`](https://docs.oracle.com/javase/8/docs/api/java/awt/Dialog.html)类继承的方法。

| 方法 | 目的 |
| --- | --- |
| [获取模态类型](https://docs.oracle.com/javase/8/docs/api/java/awt/Dialog.html#getModalityType--) | 返回此对话框框的模态类型。 |
| [设置模态类型](https://docs.oracle.com/javase/8/docs/api/java/awt/Dialog.html#setModalityType-java.awt.Dialog.ModalityType-) | 设置此对话框框的模态类型。查看[模态类型](https://docs.oracle.com/javase/8/docs/api/java/awt/Dialog.ModalityType.html)以获取可能的模态类型。如果给定的模态类型不受支持，则使用`MODELESS`类型。要确保已设置模态类型，请在调用此方法后调用`getModalityType()`方法。 |

## 使用模态 API 的示例

以下表格列出了在对话框中使用模态性的示例。

| 示例 | 描述位置 | 备注 |
| --- | --- | --- |
| `模态演示` | 本节 | 创建不同模态类型的对话框，演示这些类型的作用域阻塞。 |
