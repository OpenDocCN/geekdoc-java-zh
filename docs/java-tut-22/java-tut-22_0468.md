# 位置敏感的拖放

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/locsensitivedrop.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/locsensitivedrop.html)

有时您有一个复杂的组件，希望用户能够在其中的某些部分进行拖放，但在其他部分不允许。也许是一个只允许在特定列中放置数据的表格；或者是一个只允许在特定节点上放置数据的树。显然，您希望鼠标提供准确的反馈 — 当它悬停在接受拖放的组件的特定部分上时，应该只显示拖放位置。

通过在`TransferHandler`类的[`canImport(TransferHandler.TransferSupport)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#canImport-javax.swing.TransferHandler.TransferSupport-)方法中安装必要的逻辑，可以轻松实现这一点。它仅适用于这个特定版本的`canImport`，因为它在拖动手势悬停在组件边界上时被连续调用。当此方法返回 true 时，Swing 显示拖放光标并且拖放位置在视觉上被指示；当此方法返回 false 时，Swing 显示“无拖动”鼠标光标并且拖放位置不显示。

例如，想象一个允许拖放但不允许在第一列的表格。`canImport` 方法可能如下所示：

```java
public boolean canImport(TransferHandler.TransferSupport info) {
    // for the demo, we will only support drops (not clipboard paste)
    if (!info.isDrop()) {
        return false;
    }

    // we only import Strings
    if (!info.isDataFlavorSupported(DataFlavor.stringFlavor)) {
        return false;
    }

    // fetch the drop location
    JTable.DropLocation dl = (JTable.DropLocation)info.getDropLocation();

    int column = dl.getColumn();

    // we do not support invalid columns or the first column
    if (column == -1 || column == 0) {
        return false;
    }

    return true;
}

```

粗体显示的代码指示了位置敏感的拖放逻辑：当用户以无法计算列（因此无效）的方式放置数据或者当用户将文本放置在第一列时，`canImport` 方法返回 false — 因此 Swing 显示“无拖动”鼠标光标。一旦用户将鼠标移出第一列，`canImport` 返回 true 并且 Swing 显示拖动光标。

接下来，我们展示一个实现了位置敏感拖放的树的演示。
