# 如何使用 BorderLayout

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/layout/border.html`](https://docs.oracle.com/javase/tutorial/uiswing/layout/border.html)

* * *

**注意：** 本课程涵盖了手动编写布局代码，这可能具有挑战性。如果您不想学习布局管理的所有细节，您可能更喜欢使用`GroupLayout`布局管理器结合构建工具来布局您的 GUI。其中一个构建工具是 NetBeans IDE。否则，如果您想手动编码而不想使用`GroupLayout`，那么`GridBagLayout`被推荐为下一个最灵活和强大的布局管理器。

* * *

如果您有兴趣使用 JavaFX 创建 GUI，请参阅[JavaFX 中的布局](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html)。

以下图表示了一个使用[`BorderLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/BorderLayout.html)类的应用程序的快照。

![BorderLayoutDemo 的快照](img/ad6ae2877c21489c23de688be50fa24d.png)

点击启动按钮以使用[Java™ Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html)运行 BorderLayoutDemo（[下载 JDK 7 或更高版本](http://www.oracle.com/technetwork/java/javase/downloads/index.html)）。或者，要自己编译和运行示例，请参考示例索引。

![启动 BorderLayoutDemo 应用程序](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/BorderLayoutDemoProject/BorderLayoutDemo.jnlp)

这个演示的完整代码在`BorderLayoutDemo.java`文件中。

如前面的图片所示，`BorderLayout`对象有五个区域。这些区域由`BorderLayout`常量指定：

+   `PAGE_START`

+   `PAGE_END`

+   `LINE_START`

+   `LINE_END`

+   `CENTER`

* * *

**版本说明：**

在 JDK 1.4 版本发布之前，各个区域的首选名称不同，从罗盘点（例如，`BorderLayout.NORTH`表示顶部区域）到我们示例中使用的常量的较长版本。我们示例中使用的常量是首选的，因为它们是标准的，并且使程序能够适应具有不同方向的语言。

* * *

如果窗口被放大，中心区域会尽可能占用所有可用空间。其他区域只会扩展到填满所有可用空间为止。通常，一个容器只使用`BorderLayout`对象的一个或两个区域  只有中心，或中心和底部。

以下代码向框架的内容窗格添加组件。因为内容窗格默认使用 `BorderLayout` 类，所以代码不需要设置布局管理器。完整程序在 `BorderLayoutDemo.java` 文件中。

```java
...*//Container pane = aFrame.getContentPane()*...
JButton button = new JButton("Button 1 (PAGE_START)");
pane.add(button, BorderLayout.PAGE_START);

//Make the center component big, since that's the
//typical usage of BorderLayout.
button = new JButton("Button 2 (CENTER)");
button.setPreferredSize(new Dimension(200, 100));
pane.add(button, BorderLayout.CENTER);

button = new JButton("Button 3 (LINE_START)");
pane.add(button, BorderLayout.LINE_START);

button = new JButton("Long-Named Button 4 (PAGE_END)");
pane.add(button, BorderLayout.PAGE_END);

button = new JButton("5 (LINE_END)");
pane.add(button, BorderLayout.LINE_END);

```

指定组件的位置（例如，`BorderLayout.LINE_END`）作为 `add` 方法的参数之一。如果此组件在由 `BorderLayout` 对象控制的容器中缺失，请确保已指定组件的位置，并且没有其他组件放置在相同位置。

所有使用 `BorderLayout` 类的教程示例将组件指定为 `add` 方法的第一个参数。例如：

```java
add(component, BorderLayout.CENTER)  //preferred

```

然而，在其他程序中的代码将组件指定为第二个参数。例如，以下是编写前述代码的替代方式：

```java
add(BorderLayout.CENTER, component)  //valid but old fashioned
    *or*
add("Center", component)             //valid but error prone

```

## BorderLayout API

以下表列出了指定间隙（以像素为单位）的构造函数和方法。

指定间隙

| 构造函数或方法 | 目的 |
| --- | --- |
| [`BorderLayout(int *horizontalGap*, int *verticalGap*)`](https://docs.oracle.com/javase/8/docs/api/java/awt/BorderLayout.html#BorderLayout-int-int-) | 定义具有指定组件间隙的边界布局。 |
| [`setHgap(int)`](https://docs.oracle.com/javase/8/docs/api/java/awt/BorderLayout.html#setHgap-int-) | 设置组件之间的水平间距。 |
| [`setVgap(int)`](https://docs.oracle.com/javase/8/docs/api/java/awt/BorderLayout.html#setVgap-int-) | 设置组件之间的垂直间距。 |

## 使用 BorderLayout 的示例

以下表列出了使用 `BorderLayout` 类的代码示例，并提供到相关部分的链接。

| 示例 | 描述位置 | 注释 |
| --- | --- | --- |
| `BorderLayoutDemo` | 本页 | 将一个组件放在五个可能的位置中的每一个。 |
| `TabbedPaneDemo` | 如何使用选项卡窗格 | 其中一个示例，将一个组件放在内容窗格的中心，使组件尽可能大。 |
| `CheckBoxDemo` | 如何使用复选框 | 创建一个使用 `BorderLayout` 类的 `JPanel` 对象。将组件放入左侧（实际上是 `LINE_START`）和中心位置。 |
