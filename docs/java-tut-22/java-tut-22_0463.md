# 设置拖放模式

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/dropmodes.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/dropmodes.html)

在启用组件的拖放时，例如列表，您需要决定如何解释放置位置。例如，您是否希望限制用户替换现有条目？您是否只允许添加或插入新条目？您是否希望两者都允许？为了配置这种行为，[`JList`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html) 类提供了 [`setDropMode`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html#setDropMode-javax.swing.DropMode-) 方法，支持以下拖放模式。

+   `JList` 的默认拖放模式是 `DropMode.USE_SELECTION`。在此模式下拖动时，列表中的选定项目会移动以匹配潜在的放置点。放置时，选定项目会移动到放置位置。此模式提供了向后兼容性，但不建议使用。

+   在 `DropMode.ON` 中，列表中的选定项目会移动以匹配潜在的放置点，但在放置时不会影响选定项目。此模式可用于在现有列表项上放置。

+   在 `DropMode.INSERT` 中，用户被限制在选择现有列表项之间的空间，或在列表中的第一个项目之前或最后一个项目之后。不允许选择现有列表项。

+   `DropMode.ON_OR_INSERT` 是 `ON` 和 `INSERT` 模式的组合。

`JTree` 类提供了相同的[拖放模式](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#setDropMode-javax.swing.DropMode-)，而 `JTable` 类有[更多](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.html#setDropMode-javax.swing.DropMode-)特定于添加行或列的模式。

要获取放置的位置，`TransferSupport` 类提供了 [`getDropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getDropLocation--) 方法，该方法返回放置发生的精确点。但对于列表组件，放置的索引比像素位置更有用，因此 `JList` 提供了一个特殊的子类，称为 [`JList.DropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html)。该类提供了 [`getIndex`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html#getIndex--) 和 [`isInsert`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html#isInsert--) 方法，这些方法为您处理了数学计算。

表格、树和文本组件分别提供了 `DropLocation` 的实现，其中的方法对每个组件都最合理。[`JTable.setDropMode`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.html#setDropMode-javax.swing.DropMode-) 方法提供了最多选择。以下表格显示了所有四个类的方法：

JList、JTree、JTable 和 JTextComponent 的 DropLocation 方法

| [`JList.DropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html) | [`JTree.DropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.DropLocation.html) | [`JTable.DropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.DropLocation.html) | [`JTextComponent.DropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.DropLocation.html) |
| --- | --- | --- | --- |
| [`isInsert`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html#isInsert--) | [`getChildIndex`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.DropLocation.html#getChildIndex--) | [`isInsertRow`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.DropLocation.html#isInsertRow--) | [`getIndex`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.DropLocation.html#getIndex--) |
| [`getIndex`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html#getIndex--) | [`getPath`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.DropLocation.html#getPath--) | [`isInsertColumn`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.DropLocation.html#isInsertColumn--) | [`getBias`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.DropLocation.html#getBias--) |
|   |   | [`getRow`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.DropLocation.html#getRow--) |   |
|   |   | [`getColumn`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.DropLocation.html#getColumn--) |   |

接下来是一个演示，实现了一个自定义的传输处理程序，用于列表组件，以便它完全参与拖放操作。
