# 使用其他 Swing 功能解决常见问题

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/misc/problems.html`](https://docs.oracle.com/javase/tutorial/uiswing/misc/problems.html)

**问题：** 我的应用程序没有显示我通过`UIManager.setLookAndFeel`请求的外观。

您可能要么将外观设置为无效的外观，要么在 UI 管理器加载默认外观之后设置它。如果您确定您指定的外观是有效的，并且设置外观是程序执行的第一件事情（例如，在其主方法的顶部），请检查是否有一个引用 Swing 类的静态字段。如果没有指定外观，则此引用可能导致加载默认外观。更多信息，包括如何在创建 GUI 后设置外观，请参阅外观部分。

**问题：** 为什么我的组件没有获得焦点？

+   是否是您创建的自定义组件（例如，`JComponent`的直接子类）？如果是，您可能需要为您的组件提供输入映射和鼠标监听器。有关更多信息和演示，请参阅如何使自定义组件可聚焦。

+   组件是否在`JWindow`对象内？焦点系统要求`JWindow`的拥有框架对于`JWindow`对象中的任何组件都可见才能获得焦点。默认情况下，如果您没有为`JWindow`对象指定拥有框架，则会为其创建一个不可见的拥有框架。解决方案是在创建`JWindow`对象时要么指定一个可见且可聚焦的拥有框架，要么改用`JDialog`或`JFrame`对象。

**问题：** 为什么我的对话框无法接收用户按下 Escape 键时生成的事件？

如果您的对话框包含文本字段，则可能正在消耗事件。

+   如果您想无论组件是否消耗事件都获取 Escape 事件，应该使用[``KeyEventDispatcher``](https://docs.oracle.com/javase/8/docs/api/java/awt/KeyEventDispatcher.html)。

+   如果您只想在组件未消耗事件时获取 Escape 事件，则在`JDialog`对象中的任何`JComponent`组件上注册一个键绑定，使用`WHEN_IN_FOCUSED_WINDOW`输入映射。更多信息，请参阅如何使用键绑定页面。

**问题：** 为什么我无法将 Swing 组件应用于托盘图标？`TrayIcon`类的当前实现支持`PopupMenu`组件，但不支持其 Swing 对应物`JPopupMenu`。这种限制缩小了使用其他 Swing 功能的能力，例如菜单图标。请参阅 Bug ID [6285881](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=6285881)。

+   为了消除这种不便，将创建一个新的`JTrayIcon`类。在那之前，请使用 AWT 组件添加菜单项、复选框菜单项或子菜单。

如果在本节中找不到您的问题，请参考解决常见组件问题。
