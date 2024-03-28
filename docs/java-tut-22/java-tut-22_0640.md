# 文本

> 原文：[`docs.oracle.com/javase/tutorial/2d/overview/text.html`](https://docs.oracle.com/javase/tutorial/2d/overview/text.html)

Java 2D API 具有各种文本渲染功能，包括用于呈现字符串的方法以及用于设置字体属性和执行文本布局的整个类。

如果您只想绘制静态文本字符串，最直接的方法是通过`Graphics`类使用`drawString`方法直接呈现它。要指定字体，您可以使用`Graphics`类的`setFont`方法。

如果您想要实现自己的文本编辑例程或需要比文本组件提供的更多对文本布局的控制，您可以使用`java.awt.font`包中的 Java 2D 文本布局类。

## 字体

字体用于表示字符串中字符的形状称为*字形*。特定字符或字符组合可能表示为一个或多个字形。例如，*á*可能由两个字形表示，而连字*fi*可能由一个字形表示。

*字体*可以被视为一组字形。单个字体可能有许多*面孔*，如斜体和常规。字体中的所有面孔具有类似的印刷特征，并且可以被识别为同一*字体系列*的成员。换句话说，具有特定样式的一组字形形成一个*字体面孔*。一组字体面孔形成一个*字体系列*。字体系列的集合形成了系统中可用的字体集。

当您使用 Java 2D API 时，您可以通过使用`Font`的实例来指定字体。您可以通过调用静态方法`GraphicsEnvironment.getLocalGraphicsEnvironment`并查询返回的`GraphicsEnvironment`来确定可用的字体。`getAllFonts`方法返回一个包含系统上所有可用字体的`Font`实例数组。`getAvailableFontFamilyNames`方法返回可用字体系列的列表。

## 文本布局

在文本可以显示之前，必须对其进行布局，以便字符以适当的位置表示为适当的字形。以下是两种用于管理文本布局的 Java 2D 机制：

+   `TextLayout`类管理文本布局、高亮显示和命中检测。`TextLayout`提供的功能处理最常见的情况，包括具有混合字体、混合语言和双向文本的字符串。

+   您可以通过使用`Font`类创建自己的`GlyphVector`对象，然后通过`Graphics2D`类渲染每个`GlyphVector`对象。因此，您可以完全控制文本的形状和位置。

## 文本的渲染提示

Java 2D API 使您能够通过使用*渲染提示*来控制形状和文本渲染的质量。渲染提示由`java.awt.RenderingHints`类封装。

当应用于文本时，这种能力用于抗锯齿（也称为平滑边缘）。例如，`KEY_TEXT_ANTIALIASING`提示可以让您单独控制文本的抗锯齿效果，而不影响其他形状的抗锯齿效果。要了解更多关于渲染提示的信息，请参阅控制渲染质量课程。
