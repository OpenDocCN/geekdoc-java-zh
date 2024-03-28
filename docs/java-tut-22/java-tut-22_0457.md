# 默认的拖放支持

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/defaultsupport.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/defaultsupport.html)

从技术上讲，拖放框架支持所有 Swing 组件 —— 数据传输机制内置在每个`JComponent`中。如果您愿意，您可以为`JSlider`实现拖放支持，以便它可以完全参与数据传输。虽然`JSlider`默认不支持拖放，但您希望（并期望）支持拖放的组件提供了专门的内置支持。

一旦在组件上调用了`setDragEnabled(true)`方法，以下组件就会识别拖动手势。例如，一旦您调用了`myColorChooser.setDragEnabled(true)`，您就可以从颜色选择器中拖动颜色：

+   `JColorChooser`

+   `JEditorPane`

+   `JFileChooser`

+   `JFormattedTextField`

+   `JList`

+   `JTable`

+   `JTextArea`

+   `JTextField`

+   `JTextPane`

+   `JTree`

以下组件支持开箱即用的拖放。如果您正在使用这些组件之一，您的工作已经完成。

+   `JEditorPane`

+   `JFormattedTextField`

+   `JPasswordField`

+   `JTextArea`

+   `JTextField`

+   `JTextPane`

+   `JColorChooser`

下面的组件已经具备拖放的框架，但您需要插入少量代码来定制支持您需求的功能。

+   `JList`

+   `JTable`

+   `JTree`

对于这些关键组件，Swing 执行拖放位置的计算和渲染；它允许您指定一个拖放模式；并处理组件特定的细节，比如树的展开。您的工作量相对较小。

* * *

**注意：**

您也可以在顶层容器上安装拖放支持，例如`JFrame`和`JDialog`。您可以在顶层拖放中了解更多信息。

* * *
