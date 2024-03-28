# 选择放置操作

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/dropaction.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/dropaction.html)

每个拖动源（基于 Java 或其他方式）在导出数据时都会公布其支持的操作集。如果支持复制数据，则公布`COPY`操作；如果支持从中移动数据，则公布`MOVE`操作，依此类推。对于 Swing 组件，源操作通过[`getSourceActions`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#getSourceActions-javax.swing.JComponent-)方法公布。

当启动拖动操作时，用户可以通过与拖动手势结合使用的键盘修饰符来控制选择哪个源操作用于传输 — 这称为*用户操作*。例如，默认情况下（未使用修饰符），通常表示移动操作，按住 Control 键表示复制操作，同时按住 Shift 和 Control 表示链接操作。用户操作可通过[`getUserDropAction`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getUserDropAction--)方法获得。

用户操作表示了偏好，但最终决定放置操作的是目标。例如，考虑一个只接受复制数据的组件。再考虑一个支持复制和移动的拖动源。即使用户表示偏好移动操作，仅接受来自源的数据的复制目标的`TransferHandler`也可以编码为仅接受数据，使用[`setDropAction`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#setDropAction-int-)方法。

这项工作发生在`canImport`方法中，目标的`TransferHandler`决定是否接受传入数据。如果源支持，一个明确选择`COPY`操作的实现可能如下所示：

```java
public boolean canImport(TransferHandler.TransferSupport support) {
    // for the demo, we will only support drops (not clipboard paste)
    if (!support.isDrop()) {
        return false;
    }

    // we only import Strings
    if (!support.isDataFlavorSupported(DataFlavor.stringFlavor)) {
        return false;
    }

    // check if the source actions (a bitwise-OR of supported actions)
    // contains the COPY action
    boolean copySupported = (COPY & support.getSourceDropActions()) == COPY;
    if (copySupported) {
        support.setDropAction(COPY);
        return true;
    }

    // COPY is not supported, so reject the transfer
    return false;
}

```

粗体显示的代码片段显示了查询源支持的放置操作的位置。如果支持复制，则调用`setDropAction`方法以确保只进行复制操作，并且该方法返回 true。

接下来我们将看一个演示，明确使用`setDropAction`设置放置操作。
