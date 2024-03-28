# 解决常见组件问题

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/components/problems.html`](https://docs.oracle.com/javase/tutorial/uiswing/components/problems.html)

本节讨论您在使用组件时可能遇到的问题。如果在本节中找不到您的问题，请参考以下章节：

+   解决使用其他 Swing 功能的常见问题

+   解决常见布局问题

+   解决常见事件处理问题

+   解决常见绘制问题

**问题：** 我在实现一个模型（或者类似于 Java SE 平台标准版中的某些代码）时遇到了困难。

+   查看 Java SE 源代码。它随 JDK 一起分发，是一个很好的资源，可以找到实现模型、触发事件等代码示例。

**问题：** 每当文本字段中的文本更新时，文本字段的大小会发生变化。

+   您应该通过指定文本字段应具有的列数来指定文本字段的首选宽度。为此，您可以使用`JTextField`构造函数的`int`参数或`setColumns`方法。

**问题：** 当重新绘制时，内容窗格的某些区域看起来很奇怪。

+   如果设置内容窗格，请确保它是不透明的。您可以通过在内容窗格上调用`setOpaque(true)`来实现。请注意，尽管在大多数外观和感觉中`JPanel`是不透明的，但在 GTK+外观和感觉中并非如此。有关详细信息，请参阅将组件添加到内容窗格。

+   如果您的一个或多个组件执行自定义绘制，请确保您正确实现了它。查看解决常见绘制问题以获取帮助。

+   您可能遇到了线程安全问题。请参阅下一条目。

**问题：** 我的程序表现出一些奇怪的症状，有时似乎与时间有关。

+   确保您的代码是线程安全的。有关详细信息，请参阅 Swing 中的并发性。

**问题：** 我的模态对话框被其他窗口遮挡。

+   如果对话框没有父组件，请在创建时尝试将其设置为有效的框架或组件。

+   此错误已在 6.0 版本中修复。有关更多信息，请参阅[4255200](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=4255200)。

**问题：** 滚动条策略似乎没有按照广告中描述的那样工作。

+   一些 Swing 版本中的实现可能存在`VERTICAL_SCROLLBAR_AS_NEEDED`和`HORIZONTAL_SCROLLBAR_AS_NEEDED`策略的错误。如果您的项目可行，尽量使用最新版本的 Swing。

+   如果滚动窗格的客户端可以动态更改大小，则程序应设置客户端的首选大小，然后在客户端上调用`revalidate`。

+   确保您为您打算的方向指定了所需的策略。

**问题：** 我的滚动窗格没有滚动条。

+   如果要始终显示滚动条，请根据需要指定 `VERTICAL_SCROLLBAR_ALWAYS` 或 `HORIZONTAL_SCROLLBAR_ALWAYS` 作为滚动条策略。

+   如果希望根据需要显示滚动条，并且希望在创建滚动窗格时强制需要滚动条，有两种选择：要么设置滚动窗格或其容器的首选大小，要么实现一个具有滚动功能的类，并从 `getPreferredScrollableViewportSize` 方法返回小于组件标准首选大小的值。有关信息，请参阅 调整滚动窗格的大小。

**问题：** 我的分割窗格中的分隔符不移动！

+   您需要设置分割窗格中至少一个组件的最小大小。有关信息，请参阅 定位分隔符并限制其范围。

**问题：** `JSplitPane` 的 `setDividerLocation` 方法无效。

+   如果分割窗格没有大小（通常在尚未显示在屏幕上时为真），则 `setDividerLocation(double)` 方法无效。您可以使用 `setDividerLocation(int)` 或指定分割窗格包含组件的首选大小和分割窗格的调整权重。有关信息，请参阅 定位分隔符并限制其范围。

**问题：** 嵌套分割窗格上的边框看起来太宽。

+   如果嵌套分割窗格，则边框会累积  内部分割窗格的边框会显示在外部分割窗格的边框旁边，导致边框看起来特别宽。当嵌套许多分割窗格时，问题尤为明显。解决方法是在放置在另一个分割窗格中的任何分割窗格上将边框设置为 null。有关详细信息，请参阅 Java Bug 数据库中的 bug # [4131528](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=4131528)。

**问题：** 我的工具栏中的按钮太大。

+   尝试减少按钮的边距。例如：

    ```java
    button.setMargin(new Insets(0,0,0,0));

    ```

**问题：** 我的分层窗格中的组件层次不正确。实际上，层次似乎是相反的  深度越低，组件越高。

+   如果在分层窗格中添加组件时使用 `int` 而不是 `Integer`，就会出现这种情况。要查看发生了什么，在 `LayeredPaneDemo` 类中更改

    `layeredPane.add(label, new Integer(i));`

    为

    `layeredPane.add(label, **i**);`.

**问题：** 调用方法 `*colorChooser*.setPreviewPanel(null)` 未按预期删除颜色选择器的预览面板。

+   使用 `null` 参数指定默认预览面板。要删除预览面板，请指定一个没有大小的标准面板，如此：`*colorChooser*.setPreviewPanel(new JPanel());`
