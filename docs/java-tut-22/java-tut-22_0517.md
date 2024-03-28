# 解决常见绘制问题

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/painting/problems.html`](https://docs.oracle.com/javase/tutorial/uiswing/painting/problems.html)

**问题：** 我不知道在哪里放置我的绘制代码。

+   绘制代码应该放在任何继承自`JComponent`的组件的`paintComponent`方法中。

**问题：** 我绘制的内容不显示。

+   检查您的组件是否完全显示。解决常见组件问题应该可以帮助您解决这个问题。

+   检查是否在需要更新外观时在组件上调用了`repaint`。

**问题：** 我的组件的前景显示出来，但背景是不可见的。结果是，我的组件后面的一个或多个组件意外可见。

+   确保您的组件是不透明的。例如，许多外观和感觉中的`JPanel`默认是不透明的。要使诸如`JLabel`和 GTK+ `JPanel`之类的组件不透明，必须在它们上调用`setOpaque(true)`。

+   如果您的自定义组件扩展了`JPanel`或更专业的`JComponent`后代，则可以通过在绘制组件内容之前调用`super.paintComponent`来绘制背景。

+   您可以使用以下代码在自定义组件的`paintComponent`方法顶部自行绘制背景：

    ```java
    g.setColor(getBackground());
    g.fillRect(0, 0, getWidth(), getHeight());
    g.setColor(getForeground());

    ```

**问题：** 我使用`setBackground`设置组件的背景颜色，但似乎没有效果。

+   很可能是因为您的组件没有绘制其背景，要么是因为它不是不透明的，要么是因为您的自定义绘制代码没有绘制背景。例如，如果您为`JLabel`设置背景颜色，则还必须在标签上调用`setOpaque(true)`以使标签的背景被绘制。

**问题：** 我使用的代码与教程示例完全相同，但不起作用。为什么？

+   代码是否在与教程示例完全相同的方法中执行？例如，如果教程示例中的代码在示例的`paintComponent`方法中，则该方法可能是代码保证正常工作的唯一位置。

**问题：** 如何绘制粗线？模式？

+   Java™ 2D API 提供了广泛的支持，用于实现线宽和样式，以及用于填充和描边形状的模式。有关使用 Java 2D API 的更多信息，请参阅 2D 图形教程。

**问题：** 特定组件的边缘看起来很奇怪。

+   因为组件通常会更新其边框以反映组件状态，所以通常应避免在除了`JPanel`和`JComponent`的自定义子类之外的组件上调用`setBorder`。

+   组件是否由类似 GTK+或 Windows XP 的外观和感觉绘制，这些外观和感觉使用 UI 绘制的边框而不是`Border`对象？如果是这样，请不要在组件上调用`setBorder`。

+   组件是否有自定义绘制代码？如果有，绘制代码是否考虑了组件的插图？

**问题：** 我的 GUI 中出现视觉伪影。

+   如果设置组件的背景颜色，请确保颜色没有透明度，如果组件应该是不透明的。

+   如果需要，使用`setOpaque`方法设置组件的不透明度。例如，内容窗格必须是不透明的，但具有透明背景的组件不得是不透明的。

+   确保你的自定义组件完全填充其绘制区域，如果它是不透明的。

**问题：** 我的自定义绘制代码性能较差。

+   如果你可以绘制组件的一部分，请使用`Graphics`的`getClip`或`getClipBounds`方法确定需要绘制的区域。你绘制的越少，速度就越快。

+   如果只有组件的一部分需要更新，请使用指定绘制区域的`repaint`版本发出绘制请求。

+   想要了解如何选择高效的绘制技术，请查看[Java Media APIs home page](http://www.oracle.com/technetwork/java/javase/tech/media-141984.html)中包含"performance"字符串的部分。

**问题：** 应用于看似相同的`Graphics`对象的相同变换有时会产生略有不同的效果。

+   因为 Swing 绘制代码在调用`paintComponent`之前设置了变换（使用`Graphics`的`translate`方法），所以你应用的任何变换都会具有累积效果。这在进行简单平移时并不重要，但是一个更复杂的`AffineTransform`可能会产生意想不到的结果。

如果你在这个列表中找不到你的问题，请查看 Solving Common Component Problems 和 Solving Common Layout Problems。
