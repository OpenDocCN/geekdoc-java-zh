# 如何使用 ButtonGroup 组件

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/components/buttongroup.html`](https://docs.oracle.com/javase/tutorial/uiswing/components/buttongroup.html)

`ButtonGroup`组件管理一组按钮的选中/未选中状态。对于该组，`ButtonGroup`实例保证一次只能选择一个按钮。

最初，由`ButtonGroup`实例管理的所有按钮都未被选中。

## 如何使用 ButtonGroup 功能

你可以将`ButtonGroup`与任何继承自`AbstractButton`的对象一起使用。通常，按钮组包含`JRadioButton, JRadioButtonMenuItem`或`JToggleButton`的实例。将`JButton`或`JMenuItem`的实例放入按钮组中是没有意义的，因为`JButton`和`JMenuItem`没有实现选择/取消选择按钮状态。

通常，编写使用`ButtonGroup`组件的代码时，通常会遵循以下步骤。

1.  子类化`JFrame`

1.  与布局管理器一起调用`ContextPane`

1.  声明和配置一组单选按钮或切换按钮

1.  实例化一个`ButtonGroup`对象

1.  调用该 buttongroup 对象上的`add`方法，以便将每个按钮添加到组中。

有关详细信息和代码示例，请参阅如何使用单选按钮。它展示了如何使用`ButtonGroup`组件将一组单选按钮设置到一个 JPanel 中。

## ButtonGroup API

常用的 Button Group 构造函数/方法

| 构造函数或方法 | 目的 |
| --- | --- |
| [ButtonGroup()](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#ButtonGroup--) | 创建一个`ButtonGroup`实例。 |
| [void add(AbstractButton)](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#add-javax.swing.AbstractButton-) [void remove(AbstractButton)](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#remove-javax.swing.AbstractButton-) | 将按钮添加到组中，或从组中移除按钮。 |
| [public ButtonGroup getGroup()](https://docs.oracle.com/javase/8/docs/api/javax/swing/DefaultButtonModel.html#getGroup--) *(在`DefaultButtonModel`中)* | 获取控制按钮的`ButtonGroup`（如果有）。例如：`ButtonGroup group = ((DefaultButtonModel)button.getModel()).getGroup();` |
| [public ButtonGroup clearSelection()](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#ButtonGroup--) | 清除 ButtonGroup 中选定按钮的状态。ButtonGroup 中的按钮都未被选中。 |

## ButtonGroup 示例

以下演示应用程序使用 ButtonGroup 组件将单选按钮分组显示在窗口上。

| 示例 | 描述位置 | 注意事项 |
| --- | --- | --- |
| `RadioButtonDemo` | 如何使用单选按钮 | 使用单选按钮确定应显示哪五幅图像。 |
