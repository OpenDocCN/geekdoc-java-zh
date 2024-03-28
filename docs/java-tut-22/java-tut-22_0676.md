# 支持用户交互

> 原文：[`docs.oracle.com/javase/tutorial/2d/advanced/user.html`](https://docs.oracle.com/javase/tutorial/2d/advanced/user.html)

为了让用户与您显示的图形进行交互，您需要能够确定用户何时点击其中一个图形。[`Graphics2D`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics2D.html) 类的 `hit` 方法提供了一种简单确定鼠标点击是否发生在特定 [`Shape`](https://docs.oracle.com/javase/8/docs/api/java/awt/Shape.html) 对象上的方法。或者，您可以获取鼠标点击的位置并在 `Shape` 上调用 `contains` 方法来确定点击是否在 `Shape` 的边界内。

如果您使用基本文本，可以通过获取与文本对应的轮廓 `Shape`，然后使用该 `Shape` 调用 `hit` 或 `contains` 方法来执行简单的点击测试。支持文本编辑需要更复杂的点击测试。如果要允许用户编辑文本，通常应使用 Swing 可编辑文本组件之一。如果您使用基本文本并使用 [`TextLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/font/TextLayout.html) 类来管理文本的形状和位置，则还可以使用 `TextLayout` 来执行文本编辑的点击测试。有关更多信息，请参阅 [Java 2D 程序员指南](https://docs.oracle.com/javase/8/docs/technotes/guides/2d/spec/j2d-bookTOC.html) 中的文本和字体章节，或查看下面的 HitTestSample 示例，该示例使用 `TextLayout` 执行简单的点击测试。

## 示例：ShapeMover

此小程序允许用户在小程序窗口内拖动 `Shape`。`Shape` 在每个鼠标位置重新绘制，以提供用户拖动时的反馈。

<applet code="ShapeMover" archive="examples/lib/ShapeMoverApplet.jar" width="550" height="250" alt="ShapeMover applet"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果您看不到小程序运行，请至少安装 [Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 版本。

* * *

`ShapeMover.java` 包含了此小程序的完整代码。

当鼠标按下时，调用 `contains` 方法来确定光标是否在矩形的边界内。如果是，则更新矩形的位置。

```java
public void mousePressed(MouseEvent e){
    last_x = rect.x - e.getX();
    last_y = rect.y - e.getY();
    if(rect.contains(e.getX(),
        e.getY())) updateLocation(e);
    ...

public void updateLocation(MouseEvent e){
    rect.setLocation(last_x + e.getX(),
        last_y + e.getY());
    ...
    repaint();

```

您可能会注意到，在每个鼠标位置重新绘制 `Shape` 是很慢的，因为填充的矩形每次移动时都会重新渲染。使用双缓冲可以消除这个问题。如果使用 Swing，绘图将自动双缓冲；您无需更改渲染代码。这个程序的 Swing 版本的代码是 `SwingShapeMover.java`。

## 示例：HitTestSample

这个应用程序通过在用户点击 `TextLayout` 上的位置绘制默认插入符来说明点击测试，如下图所示。

<applet code="HitTestSample" archive="examples/lib/HitTestSampleApplet.jar" width="400" height="250" alt="HitTestSample applet"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果你看不到小程序在运行，你需要至少安装 [Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 版本。

* * *

``HitTestSample.java`` 包含了这个小程序的完整代码。

`mouseClicked` 方法使用 `TextLayout.hitTestChar` 返回一个包含鼠标点击位置（插入索引）的 `java.awt.font.TextHitInfo` 对象在 `TextLayout` 对象中。

`TextLayout` 的 `getAscent`、`getDescent` 和 `getAdvance` 方法返回的信息被用来计算 `TextLayout` 对象的原点位置，使其水平和垂直居中。

```java
...

private Point2D computeLayoutOrigin() {
  Dimension size = getPreferredSize();
  Point2D.Float origin = new Point2D.Float();

  origin.x = (float) (size.width -
             textLayout.getAdvance()) / 2;   
  origin.y = 
    (float) (size.height -
             textLayout.getDescent() +
             textLayout.getAscent())/2;
  return origin;
}

...

public void paintComponent(Graphics g) {
  super.paintComponent(g);
  setBackground(Color.white);
  Graphics2D graphics2D = (Graphics2D) g;                
  Point2D origin = computeLayoutOrigin();
  graphics2D.translate(origin.getX(),
                       origin.getY());

  // Draw textLayout.
  textLayout.draw(graphics2D, 0, 0);

  // Retrieve caret Shapes for insertionIndex.
  Shape[] carets =
      textLayout.getCaretShapes(insertionIndex);

  // Draw the carets.  carets[0] is the strong
  // caret and carets[1] is the weak caret.   
  graphics2D.setColor(STRONG_CARET_COLOR);
  graphics2D.draw(carets[0]);                
  if (carets[1] != null) {
    graphics2D.setColor(WEAK_CARET_COLOR);
    graphics2D.draw(carets[1]);
  }       
}

...

private class HitTestMouseListener
              extends MouseAdapter {

    /**
     * Compute the character position of the
     * mouse click.
     */     
    public void mouseClicked(MouseEvent e) {

      Point2D origin = computeLayoutOrigin();

      // Compute the mouse click location
      // relative to textLayout's origin.
      float clickX =
          (float) (e.getX() - origin.getX());
      float clickY =
          (float) (e.getY() - origin.getY());

      // Get the character position of the
      // mouse click.
      TextHitInfo currentHit =
          textLayout.hitTestChar(clickX, clickY);
      insertionIndex =
          currentHit.getInsertionIndex();

      // Repaint the Component so the new
      // caret(s) will be displayed.
      hitPane.repaint();
    }

```
