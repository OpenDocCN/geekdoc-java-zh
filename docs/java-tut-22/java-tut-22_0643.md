# 教程：开始使用图形

> 原文：[`docs.oracle.com/javase/tutorial/2d/basic2d/index.html`](https://docs.oracle.com/javase/tutorial/2d/basic2d/index.html)

Java 2D API 强大而复杂。然而，Java 2D API 的绝大多数用途利用了`java.awt.Graphics`类中封装的其功能的一个小子集。本课程涵盖了应用程序开发人员最常见的需求。较少常见的需求稍后在 Java 2D API 的高级主题中描述。

[`Graphics`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics.html)类的大多数方法可以分为两个基本组：

+   绘制和填充方法，使您能够渲染基本形状、文本和图像

+   设置属性的方法，影响绘制和填充的外观

诸如`setFont`和`setColor`之类的方法定义了如何渲染绘制和填充方法。

本图说明了这些方法与图形对象的关系：

![此图表示`Graphics`类的基本方法](img/cbe470dcfce104fad70f4495aa02f820.png)

绘制方法包括：

+   `drawString` – 用于绘制文本

    ```java
    g.drawString("Hello", 10, 10);

    ```

+   `drawImage` – 用于绘制图像

    ```java
    g.drawImage(img, 
                0, 0, width, height,
                0, 0, imageWidth, imageHeight,
                null);                    

    ```

+   `drawLine`，`drawArc`，`drawRect`，`drawOval`，`drawPolygon` – 用于绘制几何形状

    ```java
    g2.draw(new Line2D.Double(0, 0, 30, 40));

    ```

根据您当前的需求，您可以根据以下标准在`Graphics`类中选择几种方法之一：

+   无论您想要在指定位置以原始大小渲染图像还是将其缩放以适应给定矩形

+   无论您喜欢用颜色填充图像的透明区域还是保持其透明

填充方法适用于几何形状，包括`fillArc`，`fillRect`，`fillOval`，`fillPolygon`。

无论您绘制文本还是图像，记住在 2D 图形中，每个点由其 x 和 y 坐标确定。所有绘制和填充方法都需要这些信息，这些信息确定了文本或图像应该渲染在哪里。

例如，要绘制一条线，应用程序调用以下代码：

`java.awt.Graphics.drawLine(int x1, int y1, int x2, int y2)`

在此代码中*(x1, y1)*是线的起点，*(x2, y2)*是线的终点。

因此，绘制水平线的代码如下：

`Graphics.drawLine(20, 100, 120, 100);`

下面的演示累积了所有提到的技术。移动滑块以显示各种天气类型。

<applet code="WeatherWizard" archive="examples/lib/2d_basic2d.jar" width="400" height="250" alt="WeatherWizard example"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果您看不到小程序运行，您需要安装至少[Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

`WeatherWizard` 演示使用 `JSlider` 组件以及各种图形功能来生成和显示指定的天气类型。有关 `JSlider` 类的更多信息，请参阅 Swing 教程的 How to Use Sliders 部分。

`WeatherPainter` 类的 `paint` 方法实现了图形特性。以下代码使用 `setupWeatherReport()` 方法绘制了一个图像。

```java
...
origComposite = g2.getComposite();
if (alpha0 != null) g2.setComposite(alpha0);
g2.drawImage(img0,
             0, 0, size.width, size.height,
             0, 0, img0.getWidth(null),
             img0.getHeight(null), null);
if (img1 != null) {
    if (alpha1 != null) g2.setComposite(alpha1);
    g2.drawImage(img1,
                 0, 0, size.width, size.height,
                 0, 0, img1.getWidth(null),
                 img1.getHeight(null), null);
} 
...

```

`setFont` 和 `drawString` 方法渲染温度和天气状况。

```java
...
// Freezing, Cold, Cool, Warm, Hot,
// Blue, Green, Yellow, Orange, Red
Font font = new Font("Serif", Font.PLAIN, 36);
g.setFont(font);

String tempString = feels + " " + temperature+"F";
FontRenderContext frc =
           ((Graphics2D)g).getFontRenderContext();
...
g.setColor(textColor);
int xTextTemp = rX-(int)boundsTemp.getX();
int yTextTemp = rY-(int)boundsTemp.getY();
g.drawString(tempString, xTextTemp, yTextTemp);

int xTextCond = rX-(int)boundsCond.getX();
int yTextCond = rY-(int)boundsCond.getY()
                   + (int)boundsTemp.getHeight();
g.drawString(condStr, xTextCond, yTextCond);

```

`fillRect` 方法允许您绘制一个填充了指定颜色的矩形。

```java
...
Rectangle2D boundsTemp 
          = font.getStringBounds(tempString, frc);
Rectangle2D boundsCond 
          = font.getStringBounds(condStr, frc);
int wText = Math.max((int)boundsTemp.getWidth(),
            (int)boundsCond.getWidth());
int hText = (int)boundsTemp.getHeight()
            + (int)boundsCond.getHeight();
int rX = (size.width-wText)/2;
int rY = (size.height-hText)/2;

g.setColor(Color.LIGHT_GRAY);
g2.fillRect(rX, rY, wText, hText);
...

```

尝试修改 `WeatherWizard` 演示以更改图形内容。例如，使用 `fillRoundRect` 方法代替 `fillRect` 或在 `setFont` 方法中应用另一个字体大小。在 `WeatherWizard.java` 文件中找到此小程序的完整代码。演示还需要以下图像：`weather-cloud.png`, `weather-rain.png`, `weather-snow.png` 和 `weather-sun.png`，这些图像位于 `images` 目录中。
