# 导入方法

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/import.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/import.html)

现在我们将看一下用于将数据导入组件的方法。这些方法在组件成为操作目标时，用于放置手势或粘贴操作。用于导入数据的`TransferHandler`方法包括：

+   [`canImport(TransferHandler.TransferSupport)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#canImport-javax.swing.TransferHandler.TransferSupport-) — 这个方法在拖拽手势期间被重复调用，如果光标下方的区域可以接受传输，则返回 true，否则返回 false。例如，如果用户将颜色拖动到只接受文本的组件上，那么该组件的`TransferHandler`的`canImport`方法应该返回 false。

+   [`importData(TransferHandler.TransferSupport)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#importData-javax.swing.TransferHandler.TransferSupport-) — 这个方法在成功放置（或粘贴）时被调用，并启动数据传输到目标组件。如果导入成功则返回 true，否则返回 false。

* * *

**版本说明：**

这些方法替换了不使用`TransferSupport`类的旧版本。与其替代方法不同，`canImport(JComponent, DataFlavor[])`不会被连续调用。

* * *

您会注意到这些导入方法接受一个`TransferHandler.TransferSupport`参数。接下来我们将看一下`TransferSupport`类，然后一些示例导入方法。
