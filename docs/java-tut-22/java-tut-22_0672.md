# 绘制区域剪切

> 原文：[`docs.oracle.com/javase/tutorial/2d/advanced/clipping.html`](https://docs.oracle.com/javase/tutorial/2d/advanced/clipping.html)

任何[`Shape`](https://docs.oracle.com/javase/8/docs/api/java/awt/Shape.html)对象都可以用作限制将呈现的绘图区域的剪切路径。剪切路径是[`Graphics2D`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics2D.html)上下文的一部分；要设置剪切属性，调用`Graphics2D.setClip`并传入定义要使用的剪切路径的`Shape`。可以通过调用`clip`方法并传入另一个`Shape`来缩小剪切路径；剪切设置为当前剪切和指定`Shape`的交集。

## 示例：ClipImage

该示例通过动画剪切路径来显示图像的不同部分。

<applet code="ClipImage" alt="applet animates clipping path" archive="examples/lib/ClipImageApplet.jar" width="300" height="300"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果看不到小程序运行，请至少安装[Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

`ClipImage.java`包含此小程序的完整代码。小程序需要`clouds.jpg`图像文件。

剪切路径由椭圆和随机设置尺寸的矩形的交集定义。将椭圆传递给`setClip`方法，然后调用`clip`将剪切路径设置为椭圆和矩形的交集。

```java
private Ellipse2D ellipse = new Ellipse2D.Float();
private Rectangle2D rect = new Rectangle2D.Float();
...
ellipse.setFrame(x, y, ew, eh);
g2.setClip(ellipse);
rect.setRect(x+5, y+5, ew-10, eh-10);
g2.clip(rect);

```

## 示例：Starry

也可以从文本字符串创建一个剪切区域。以下示例使用字符串*The Starry Night*创建一个`TextLayout`。然后，获取`TextLayout`的轮廓。[`TextLayout.getOutline`](https://docs.oracle.com/javase/8/docs/api/java/awt/font/TextLayout.html#getOutline-java.awt.geom.AffineTransform-)方法返回一个`Shape`对象，并从该`Shape`对象的边界创建一个`Rectangle`。边界包含布局可以绘制的所有像素。将图形上下文中的颜色设置为蓝色，并绘制轮廓形状，如下图和代码片段所示。

![The Starry Night text (outline)](img/858a497e45605021bbb6564d33c175d0.png)

```java
FontRenderContext frc = g2.getFontRenderContext();
Font f = new Font("Helvetica", 1, w/10);
String s = new String("The Starry Night");
TextLayout textTl = new TextLayout(s, f, frc);
AffineTransform transform = new AffineTransform();
Shape outline = textTl.getOutline(null);
Rectangle r = outline.getBounds();
transform = g2.getTransform();
transform.translate(w/2-(r.width/2), h/2+(r.height/2));
g2.transform(transform);
g2.setColor(Color.blue);
g2.draw(outline);   

```

接下来，使用从`getOutline`创建的`Shape`对象在图形上下文中设置一个剪切区域。将梵高的著名绘画作品`The Starry Night`的`starry.gif`图像绘制到从`Rectangle`对象的左下角开始的剪切区域中。

```java
g2.setClip(outline);
g2.drawImage(img, r.x, r.y, r.width, r.height, this);

```

<applet code="Starry" alt="applet showing Starry Night painting in clipping area" archive="examples/lib/StarryApplet.jar" width="600" height="150"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果你看不到小程序在运行，你需要安装至少 [Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 版本。

* * *

`Starry.java` 包含了这个程序的完整代码。这个小程序需要 `Starry.gif` 图像文件。
