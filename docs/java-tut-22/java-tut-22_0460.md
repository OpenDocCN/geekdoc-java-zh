# 导出方法

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/export.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/export.html)

我们将要检查的第一组方法用于从组件中导出数据。当涉及的组件是操作的源时，这些方法会在拖动手势或剪切/复制操作时被调用。用于导出数据的`TransferHandler`方法包括：

+   [`getSourceActions(JComponent)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#getSourceActions-javax.swing.JComponent-) — 这个方法用于查询源组件支持的操作，比如`COPY`、`MOVE`或`LINK`，以及任何组合。例如，客户列表可能不支持将客户名称移出列表，但很可能支持复制客户名称。我们的大多数示例都支持`COPY`和`MOVE`。

+   [`createTransferable(JComponent)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#createTransferable-javax.swing.JComponent-) — 这个方法将要导出的数据捆绑到一个[`Transferable`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/Transferable.html)对象中，以便进行传输准备。

+   [`exportDone(JComponent, Transferable, int)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#exportDone-javax.swing.JComponent-java.awt.datatransfer.Transferable-int-) — 这个方法在导出完成后被调用。当操作是`MOVE`时，在传输完成后需要从源中移除数据 — 这个方法是进行任何必要清理的地方。

## 示例导出方法

这里是一些导出方法的示例实现：

```java
int getSourceActions(JComponent c) {
    return COPY_OR_MOVE;
}

Transferable createTransferable(JComponent c) {
    return new StringSelection(c.getSelection());
}

void exportDone(JComponent c, Transferable t, int action) {
    if (action == MOVE) {
        c.removeSelection();
    }
}

```

接下来我们将看一下用于数据导入的`TransferHandler`方法。
