# Swing 组件中的打印支持

> 原文：[`docs.oracle.com/javase/tutorial/2d/printing/swing.html`](https://docs.oracle.com/javase/tutorial/2d/printing/swing.html)

在前一节中展示的 `PrintUIWindow.java` 示例表明打印出的内容与屏幕上看到的完全相同。这种外观看起来是合理的。然而，如果一个窗口是可滚动的，那么当前滚动出视图的内容不会包含在打印输出中。这会在打印机上产生一个倾倒效果。当打印大型组件（如 Swing 表格或文本组件）时，这就成为一个特殊问题。组件可能包含许多行文本，这些文本在屏幕上并不都能完全可见。在这种情况下，以与屏幕显示一致的方式打印组件显示的内容。

要解决这个问题，Swing 表格和所有文本组件都具有打印功能。以下方法直接提供了 Java 2D 打印的使用：

+   [`javax.swing.JTable.print();`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.html#print--)

+   [`javax.swing.text.JTextComponent.print();`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#print--)

这些方法为它们的内容提供了完整的打印实现。应用程序不需要直接创建 `PrinterJob` 对象并实现 `Printable` 接口。调用这些方法会显示打印对话框，并根据用户的选择打印组件的数据。还有其他提供更多选项的方法。
