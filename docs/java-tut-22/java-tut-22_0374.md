# 如何使用编辑窗格和文本窗格

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/components/editorpane.html`](https://docs.oracle.com/javase/tutorial/uiswing/components/editorpane.html)

两个 Swing 类支持样式文本：[`JEditorPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html)及其子类[`JTextPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html)。`JEditorPane`类是 Swing 样式文本组件的基础，并提供了一种机制，通过该机制您可以为自定义文本格式添加支持。如果您需要无样式文本，请改用文本区域。

运行 TextSamplerDemo 可以看到编辑窗格和文本窗格的使用。以下是`TextSamplerDemo`示例的图片。

![提供每个 Swing 文本组件示例的应用程序](img/53a15966ce4b1b9831354a10bee51eb4.png)

单击“启动”按钮以使用[Java™ Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html)运行 TextSamplerDemo（[下载 JDK 7 或更高版本](http://www.oracle.com/technetwork/java/javase/downloads/index.html)）。或者，要自行编译和运行示例，请参考示例索引。

![启动 TextSamplerDemo 应用程序](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TextSamplerDemoProject/TextSamplerDemo.jnlp)

`TextSamplerDemo`示例仅仅是展示编辑窗格和文本窗格功能的开始。然而，右上角的编辑窗格展示了一个方便易用的功能：它显示从 URL 加载的不可编辑的帮助信息。右下角的文本窗格演示了您可以轻松地将图像甚至组件直接嵌入文本窗格中。

* * *

**注意：**

如果您需要一个完整的帮助系统，请查看[javahelp](https://javaee.github.io/javahelp/)项目。

* * *

Swing 文本 API 功能强大且庞大，我们可以专门撰写一本书来介绍如何使用编辑窗格和文本窗格。本节介绍它们的功能，提供使用建议，并指向其他信息来源。

+   使用编辑窗格显示来自 URL 的文本

+   编辑窗格 vs. 文本窗格

+   使用文本窗格的示例

+   编辑窗格和文本窗格 API

+   使用编辑窗格和文本窗格的示例

## 使用编辑窗格显示来自 URL 的文本

在不了解 Swing 文本系统的情况下，您可以完成的任务之一是显示来自 URL 的文本。以下是`TextSamplerDemo.java`中创建的一个不可编辑的编辑窗格，用于显示使用 HTML 标记格式化的文本的代码。

```java
JEditorPane editorPane = new JEditorPane();
editorPane.setEditable(false);
java.net.URL helpURL = TextSamplerDemo.class.getResource(
                                "TextSamplerDemoHelp.html");
if (helpURL != null) {
    try {
        editorPane.setPage(helpURL);
    } catch (IOException e) {
        System.err.println("Attempted to read a bad URL: " + helpURL);
    }
} else {
    System.err.println("Couldn't find file: TextSamplerDemoHelp.html");
}

//Put the editor pane in a scroll pane.
JScrollPane editorScrollPane = new JScrollPane(editorPane);
editorScrollPane.setVerticalScrollBarPolicy(
                JScrollPane.VERTICAL_SCROLLBAR_ALWAYS);
editorScrollPane.setPreferredSize(new Dimension(250, 145));
editorScrollPane.setMinimumSize(new Dimension(10, 10));

```

代码使用默认构造函数创建编辑窗格，然后调用`setEditable(false)`以防止用户编辑文本。接下来，代码创建`URL`对象，并使用它调用`setPage`方法。

`setPage`方法打开 URL 指向的资源，并确定文本的格式（在示例中为 HTML）。如果已知文本格式，编辑窗格将使用在 URL 找到的文本进行初始化。标准编辑窗格可以理解纯文本、HTML 和 RTF。请注意，页面可能会异步加载，这会使 GUI 保持响应性，但意味着您不应该指望在调用`setPage`返回后数据已完全加载。

## 编辑窗格与文本窗格

要使用编辑窗格和文本窗格，您需要了解文本系统，该系统在文本组件功能中有描述。关于编辑窗格和文本窗格的几个事实分散在该部分中。在这里，我们再次列出这些事实并提供更多细节。这里的信息应该帮助您了解编辑窗格和文本窗格之间的区别，以及何时使用哪种。

+   通过`setPage`方法，可以轻松地从 URL 加载文本到编辑窗格或文本窗格中。`JEditorPane`类还提供了构造函数，让您可以从 URL 初始化编辑窗格。`JTextPane`类没有这样的构造函数。参见使用编辑窗格显示来自 URL 的文本的示例，该示例使用此功能加载一个不可编辑的编辑窗格，其中包含 HTML 格式的文本。

    使用`setPage`方法时要注意文档和编辑器工具包可能会发生变化。例如，如果编辑窗格包含纯文本（默认情况下），而您将其加载为 HTML，则文档将更改为`HTMLDocument`实例，编辑器工具包将更改为`HTMLEditorKit`实例。如果您的程序使用`setPage`方法，请确保调整代码以适应窗格文档和编辑器工具包实例的可能更改（在新文档上重新注册文档监听器等）。

+   默认情况下，编辑窗格知道如何读取、写入和编辑纯文本、HTML 和 RTF 文本。文本窗格继承了这种能力，但施加了一定的限制。文本窗格要求其文档实现`StyledDocument`接口。`HTMLDocument`和`RTFDocument`都是`StyledDocuments`，因此在文本窗格中 HTML 和 RTF 可以正常工作。但是，如果您将文本窗格加载为纯文本，则文本窗格的文档不是您可能期望的`PlainDocument`，而是`DefaultStyledDocument`。

+   要支持自定义文本格式，需要实现一个可以读取、写入和编辑该格式文本的编辑器工具包。然后调用`registerEditorKitForContentType`方法将您的工具包注册到`JEditorPane`类中。通过这种方式注册编辑器工具包，程序中的所有编辑窗格和文本窗格都能够读取、写入和编辑新格式。但是，如果新的编辑器工具包不是`StyledEditorKit`，文本窗格将不支持新格式。

+   如前所述，文本窗格需要其文档实现`StyledDocument`接口。Swing 文本包提供了这个接口的默认实现，`DefaultStyledDocument`，这是文本窗格默认使用的文档。文本窗格还需要其编辑器工具包是`StyledEditorKit`的一个实例（或子类）。请注意，`StyleEditorKit`的`read`和`write`方法适用于纯文本。

+   通过它们的样式文档和样式编辑器工具包，文本窗格提供对命名样式和逻辑样式的支持。`JTextPane`类本身包含许多用于处理样式的方法，这些方法只是调用其文档或编辑器工具包中的方法。

+   通过`JTextPane`类提供的 API，您可以在文本窗格中嵌入图像和组件。您也可以在编辑窗格中嵌入图像，但只能通过在 HTML 或 RTF 文件中包含图像来实现。

## 使用文本窗格的示例

这是`TextSamplerDemo`示例中创建和初始化文本窗格的代码。

```java
String[] initString =
        { /* ... * fill array with initial text * ... */ };

String[] initStyles =
        { /* ... * fill array with names of styles * ... */ };

JTextPane textPane = new JTextPane();
StyledDocument doc = textPane.getStyledDocument();
addStylesToDocument(doc);

//Load the text pane with styled text.
try {
    for (int i=0; i < initString.length; i++) {
        doc.insertString(doc.getLength(), initString[i],
                         doc.getStyle(initStyles[i]));
    }
} catch (BadLocationException ble) {
    System.err.println("Couldn't insert initial text into text pane.");
}

```

简而言之，这段代码将初始文本硬编码到一个数组中，并创建并将几个*样式*  表示不同段落和字符格式的对象  硬编码到另一个数组中。接下来，代码循环遍历这些数组，将文本插入文本窗格，并指定要用于插入文本的样式。

虽然这是一个有趣的例子，简洁地展示了`JTextPane`的几个特性，但“现实世界”中的程序不太可能以这种方式初始化文本窗格。相反，程序会使用编辑窗格保存文档，然后再用它来初始化文本窗格。

## 编辑窗格和文本窗格 API

本节列出了与文本和编辑窗格相关的一些 API。许多[JEditorPane](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html)及其子类[JTextPane](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html)最有用的方法都是从`JTextComponent`类继承的。您可以在文本组件 API 中找到`JTextComponent`的 API 表。还请参阅 The JComponent Class，其中描述了从`JComponent`继承的 API。

JEditorPane 用于显示来自 URL 的文本的 API

| 方法或构造函数 | 描述 |
| --- | --- |
| [JEditorPane(URL)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html#JEditorPane-java.net.URL-) [JEditorPane(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html#JEditorPane-java.lang.String-) | 创建一个加载指定 URL 文本的编辑窗格。 |
| [setPage(URL)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html#setPage-java.net.URL-) [setPage(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html#setPage-java.lang.String-) | 使用指定 URL 加载编辑窗格（或文本窗格）中的文本。 |
| [URL getPage()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html#getPage--) | 获取编辑窗格（或文本窗格）当前页面的 URL。 |

JTextPane API

| 方法或构造函数 | 描述 |
| --- | --- |
| [JTextPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html#JTextPane--) [JTextPane(StyledDocument)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html#JTextPane-javax.swing.text.StyledDocument-) | 创建一个文本窗格。可选参数指定文本窗格的模型。 |
| [StyledDocument getStyledDocument](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html#getStyledDocument--) [setStyledDocument(StyledDocument)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html#setStyledDocument-javax.swing.text.StyledDocument-) | 获取或设置文本窗格的模型。 |

## 使用文本窗格和编辑窗格的示例

要开始使用文本，您可能希望运行这些程序并检查它们的代码，找到与您想要做的类似的内容。

| 示例 | 描述位置 | 备注 |
| --- | --- | --- |
| `TextSamplerDemo` | 使用文本组件 | 使用每个 Swing 文本组件。 |
| `TextComponentDemo` | 文本组件特性 | 提供一个定制的文本窗格。展示了许多文本组件特性，如撤销和重做，文档过滤器，文档监听器，插入符位置变化监听器，以及如何将编辑操作与菜单和快捷键关联起来。 |
| `TreeDemo` | 如何使用树形结构 | 使用编辑窗格显示从 HTML 文件加载的帮助信息。 |

学习如何在 JavaFX 中编辑 HTML 文本，参考[使用 JavaFX UI 控件：HTML 编辑器](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/editor.htm) 教程。
