# TransferHandler 类

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/transferhandler.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/transferhandler.html)

数据传输机制的核心是[`TransferHandler`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html)类。顾名思义，`TransferHandler`提供了一个简单的机制来在`JComponent`之间传输数据 —— 所有细节都包含在这个类及其支持类中。大多数组件都提供了默认的传输处理程序。您可以在任何组件上创建和安装自己的传输处理程序。

有三种方法用于在组件上使用`TransferHandler`：

+   [`setDragEnabled(boolean)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html#setDragEnabled-boolean-) — 打开拖动支持。（默认值为 false。）这个方法在支持拖动手势的每个组件上定义；链接将带您到`JList`的文档。

+   [`setDropMode(DropMode)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html#setDropMode-javax.swing.DropMode-) — 配置如何确定放置位置。这个方法为`JList`、`JTable`和`JTree`定义；链接将带您到`JList`的文档。

+   [`setTransferHandler(TransferHandler)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setTransferHandler-javax.swing.TransferHandler-) — 用于插入自定义数据导入和导出。这个方法在`JComponent`上定义，因此每个 Swing 组件都会继承它。

如前所述，默认的 Swing 传输处理程序，例如文本组件和颜色选择器使用的那些，提供了被认为对于数据的导入和导出最有用的支持。然而列表、表格和树不支持默认的拖放。这是因为没有通用的方法来处理这些组件上的拖放。例如，在`JTree`的特定节点上放置意味着什么？是替换节点，插入在其下方，还是作为该节点的子节点插入？此外，我们不知道树背后的模型是什么类型 —— 它可能是不可变的。

虽然 Swing 不能为这些组件提供默认实现，但拖放的框架已经存在。您只需要提供一个管理实际数据导入的自定义`TransferHandler`。

* * *

**注意：**

如果在 Swing 组件上安装了自定义的`TransferHandler`，默认支持将被替换。例如，如果用一个只处理颜色的`TransferHandler`替换`JTextField`的`TransferHandler`，那么它将失去支持文本的导入和导出的能力。

如果您必须替换默认的`TransferHandler` — 例如，处理文本的`TransferHandler` — 您将需要重新实现文本导入和导出功能。这不需要像 Swing 提供的那样广泛 — 它可以简单到支持`StringFlavor`数据 flavor，具体取决于您的应用程序需求。

* * *

接下来我们展示了需要实现数据导出的`TransferHandler`方法。
