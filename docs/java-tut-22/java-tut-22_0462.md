# TransferSupport 类

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/transfersupport.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/transfersupport.html)

[`TransferSupport`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html)类有两个功能。正如其名称所示，它的第一个功能是支持传输过程，为此提供了几个实用方法用于访问数据传输的详细信息。以下列表显示了可以用于从`TransferHandler`获取信息的方法。其中几个方法与放置操作有关，将在设置放置模式中讨论。

+   [`Component getComponent()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getComponent--)— 这个方法返回传输的目标组件。

+   [`int getDropAction()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getDropAction--) — 这个方法在传输为放置操作时返回选择的操作（`COPY`，`MOVE`或`LINK`）。如果传输不是一个放置操作，这个方法会抛出异常。

+   [`int getUserDropAction()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getUserDropAction--) — 这个方法返回用户选择的放置操作。例如，如果用户在拖动手势期间同时按住 Control 和 Shift 键，这表示一个`ACTION_LINK`操作。有关用户放置操作的更多信息，请参阅[`DropTargetDragEvent`](https://docs.oracle.com/javase/8/docs/api/java/awt/dnd/DropTargetDragEvent.html)的 API。如果传输不是一个放置操作，这个方法会抛出异常。

+   [`int getSourceDropActions()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getSourceDropActions--) — 这个方法返回源组件支持的操作集。如果传输不是一个放置操作，这个方法会抛出异常。

+   [`DataFlavor[] getDataFlavors()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getDataFlavors--) — 这个方法返回此组件支持的所有数据 flavors。例如，一个组件可能支持文件和文本，或者文本和颜色。如果传输不是一个放置操作，这个方法会抛出异常。

+   [`boolean isDataFlavorSupported(DataFlavor)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#isDataFlavorSupported-java.awt.datatransfer.DataFlavor-) — 这个方法在指定的`DataFlavor`受支持时返回 true。[`DataFlavor`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html)指示表示的数据类型，例如图像（`imageFlavor`），字符串（`stringFlavor`），文件列表（`javaFileListFlavor`）等。

+   [`Transferable getTransferable()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getTransferable--) — 此方法返回此传输的`Transferable`数据。 使用这些方法之一查询有关传输的信息比获取传输并查询它更有效，因此不建议使用此方法，除非您无法以其他方式获取信息。

+   [`DropLocation getDropLocation()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getDropLocation--) — 此方法返回组件中的放置位置。 具有内置拖放支持的组件，例如列表、表格和树，会重写此方法以返回更有用的数据。 例如，`JList`组件的此方法版本返回发生拖放的列表中的索引。 如果传输不是拖放，则此方法会引发异常。

## 样本导入方法

现在您熟悉了`TransferSupport`实用程序方法，让我们看看样本`canImport`和`importData`方法：

```java
public boolean canImport(TransferSupport supp) {
    // Check for String flavor
    if (!supp.isDataFlavorSupported(stringFlavor)) {
        return false;
    }

    // Fetch the drop location
    DropLocation loc = supp.getDropLocation();

    // Return whether we accept the location
    return shouldAcceptDropLocation(loc);
}

public boolean importData(TransferSupport supp) {
    if (!canImport(sup)) {
        return false;
    }

    // Fetch the Transferable and its data
    Transferable t = supp.getTransferable();
    String data = t.getTransferData(stringFlavor);

    // Fetch the drop location
    DropLocation loc = supp.getDropLocation();

    // Insert the data at this location
    insertAt(loc, data);

    return true;
}

```

接下来我们将看看如何为选定的组件设置拖放模式。
