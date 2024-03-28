# 如何使用 GridLayout

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/layout/grid.html`](https://docs.oracle.com/javase/tutorial/uiswing/layout/grid.html)

* * *

**注意：** 本课程涵盖手动编写布局代码，这可能具有挑战性。如果您不想学习布局管理的所有细节，您可能更喜欢使用`GroupLayout`布局管理器结合构建工具来布局您的 GUI。其中一个构建工具是 NetBeans IDE。否则，如果您想手动编码且不想使用`GroupLayout`，那么建议使用`GridBagLayout`作为下一个最灵活和强大的布局管理器。

* * *

如果您有兴趣使用 JavaFX 创建 GUI，请参阅[JavaFX 中的布局](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html)。

以下图表示使用[`GridLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/GridLayout.html)类的应用程序的快照。

![GridLayoutDemo 的快照](img/014b93e45655af3e7a1f69b4c75c538e.png)

点击“启动”按钮以使用[Java™ Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html)运行`GridLayoutDemo`（[下载 JDK 7 或更高版本](http://www.oracle.com/technetwork/java/javase/downloads/index.html)）。或者，要自行编译和运行示例，请参考示例索引。

![启动 GridLayoutDemo 应用程序](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/GridLayoutDemoProject/GridLayoutDemo.jnlp)

此演示的完整代码位于`GridLayoutDemo.java`文件中。

`GridLayout` 对象将组件放置在单元格网格中。每个组件占据其单元格中的所有可用空间，每个单元格的大小完全相同。如果调整`GridLayoutDemo`窗口的大小，`GridLayout` 对象会更改单元格大小，以使单元格尽可能大，给定容器可用的空间。

下面的代码片段创建了`GridLayout`对象和其管理的组件。

```java

GridLayout experimentLayout = new GridLayout(0,2);

...

        compsToExperiment.setLayout(experimentLayout);

        compsToExperiment.add(new JButton("Button 1"));
        compsToExperiment.add(new JButton("Button 2"));
        compsToExperiment.add(new JButton("Button 3"));
        compsToExperiment.add(new JButton("Long-Named Button 4"));
        compsToExperiment.add(new JButton("5"));

```

`GridLayout` 类的构造函数创建一个具有两列并且必要行数的实例。

使用组合框设置组件周围的垂直或水平填充量，然后单击“应用间隙”按钮。以下代码片段显示了如何使用`GridLayout`类的`setVgap`和`setHgap`方法处理您的选择：

```java

applyButton.addActionListener(new ActionListener(){
            public void actionPerformed(ActionEvent e){
                //Get the horizontal gap value
                String horGap = (String)horGapComboBox.getSelectedItem();
                //Get the vertical gap value
                String verGap = (String)verGapComboBox.getSelectedItem();
                //Set up the horizontal gap value
                experimentLayout.setHgap(Integer.parseInt(horGap));
                //Set up the vertical gap value
                experimentLayout.setVgap(Integer.parseInt(verGap));
                //Set up the layout of the buttons
                experimentLayout.layoutContainer(compsToExperiment);
            }
        });

```

## `GridLayout` API

以下表格列出了指定行数和列数的`GridLayout`类的构造函数。

`GridLayout` 类的构造函数

| 构造函数 | 目的 |
| --- | --- |
| [`GridLayout(int *rows*, int *cols*)`](https://docs.oracle.com/javase/8/docs/api/java/awt/GridLayout.html#GridLayout-int-int-) | 创建具有指定行数和列数的网格布局。布局中的所有组件都具有相同的大小。`rows`和`cols`中的一个，但不是两者都可以为零，这意味着可以在一行或一列中放置任意数量的对象。 |
| [`GridLayout(int *rows*, int *cols*, int *hgap*, int *vgap*)`](https://docs.oracle.com/javase/8/docs/api/java/awt/GridLayout.html#GridLayout-int-int-int-int-) | 创建具有指定行数和列数的网格布局。此外，水平和垂直间隙设置为指定值。水平间隙放置在每列之间。垂直间隙放置在每行之间。 |

`GridLayout`类有两个构造函数：

## 使用`GridLayout`的示例

下表列出了使用`GridLayout`类的代码示例，并提供到相关部分的链接。

| 示例 | 描述位置 | 注释 |
| --- | --- | --- |
| `GridLayoutDemo` | 本页面 | 使用了 2 列网格。 |
| `ComboBoxDemo2` | 如何使用组合框 | 其中一个示例，使用 1x1 网格使组件尽可能大。 |
| `LabelDemo` | 如何使用标签 | 使用了 3 行网格。 |
