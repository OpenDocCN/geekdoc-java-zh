# 细看绘制机制

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/painting/closer.html`](https://docs.oracle.com/javase/tutorial/uiswing/painting/closer.html)

现在您知道`paintComponent`方法是放置所有绘制代码的地方。虽然这个方法在绘制时会被调用，但实际上绘制是从类层次结构的更高层开始的，即`paint`方法（由`java.awt.Component`定义）。每当您的组件需要呈现时，绘制子系统都会执行此方法。其签名为：

+   `public void paint(Graphics g)`

`javax.swing.JComponent`扩展了这个类，并将`paint`方法进一步分解为三个单独的方法，按以下顺序调用：

+   `protected void paintComponent(Graphics g)`

+   `protected void paintBorder(Graphics g)`

+   `protected void paintChildren(Graphics g)`

API 并未阻止您的代码覆盖`paintBorder`和`paintChildren`，但一般来说，您没有理由这样做。在实际情况下，`paintComponent`将是您唯一需要覆盖的方法。

正如先前提到的，大多数标准的 Swing 组件都通过单独的 UI 代理实现其外观和感觉。这意味着大多数（或全部）标准 Swing 组件的绘制过程如下进行。

1.  `paint()`调用`paintComponent()`.

1.  如果`ui`属性非空，则`paintComponent()`会调用`ui.update()`。

1.  如果组件的`opaque`属性为 true，则`ui.update()`会用背景颜色填充组件的背景并调用`ui.paint()`。

1.  `ui.paint()`渲染组件的内容。

这就是为什么我们的`SwingPaintDemo`代码调用`super.paintComponent(g)`。我们可以添加额外的注释以使其更清晰：

```java
public void paintComponent(Graphics g) { // Let UI Delegate paint first, which 
    // includes background filling since 
    // this component is opaque.

    super.paintComponent(g);       
    g.drawString("This is my custom Panel!",10,20);
    redSquare.paintSquare(g);
}  

```

如果您已经理解了本课程提供的所有演示代码，恭喜！您已经具备足够的实际知识来在自己的应用程序中编写高效的绘制代码。但是，如果您想更深入地了解“底层原理”，请参考本课程第一页链接的 SDN 文章。
