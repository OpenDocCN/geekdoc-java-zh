# 调整组件大小

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/lookandfeel/size.html`](https://docs.oracle.com/javase/tutorial/uiswing/lookandfeel/size.html)

您是否曾经需要一个较小版本的组件放置在工具面板、工具栏或状态栏中？您可以通过在组件上设置客户属性来调整组件的大小。除了“常规”大小外，还支持三种大小：迷你、小和大。

唯一不支持大小变体属性的组件是`JLabel`。但是，您可以通过更改其字体大小来更改标签的大小。

* * *

**注意：**

其他外观实现，如苹果的 Aqua，也可能支持大小变体客户属性。Nimbus 目前是唯一支持大小变体的 Sun 外观。

* * *

您可以通过一行代码设置组件的大小，在组件显示之前。以下代码片段显示了如何使用每个大小：

```java
// mini
myButton.putClientProperty("JComponent.sizeVariant", "mini");
// small
mySlider.putClientProperty("JComponent.sizeVariant", "small");
// large
myTextField.putClientProperty("JComponent.sizeVariant", "large");

```

如果您已正确设置大小变体属性，但组件显示为其“常规”大小，则可能需要强制更新 UI。您可以在窗口显示之前通过调用[`SwingUtilities.updateComponentTreeUI(Component)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingUtilities.html#updateComponentTreeUI-java.awt.Component-)方法来执行此操作。以下代码片段更新窗口及其包含的所有组件：

```java
JFrame frame = ...;

SwingUtilities.updateComponentTreeUI(frame);

frame.pack();
frame.setVisible(true);

```
