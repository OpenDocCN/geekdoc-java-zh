# 如何使用标签

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/components/label.html`](https://docs.oracle.com/javase/tutorial/uiswing/components/label.html)

使用[`JLabel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html)类，您可以显示不可选择的文本和图像。如果您需要创建一个显示字符串、图像或两者的组件，可以使用或扩展`JLabel`来实现。如果组件是交互式的并具有某种状态，请使用按钮而不是标签。

通过在标签文本中指定 HTML 代码，您可以赋予标签各种特性，如多行、多字体或多颜色。如果标签仅使用单一颜色或字体，您可以通过使用`setForeground`或`setFont`方法来避免 HTML 处理的开销。有关详细信息，请参阅在 Swing 组件中使用 HTML。

请注意，默认情况下标签不是不透明的。如果您需要绘制标签的背景，建议将其不透明属性设置为"true"。以下代码片段显示了如何实现这一点。

```java
label.setOpaque(true);

```

以下图片介绍了一个显示三个标签的应用程序。窗口被分为三行等高；每行中的标签尽可能宽。

![LabelDemo 的快照，使用带有文本和图标的标签。](img/dd78495f649dca956bf097892d93d98a.png)

* * *

**试一试：**

1.  单击“启动”按钮以使用[Java™ Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html)运行 Label Demo（[下载 JDK 7 或更高版本](http://www.oracle.com/technetwork/java/javase/downloads/index.html)）。或者，要自行编译和运行示例，请参阅示例索引。![启动 LabelDemo 应用程序](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/LabelDemoProject/LabelDemo.jnlp)

1.  调整窗口大小，以查看标签内容如何放置在标签的绘图区域内。

    所有标签内容都具有默认的垂直对齐方式——即，标签内容在标签绘图区域中垂直居中。包含图像和文本的顶部标签具有水平居中对齐。只包含文本的第二个标签具有左侧（leading）对齐，这是从左到右语言中仅包含文本标签的默认值。只包含图像的第三个标签具有水平居中对齐，这是仅包含图像标签的默认值。

* * *

下面是`LabelDemo.java`中创建上述示例标签的代码。

```java
ImageIcon icon = createImageIcon("images/middle.gif");
. . .
label1 = new JLabel("Image and Text",
                    icon,
                    JLabel.CENTER);
//Set the position of the text, relative to the icon:
label1.setVerticalTextPosition(JLabel.BOTTOM);
label1.setHorizontalTextPosition(JLabel.CENTER);

label2 = new JLabel("Text-Only Label");
label3 = new JLabel(icon);

```

`createImageIcon` 方法的代码与本教程中使用的代码类似。您可以在如何使用图标中找到它。

通常，标签描述另一个组件。当发生这种情况时，您可以通过使用`setLabelFor`方法来提高程序的可访问性，以标识标签描述的组件。例如：

```java
amountLabel.setLabelFor(amountField);

```

前面的代码取自如何使用格式化文本字段中讨论的`FormattedTextFieldDemo`示例，让辅助技术知道标签（`amountLabel`）提供有关格式化文本字段（`amountField`）的信息。有关辅助技术的更多信息，请参阅如何支持辅助技术。

## 标签 API

以下表格列出了常用的`JLabel`构造函数和方法。您可能要调用的其他方法由`Component`和`JComponent`类定义。它们包括`setFont`、`setForeground`、`setBorder`、`setOpaque`和`setBackground`。有关详细信息，请参阅 The JComponent Class。使用标签的 API 分为三类：

+   设置或获取标签内容

+   调整标签外观

+   支持辅助功能

* * *

**注意：**

在以下 API 中，不要将标签对齐与 X 和 Y 对齐混淆。X 和 Y 对齐由布局管理器使用，可以影响任何组件的大小或位置，而不仅仅是标签。另一方面，标签对齐对标签的大小或位置没有影响。标签对齐仅确定标签内容在标签绘制区域内的位置。通常，标签的绘制区域正好足以在标签上绘制，因此标签对齐是无关紧要的。有关 X 和 Y 对齐的更多信息，请参阅如何使用 BoxLayout。

* * *

设置或获取标签内容

| 方法或构造函数 | 目的 |
| --- | --- |

| [JLabel(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel-javax.swing.Icon-) [JLabel(Icon, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel-javax.swing.Icon-int-)

[JLabel(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel-java.lang.String-)

[JLabel(String, Icon, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel-java.lang.String-javax.swing.Icon-int-)

[JLabel(String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel-java.lang.String-int-)

[JLabel()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel--) | 创建一个`JLabel`实例，将其初始化为具有指定的文本/图像/对齐方式。`int`参数指定标签内容在其绘图区域内的水平对齐方式。水平对齐必须是`SwingConstants`接口中定义的以下常量之一（`JLabel`实现的接口）：`LEFT`、`CENTER`、`RIGHT`、`LEADING`或`TRAILING`。为了方便本地化，我们强烈建议使用`LEADING`和`TRAILING`，而不是`LEFT`和`RIGHT`。

| [void setText(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setText-java.lang.String-) [String getText()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getText--) | 设置或获取标签显示的文本。您可以使用 HTML 标记格式化文本，如在 Swing 组件中使用 HTML 中所述。 |
| --- | --- |
| [void setIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setIcon-javax.swing.Icon-) [Icon getIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getIcon--) | 设置或获取标签显示的图像。 |
| [void setDisplayedMnemonic(char)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setDisplayedMnemonic-char-) [char getDisplayedMnemonic()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getDisplayedMnemonic--) | 设置或获取应该看起来像键盘替代的字母。当标签描述无法显示键盘替代但具有键盘替代的组件（如文本字段）时，这很有帮助。如果还设置了 labelFor 属性（使用`setLabelFor`），那么当用户激活助记键时，键盘焦点将转移到由 labelFor 属性指定的组件。 |
| [void setDisplayedMnemonicIndex(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setDisplayedMnemonicIndex-int-) [int getDisplayedMnemonicIndex()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getDisplayedMnemonicIndex--) | 设置或获取文本中应该被装饰以表示助记键的字符的提示。当您有两个相同字符的实例并希望装饰第二个实例时，这很有用。例如，`setDisplayedMnemonicIndex(5)`装饰文本中位置为 5 的字符（即文本中的第 6 个字符）。并非所有类型的外观和感觉都支持此功能。 |
| [void setDisabledIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setDisabledIcon-javax.swing.Icon-) [Icon getDisabledIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getDisabledIcon--) | 设置或获取标签在禁用时显示的图像。如果不指定禁用图像，则外观会通过操作默认图像来创建一个。 |

调整标签外观

| 方法 | 目的 |
| --- | --- |

| [void setHorizontalAlignment(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setHorizontalAlignment-int-) [void setVerticalAlignment(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setVerticalAlignment-int-)

[int getHorizontalAlignment()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getHorizontalAlignment--)

[int getVerticalAlignment()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getVerticalAlignment--) | 设置或获取标签内容应放置的区域。[`SwingConstants`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingConstants.html) 接口定义了水平对齐的五个可能值：`LEFT`、`CENTER`（仅图像标签的默认值）、`RIGHT`、`LEADING`（仅文本标签的默认值）、`TRAILING`。垂直对齐有：`TOP`、`CENTER`（默认）和 `BOTTOM`。 |

| [void setHorizontalTextPosition(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setHorizontalTextPosition-int-) [void setVerticalTextPosition(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setVerticalTextPosition-int-)

[int getHorizontalTextPosition()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getHorizontalTextPosition--)

[int getVerticalTextPosition()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getVerticalTextPosition--) | 设置或获取标签文本相对于标签图像放置的位置。[`SwingConstants`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingConstants.html) 接口定义了水平位置的五个可能值：`LEADING`、`LEFT`、`CENTER`、`RIGHT` 和 `TRAILING`（默认）。垂直位置有：`TOP`、`CENTER`（默认）和 `BOTTOM`。 |

| [void setIconTextGap(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setIconTextGap-int-) [int getIconTextGap()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getIconTextGap--) | 设置或获取标签文本与图像之间的像素数。 |
| --- | --- |

支持辅助功能

| 方法 | 目的 |
| --- | --- |
| [void setLabelFor(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setLabelFor-java.awt.Component-) [Component getLabelFor()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getLabelFor--) | 设置或获取标签描述的组件。 |

## 使用标签的示例

下表列出了一些使用标签的示例。

| 示例 | 描述位置 | 注释 |
| --- | --- | --- |
| `LabelDemo` | 本节 | 展示了如何指定水平和垂直对齐，以及如何对齐标签的文本和图像。 |
| `HtmlDemo` | 在 Swing 组件中使用 HTML | 让您尝试为标签指定 HTML 文本。 |
| `BoxAlignmentDemo` | 解决对齐问题 | 演示了在垂直盒式布局中使用标签可能出现的对齐问题。展示了如何解决这个问题。 |
| `DialogDemo` | 如何使用对话框 | 使用可变标签显示说明并提供反馈。 |
| `SplitPaneDemo` | 如何使用分割窗格 和 如何使用列表 | 在滚动窗格内使用标签显示图像。 |
| `SliderDemo2` | 如何使用滑块 | 使用`JLabel`为滑块提供标签。 |
| `TableDialogEditDemo` | 如何使用表格 | 实现了一个标签子类`ColorRenderer`，用于在表格单元格中显示颜色。 |
| `FormattedTextFieldDemo` | 如何使用格式化文本字段 | 包含四行，每行包含一个标签和描述它的格式化文本字段。 |
| `TextComponentDemo` | 文本组件特性 | `TextComponentDemo`有一个内部类(`CaretListenerLabel`)，它扩展了`JLabel`以提供一个监听事件的标签，根据事件更新自身。 |
| `ColorChooserDemo` | 如何使用颜色选择器 | 使用不透明标签来显示当前选择的颜色在固定颜色背景上。 |

查看[使用 JavaFX UI 控件：标签](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/label.htm)教程，了解 JavaFX 标签控件。
