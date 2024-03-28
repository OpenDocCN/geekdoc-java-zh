# 如何使用模型

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/components/model.html`](https://docs.oracle.com/javase/tutorial/uiswing/components/model.html)

大多数 Swing 组件都有模型。例如，按钮（`JButton`）有一个模型（`ButtonModel`对象），用于存储按钮的状态——其键盘助记符是什么，是否启用，选中或按下等。一些组件有多个模型。例如，列表（`JList`）使用`ListModel`来保存列表的内容，并使用`ListSelectionModel`来跟踪列表的当前选择。

通常你不需要了解组件使用的模型。例如，使用按钮的程序通常直接处理`JButton`对象，而不处理`ButtonModel`对象。

那么为什么模型存在呢？最大的原因是它们让你灵活地确定数据的存储和检索方式。例如，如果你正在设计一个在稀疏填充表中显示数据的电子表格应用程序，你可以创建一个针对这种用途进行优化的自定义表模型。

模型还有其他好处。它们意味着数据不会在程序的数据结构和 Swing 组件的数据结构之间复制。此外，模型会自动将更改传播给所有感兴趣的侦听器，使得 GUI 能够轻松与数据保持同步。例如，要向列表添加项目，可以调用列表模型的方法。当模型的数据发生变化时，模型会向`JList`和任何其他已注册的侦听器发送事件，GUI 相应更新。

虽然 Swing 的模型架构有时被称为模型-视图-控制器（MVC）设计，但实际上并不是这样。 Swing 组件通常是这样实现的，即视图和控制器是不可分割的，由外观提供的单个 UI 对象实现。 Swing 模型架构更准确地描述为*可分离的模型架构*。如果你对了解更多关于 Swing 模型架构感兴趣，请参阅[Swing 架构概述](http://www.oracle.com/technetwork/java/architecture-142923.html)，这是*Swing 连接*中的一篇文章。

## 一个示例：Converter

本节介绍了一个名为 Converter 的示例，这是一个不断在公制和美制单位之间转换距离测量的应用程序。你可以[运行 Converter](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ConverterProject/Converter.jnlp)（[下载 JDK 7 或更高版本](http://www.oracle.com/technetwork/java/javase/downloads/index.html)）。或者，要自己编译和运行示例，请参考示例索引。

如下图所示，Converter 具有两个滑块，每个滑块都与一个文本字段绑定。这些滑块和文本字段都显示相同的数据——一个距离——但使用两种不同的度量单位。

![在 Metal 外观中的 Converter 截图](img/94f6458d5ed6bedaf2e380fdf4430b56.png)

对于这个程序来说，重要的是确保只有一个模型控制数据的值。有各种方法可以实现这一点；我们通过将其推迟到顶部滑块的模型来实现。底部滑块的模型（一个名为`FollowerRangeModel`的自定义类的实例）将所有数据查询转发到顶部滑块的模型（一个名为`ConverterRangeModel`的自定义类的实例）。每个文本字段通过监听值变化的事件处理程序与其滑块保持同步，反之亦然。确保顶部滑块的模型最终决定显示什么距离。

当我们开始实现自定义滑块模型时，我们首先查看了如何使用滑块的 API 部分。它告诉我们所有滑块数据模型必须实现`BoundedRangeModel`接口。[`BoundedRangeModel` API 文档](https://docs.oracle.com/javase/8/docs/api/javax/swing/BoundedRangeModel.html)告诉我们接口有一个名为`DefaultBoundedRangeModel`的实现类。[`DefaultBoundedRangeModel`的 API 文档](https://docs.oracle.com/javase/8/docs/api/javax/swing/DefaultBoundedRangeModel.html)显示它是`BoundedRangeModel`的通用实现。

我们没有直接使用`DefaultBoundedRangeModel`，因为它将数据存储为整数，而`Converter`使用浮点数据。因此，我们实现了`ConverterRangeModel`作为`Object`的子类。然后，我们实现了`FollowerRangeModel`作为`ConverterRangeModel`的子类。

## 更多信息

要了解各个组件的模型，请参阅各个组件的"How to"页面和 API 文档。以下是一些直接使用模型的示例：

+   除了最简单的表格示例之外，所有示例都实现了自定义表格数据模型。

+   颜色选择器演示在颜色选择器的选择模型上有更改监听器，以便在用户选择新颜色时通知。在 ColorChooserDemo2 中，`CrayonPanel`类直接使用颜色选择模型来设置当前颜色。

+   DynamicTreeDemo 示例设置了树模型（为`DefaultTreeModel`的一个实例），直接与其交互，并监听其变化。

+   ListDemo 设置列表数据模型（为`DefaultListModel`的一个实例）并直接与其交互。

+   SharedModelDemo 定义了一个扩展`DefaultListModel`并实现`TableModel`的`SharedDataModel`类。一个`JList`和一个`JTable`共享`SharedDataModel`的一个实例，提供模型数据的不同视图。

+   在事件监听器示例中，ListDataEventDemo 直接创建并使用`DefaultListModel`。

+   我们的微调器示例创建微调器模型。

+   正如你已经看到的，转换器示例定义了两个自定义滑块模型。
