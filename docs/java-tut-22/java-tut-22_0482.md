# Swing 组件支持的监听器

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/events/eventsandcomponents.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/eventsandcomponents.html)

您可以通过查看可以在组件上注册的事件监听器的类型来了解组件可以触发哪些事件。例如，`JComboBox` 类定义了以下监听器注册方法：

+   `addActionListener`

+   `addItemListener`

+   `addPopupMenuListener`

因此，组合框除了继承自`JComponent`的监听器方法外，还支持动作、项目和上下文菜单监听器。

Swing 组件支持的监听器分为两类：

+   所有 Swing 组件都支持的监听器

+   其他 Swing 组件支持的监听器

## 所有 Swing 组件都支持的监听器

因为所有 Swing 组件都是从 AWT `Component` 类继承的，您可以在任何 Swing 组件上注册以下监听器：

**组件监听器**

监听组件的大小、位置或可见性的变化。

**焦点监听器**

监听组件是否获得或失去键盘焦点。

**键盘监听器**

监听键盘按键；键盘事件仅由当前具有键盘焦点的组件触发。

**鼠标监听器**

监听鼠标点击、鼠标按下、鼠标释放以及鼠标移动进入或离开组件的绘图区域。

**鼠标移动监听器**

监听鼠标光标在组件上的位置变化。

**鼠标滚轮监听器**

监听鼠标滚轮在组件上的移动。

[**层次监听器**](https://docs.oracle.com/javase/8/docs/api/java/awt/event/HierarchyListener.html)

监听组件的包含层次结构的更改事件。

[**层次边界监听器**](https://docs.oracle.com/javase/8/docs/api/java/awt/event/HierarchyBoundsListener.html)

监听组件的包含层次结构的移动和调整事件的变化。

所有 Swing 组件都是从 AWT `Container` 类继承的，但其中许多并不用作容器。因此，从技术上讲，任何 Swing 组件都可以触发容器事件，通知监听器组件已被添加到容器或从容器中移除。然而，实际上，只有容器（如面板和框架）和复合组件（如组合框）通常会触发容器事件。

`JComponent` 提供对另外三种监听器类型的支持。您可以注册一个[祖先监听器](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/AncestorListener.html)，以便在组件的包含祖先被添加到容器、移除、隐藏、显示或移动时收到通知。这种监听器类型是一个实现细节，早于层次监听器。

另外两种监听器类型是 Swing 组件符合 JavaBeans 规范的一部分。这意味着 Swing 组件支持绑定和约束属性，并通知监听器属性的更改。属性更改监听器 监听绑定属性的更改，并被几个 Swing 组件使用，例如 格式化文本字段，以跟踪组件的绑定属性的更改。此外，属性更改监听器以及 [可否决更改监听器](https://docs.oracle.com/javase/8/docs/api/java/beans/VetoableChangeListener.html) 被构建工具用于监听约束属性的更改。有关更多信息，请参考 JavaBeans 路径中的 属性 课程。

## Swing 组件支持的其他监听器

以下表格列出了 Swing 组件及其支持的专用监听器，不包括所有 `Component`、`Container` 或 `JComponent` 支持的监听器。在许多情况下，事件直接从组件中触发。在其他情况下，事件是从组件的数据或选择模型中触发的。要了解您感兴趣的特定组件和监听器的详细信息，请首先转到组件的操作指南部分，然后如有必要再转到监听器的操作指南部分。

此表列出了带有其专用监听器的 Swing 组件

| 组件 | 动作监听器 | 插入符监听器 | 更改监听器 | 文档监听器, 可撤销编辑监听器 | 项目监听器 | 列表选择监听器 | 窗口监听器 | 其他类型的监听器 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 按钮 | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |
| 复选框 | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |
| 颜色选择器 |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   |
| 组合框 | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |
| 对话框 |   |   |   |   |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |
| 编辑窗格 |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   | [超链接](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/HyperlinkListener.html) |
| 文件选择器 | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   |   |   |
| 格式化文本字段 | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |
| 框架 |   |   |   |   |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |
| 内部框架 |   |   |   |   |   |   |   | 内部框架 |
| 列表 |   |   |   |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | 列表数据 |
| 菜单 |   |   |   |   |   |   |   | [菜单](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/MenuListener.html) |
| 菜单项 | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   | [菜单键](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/MenuKeyListener.html) [菜单拖动鼠标](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/MenuDragMouseListener.html) |
| 选项窗格 |   |   |   |   |   |   |   |   |
| 密码字段 | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |
| 弹出菜单 |   |   |   |   |   |   |   | [弹出菜单](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/PopupMenuListener.html) |
| 进度条 |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   |
| 单选按钮 | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |
| 滑块 |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   |
| 微调器 |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   |
| 选项卡面板 |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   |

| 表格 |   |   |   |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   表格模型 [表格列模型](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableColumnModelListener.html)

[单元格编辑器](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/CellEditorListener.html) |

| 文本区域 |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 文本字段 | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |
| 文本面板 |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   [超链接](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/HyperlinkListener.html) |
| 切换按钮 | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |

| 树 |   |   |   |   |   |   |   |   树展开 树将展开

树模型

树选择 |

| 视口（被滚动窗格使用） |   |   | ![checked](img/12bd475494acc0d71eb308e2275a6457.png) |   |   |   |   |   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
