# 变换形状、文本和图像

> 原文：[`docs.oracle.com/javase/tutorial/2d/advanced/transforming.html`](https://docs.oracle.com/javase/tutorial/2d/advanced/transforming.html)

您可以在`Graphics2D`上下文中修改变换属性，以在渲染时移动、旋转、缩放和剪切图形基元。变换属性由[`AffineTransform`](https://docs.oracle.com/javase/8/docs/api/java/awt/geom/AffineTransform.html)类的实例定义。仿射变换是一种变换，如平移、旋转、缩放或剪切，在变换后平行线仍保持平行。

[`Graphics2D`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics2D.html)类提供了几种方法来更改变换属性。您可以构造一个新的`AffineTransform`并通过调用`transform`来更改`Graphics2D`的变换属性。

`AffineTransform`定义了以下工厂方法，以便更容易构造新的变换：

+   `getRotateInstance`

+   `getScaleInstance`

+   `getShearInstance`

+   `getTranslateInstance`

或者，您可以使用`Graphics2D`的一个变换方法来修改当前变换。当您调用这些便利方法之一时，生成的变换将与当前变换连接，并在渲染期间应用：

+   `rotate`  用于指定以弧度为单位的旋转角度

+   `scale`  用于指定*x*和*y*方向的缩放因子

+   `shear`  用于指定*x*和*y*方向的剪切因子

+   `translate`  用于指定*x*和*y*方向的平移偏移量

您还可以直接构造一个`AffineTransform`对象，并通过调用`transform`方法将其与当前变换连接。

`drawImage`方法也被重载，允许您指定一个在渲染时应用于图像的`AffineTransform`。在调用`drawImage`时指定变换不会影响`Graphics2D`的变换属性。

## 示例：变换

下面的程序与`StrokeandFill`相同，但还允许用户在渲染选定对象时选择要应用的变换。

<applet code="Transform" archive="examples/lib/TransformApplet.jar" width="550" height="400" alt="Transform applet"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果您看不到小程序运行，则需要安装至少[Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

``Transform.java`` 包含此小程序的完整代码。

当从变换菜单中选择一个变换时，该变换将连接到`AffineTransform` `at`上：

```java
public void setTrans(int transIndex) {
    // Sets the AffineTransform.
    switch ( transIndex ) {
    case 0 :
        at.setToIdentity();
        at.translate(w/2, h/2);
        break;
    case 1 :
        at.rotate(Math.toRadians(45));
        break;
    case 2 :
        at.scale(0.5, 0.5);
        break;
    case 3 :
        at.shear(0.5, 0.0);
        break;
    }
}

```

在显示与菜单选择对应的形状之前，应用程序首先从`Graphics2D`对象中检索当前变换：

```java
AffineTransform saveXform = g2.getTransform();

```

此变换将在渲染后恢复到`Graphics2D`中。

在检索当前变换后，创建另一个`AffineTransform`，`toCenterAt`，使形状在面板中心渲染。`at` `AffineTransform`被连接到`toCenterAt`上：

```java
AffineTransform toCenterAt = new AffineTransform();
toCenterAt.concatenate(at);
toCenterAt.translate(-(r.width/2), -(r.height/2));

```

使用`transform`方法将`toCenterAt`变换连接到`Graphics2D`变换上：

```java
g2.transform(toCenterAt);

```

渲染完成后，使用`setTransform`方法恢复原始变换：

```java
g2.setTransform(saveXform);

```

* * *

**注意：**永远不要使用`setTransform`方法将坐标变换连接到现有的变换上。`setTransform`方法会覆盖`Graphics2D`对象的当前变换，这可能会因其他原因而需要，比如在窗口中定位 Swing 和轻量级组件。执行变换的步骤如下：

1.  使用`getTransform`方法获取当前变换。

1.  使用`transform`、`translate`、`scale`、`shear`或`rotate`来连接一个变换。

1.  执行渲染。

1.  使用`setTransform`方法恢复原始变换。

* * *
