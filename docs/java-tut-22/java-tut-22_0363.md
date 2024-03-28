# JComponent 类

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/components/jcomponent.html`](https://docs.oracle.com/javase/tutorial/uiswing/components/jcomponent.html)

除了顶级容器之外，所有以"J"开头命名的 Swing 组件都是从[`JComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html)类继承的。例如，`JPanel`、`JScrollPane`、`JButton`和`JTable`都继承自`JComponent`。但是，`JFrame`和`JDialog`不是因为它们实现了顶级容器。

`JComponent`类扩展了[`Container`](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html)类，而`Container`类本身又扩展了[`Component`](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html)类。`Component`类包括从提供布局提示到支持绘制和事件的所有内容。`Container`类支持向容器添加组件和对它们进行布局。本节的 API 表格总结了`Component`、`Container`和`JComponent`最常用的方法。

## JComponent 特性

`JComponent`类为其子类提供以下功能：

+   工具提示

+   绘制和边框

+   应用程序范围内可插拔的外观和感觉

+   自定义属性

+   布局支持

+   支持辅助功能

+   拖放支持

+   双缓冲

+   键绑定

**工具提示**

通过使用`setToolTipText`方法指定一个字符串，您可以为组件的用户提供帮助。当光标停留在组件上时，指定的字符串将显示在靠近组件的小窗口中。更多信息请参见如何使用工具提示。

**绘制和边框**

`setBorder`方法允许您指定组件显示在边缘周围的边框。要绘制组件的内部，请重写`paintComponent`方法。有关详细信息，请参见如何使用边框和执行自定义绘制。

**应用程序范围内可插拔的外观和感觉**

在幕后，每个`JComponent`对象都有一个对应的`ComponentUI`对象，负责执行所有绘图、事件处理、大小确定等工作。使用哪个`ComponentUI`对象取决于当前的外观和感觉，您可以使用`UIManager.setLookAndFeel`方法进行设置。详细信息请参见如何设置外观和感觉。

**自定义属性**

您可以将一个或多个属性（名称/对象对）与任何`JComponent`关联。例如，布局管理器可以使用属性将约束对象与其管理的每个`JComponent`关联起来。您可以使用`putClientProperty`和`getClientProperty`方法来放置和获取属性。有关属性的一般信息，请参阅属性。

**布局支持**

虽然`Component`类提供了布局提示方法，如`getPreferredSize`和`getAlignmentX`，但除了创建子类并覆盖这些方法之外，它并没有提供任何设置这些布局提示的方法。为了提供另一种设置布局提示的方法，`JComponent`类添加了设置器方法 - `setMinimumSize`、`setMaximumSize`、`setAlignmentX`和`setAlignmentY`。详情请参阅在容器中布局组件。

**辅助功能支持**

`JComponent`类提供了 API 和基本功能，以帮助辅助技术（如屏幕阅读器）从 Swing 组件中获取信息。有关辅助功能的更多信息，请参阅如何支持辅助技术。

**拖放支持**

`JComponent`类提供了 API 来设置组件的传输处理程序，这是 Swing 拖放支持的基础。详情请参阅拖放简介。

**双缓冲**

双缓冲平滑屏幕绘制。详情请参阅执行自定义绘制。

**按键绑定**

此功能使组件在用户按下键盘上的键时做出反应。例如，在许多外观中，当按钮获得焦点时，按下空格键等同于鼠标单击按钮。外观会自动设置按下和释放空格键之间的绑定以及按钮上的结果效果。有关按键绑定的更多信息，请参阅如何使用按键绑定。

## JComponent API

`JComponent`类提供了许多新方法，并继承了许多方法从`Component`和`Container`。以下表格总结了我们最常使用的方法。

+   自定义组件外观

+   设置和获取组件状态

+   处理事件

+   绘制组件

+   处理包含关系

+   组件布局

+   获取大小和位置信息

+   指定绝对大小和位置

自定义组件外观

| 方法 | 目的 |
| --- | --- |
| [void setBorder(Border)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setBorder-javax.swing.border.Border-) [Border getBorder()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getBorder--) | 设置或获取组件的边框。详情请参阅如何使用边框。 |
| [void setForeground(Color)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setForegroundColor-java.awt.Color-) [void setBackground(Color)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setBackground-java.awt.Color-) | 为组件设置前景色或背景色。前景色通常是组件中绘制文本所使用的颜色。背景色（不出所料）是组件背景区域的颜色，假设组件是不透明的。 |
| [Color getForeground()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getForeground--) [Color getBackground()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getBackground--) | 获取组件的前景色或背景色。 |
| [void setOpaque(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setOpaque-boolean-) [boolean isOpaque()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#isOpaque--) | 设置或获取组件是否是不透明的。不透明组件会用其背景颜色填充其背景。 |
| [void setFont(Font)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setFont-java.awt.Font-) [Font getFont()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getFont--) | 设置或获取组件的字体。如果组件没有设置字体，则返回其父组件的字体。 |
| [void setCursor(Cursor)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setCursor-java.awt.Cursor-) [Cursor getCursor()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getCursor--) | 设置或获取显示在组件及其所有包含组件上的光标（除了具有自己设置光标的子组件）。示例：`aPanel.setCursor( Cursor.getPredefinedCursor( Cursor.WAIT_CURSOR));` |

设置和获取组件状态

| 方法 | 目的 |
| --- | --- |

| [void setComponentPopupMenu(JPopupMenu)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setComponentPopupMenu-javax.swing.JPopupMenu-) | 为这个`JComponent`设置`JPopupMenu`。UI 负责注册绑定并添加必要的监听器，以便在适当的时候显示`JPopupMenu`。`JPopupMenu`何时显示取决于外观和感觉：有些可能在鼠标事件上显示它，有些可能启用键绑定。

如果`popup`为 null，并且`getInheritsPopupMenu`返回`true`，那么`getComponentPopupMenu`将被委托给父组件。这提供了一种让所有子组件继承父组件的`popupmenu`的方法。

| [void setTransferHandler(TransferHandler)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setTransferHandler-javax.swing.TransferHandler-) [TransferHandler getTransferHandler()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getTransferHandler--) | 设置或移除`transferHandler`属性。`TransferHandler`支持通过剪切、复制或粘贴到/从剪贴板以及拖放来交换数据。有关更多详细信息，请参阅拖放简介。 |
| --- | --- |
| [void setToolTipText(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setToolTipText-java.lang.String-) | 设置要显示在工具提示中的文本。有关更多信息，请参阅如何使用工具提示。 |
| [void setName(String)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setName-java.lang.String-) [String getName()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getName--) | 设置或获取组件的名称。当您需要将文本与不显示文本的组件关联时，这可能很有用。 |
| [boolean isShowing()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#isShowing--) | 确定组件是否显示在屏幕上。这意味着组件必须可见，并且必须在一个可见且显示的容器中。 |
| [void setEnabled(boolean)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setEnabled-boolean-) [boolean isEnabled()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#isEnabled--) | 设置或获取组件是否启用。启用的组件可以响应用户输入并生成事件。 |
| [void setVisible(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setVisible-boolean-) [boolean isVisible()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#isVisible--) | 设置或获取组件是否可见。组件最初是可见的，顶层组件除外。 |

**处理事件**

(详细信息请参阅编写事件侦听器)

| 方法 | 目的 |
| --- | --- |

| [void addHierarchyListener(hierarchyListener l)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#addHierarchyListener-java.awt.event.HierarchyListener-) [void removeHierarchyListener(hierarchyListener l)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#removeHierarchyListener-java.awt.event.HierarchyListener-)

| 向此组件添加或移除指定的层次结构侦听器，以在此容器所属的层次结构发生更改时接收层次结构更改事件。如果侦听器 l 为 null，则不会抛出异常，也不会执行任何操作。 |
| --- |
| [void addMouseListener(MouseListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#addMouseListener-java.awt.event.MouseListener-) [void removeMouseListener(MouseListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#removeMouseListener-java.awt.event.MouseListener-) | 向组件添加或移除一个鼠标监听器。当用户使用鼠标与被监听组件交互时，鼠标监听器会收到通知。 |
| [void addMouseMotionListener(MouseMotionListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#addMouseMotionListener-java.awt.event.MouseMotionListener-) [void removeMouseMotionListener(MouseMotionListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#removeMouseMotionListener-java.awt.event.MouseMotionListener-) | 向组件添加或移除一个鼠标移动监听器。当用户在被监听组件的边界内移动鼠标时，鼠标移动监听器会收到通知。 |
| [void addKeyListener(KeyListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#addKeyListener-java.awt.event.KeyListener-) [void removeKeyListener(KeyListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#removeKeyListener-java.awt.event.KeyListener-) | 向组件添加或移除一个键盘监听器。当用户在键盘上输入并且被监听组件拥有键盘焦点时，键盘监听器会收到通知。 |
| [void addComponentListener(ComponentListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#addComponentListener-java.awt.event.ComponentListener-) [void removeComponentListener(ComponentListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#removeComponentListener-java.awt.event.ComponentListener-) | 向组件添加或移除一个组件监听器。当被监听组件被隐藏、显示、移动或调整大小时，组件监听器会收到通知。 |
| [boolean contains(int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#contains-int-int-) [boolean contains(Point)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#contains-java.awt.Point-) | 判断指定点是否在组件内。参数应该使用组件的坐标系来指定。两个`int`参数分别指定*x*和*y*坐标。 |
| [获取指定位置的组件(int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponentAt-int-int-) [获取指定位置的组件(Point)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponentAt-java.awt.Point-) | 返回包含指定*x, y*位置的组件。在组件重叠的情况下，返回最顶层的子组件。通过使用`Component.contains()`来确定哪个组件最接近索引 0，并声称包含给定点。 |

| [设置组件 Z 轴顺序(component comp, int index)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponentZOrder-java.awt.Component-int-)  | 将指定的组件移动到容器中指定的 Z 轴顺序索引位置。

如果组件是其他容器的子组件，则在添加到此容器之前，它将从该容器中移除。此方法与`java.awt.Container.add(Component, int)`之间的重要区别在于，此方法在将组件从其先前的容器中移除时，除非必要且允许底层本机窗口系统，否则不会调用组件的`removeNotify`。这样，如果组件具有键盘焦点，则在移动到新位置时会保持焦点。

**注意：**  Z 轴顺序决定了组件绘制的顺序。具有最高 Z 轴顺序的组件首先绘制，具有最低 Z 轴顺序的组件最后绘制。在组件重叠的情况下，具有较低 Z 轴顺序的组件会覆盖具有较高 Z 轴顺序的组件。 |

|  [获取组件 Z 轴顺序(component comp)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponentZOrder-comp-) | 返回容器内组件的 Z 轴顺序索引。组件在 Z 轴顺序层次结构中越高，其索引越低。具有最低 Z 轴顺序索引的组件最后绘制，位于所有其他子组件之上。 |
| --- | --- |

**绘画组件**

(详见执行自定义绘画)

| 方法 | 目的 |
| --- | --- |
| [void 重绘()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#repaint--) [void 重绘(int, int, int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#repaint-int-int-int-int-) | 请求重绘组件的全部或部分。四个`int`参数指定要绘制的矩形的边界（*x*、*y*、宽度、高度，按顺序）。 |
| [void 重绘(Rectangle)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#repaint-java.awt.Rectangle-) | 请求重绘组件内指定区域。 |
| [void 重新验证()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#revalidate--) | 请求重新布局组件及其受影响的容器。通常情况下，除非在组件可见后显式更改组件的大小/对齐提示或在其可见后更改包含层次结构，否则不应调用此方法。在`revalidate`后始终调用`repaint`。 |
| [void 绘制组件(Graphics)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#paintComponent-java.awt.Graphics-) | 绘制组件。重写此方法以实现自定义组件的绘制。 |

**处理包含层次结构**

(查看使用顶层容器获取更多信息)

| 方法 | 目的 |
| --- | --- |

| [添加组件(Component)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#add-java.awt.Component-) [添加组件(Component, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#add-java.awt.Component-int-)

[void 添加(Component, Object)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#add-java.awt.Component-java.lang.Object-) | 将指定组件添加到此容器。此方法的单参数版本将组件添加到容器的末尾。当存在时，`int`参数指示新组件在容器中的位置。当存在时，`Object`参数为当前布局管理器提供布局约束。 |

| [void 移除(int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#remove-int-) [void 移除(Component)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#remove-java.awt.Component-)

[void 移除全部()](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#removeAll--) | 从此容器中移除一个或全部组件。当存在时，`int`参数指示要移除的组件在容器中的位置。 |

| [获取根窗格()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getRootPane--) | 获取包含组件的根窗格。 |
| --- | --- |
| [获取顶层祖先容器](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getTopLevelAncestor--) | 获取组件的最顶层容器  一个`Window`、`Applet`，或者如果组件尚未添加到任何容器中则为 null。 |
| [获取父容器()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getParent--) | 获取组件的直接容器。 |
| [获取组件数量()](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponentCount--) | 获取此容器中的组件数量。 |
| [获取组件(int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponent-int-) [获取组件(Component[])](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponents--) | 获取此容器中的一个或所有组件。`int`参数表示要获取的组件的位置。 |
| [获取组件 Z 顺序索引(int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponentZOrder-int-) [获取组件 Z 顺序索引(Component[])](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponents--) | 返回容器内组件的 Z 顺序索引。组件在 Z 顺序层次结构中越高，其索引越低。具有最低 Z 顺序索引的组件最后绘制，位于所有其他子组件之上。 |

**组件布局**

(查看在容器内布局组件获取更多信息)

| 方法 | 目的 |
| --- | --- |

| [设置首选大小(Dimension)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setPreferredSize-java.awt.Dimension-) [设置最大大小(Dimension)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setMaximumSize-java.awt.Dimension-)

[设置最小尺寸(Dimension)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setMinimumSize-java.awt.Dimension-) | 设置组件的首选、最大或最小尺寸，以像素为单位。首选尺寸表示组件的最佳尺寸。组件的大小不应大于其最大尺寸，也不应小于其最小尺寸。请注意，这些仅是提示，可能会被某些布局管理器忽略。 |

| [获取首选大小(Dimension)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getPreferredSize--) [获取最大大小(Dimension)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getMaximumSize--)

[获取最小尺寸(Dimension)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getMinimumSize--) | 获取组件的首选、最大或最小尺寸，以像素为单位。许多 JComponent 类都有设置器和获取器方法。对于那些没有相应设置器方法的非`JComponent`子类，可以通过创建子类并覆盖这些方法来设置组件的首选、最大或最小尺寸。 |

| [void setAlignmentX(float)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setAlignmentX-float-) [void setAlignmentY(float)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setAlignmentY-float-) | 设置组件沿着*x-*或*y-*轴的对齐方式。这些值表示组件希望相对于其他组件对齐的方式。该值应为介于 0 和 1 之间的数字，其中 0 表示沿着原点对齐，1 表示与原点最远对齐，0.5 表示居中，依此类推。请注意，这仅仅是提示，某些布局管理器可能会忽略这些值。 |
| --- | --- |
| [float getAlignmentX()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getAlignmentX--) [float getAlignmentY()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getAlignmentY--) | 获取组件沿着*x-*或*y-*轴的对齐方式。对于非`JComponent`子类，没有相应的设置方法，您可以通过创建子类并重写这些方法来设置组件的对齐方式。 |
| [void setLayout(LayoutManager)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#setLayout-java.awt.LayoutManager-) [LayoutManager getLayout()](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getLayout--) | 设置或获取组件的布局管理器。布局管理器负责调整和定位容器中的组件。 |
| [void applyComponentOrientation(ComponentOrientation)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#applyComponentOrientation-java.awt.ComponentOrientation-) [void setComponentOrientation(ComponentOrientation)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#setComponentOrientation-java.awt.ComponentOrientation-) | 设置此容器及其中包含的所有组件的`ComponentOrientation`属性。有关更多信息，请参见设置容器的方向。 |

**获取大小和位置信息**

| 方法 | 目的 |
| --- | --- |
| [int getWidth()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getWidth--) [int getHeight()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getHeight--) | 获取组件当前以像素为单位的宽度或高度。 |
| [Dimension getSize()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getSize--) [Dimension getSize(Dimension)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getSize-java.awt.Dimension-) | 获取组件当前以像素为单位的大小。在使用此方法的单参数版本时，调用者负责创建返回结果的`Dimension`实例。 |
| [int getX()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getX--) [int getY()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getY--) | 获取组件原点相对于父级左上角以像素为单位的当前*x*或*y*坐标。 |
| [Rectangle getBounds()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getBounds--) [Rectangle getBounds(Rectangle)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getBounds-java.awt.Rectangle-) | 获取以像素为单位的组件边界。边界指定组件的宽度、高度和相对于其父级的原点。在使用此方法的单参数版本时，调用者负责创建返回结果的`Rectangle`实例。 |

| [Point getLocation()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getLocation--) [Point getLocation(Point)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getLocation-java.awt.Point-)

| 获取组件相对于父级左上角以像素为单位的当前位置。在使用`getLocation`方法的单参数版本时，调用者负责创建返回结果的`Point`实例。 |
| --- |
| [Point getLocationOnScreen()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getLocationOnScreen--) | 返回相对于屏幕左上角的位置。 |
| [Insets getInsets()](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getInsets--) | 获取组件边框的大小。 |

**指定绝对大小和位置**

(查看不使用布局管理器（绝对定位）获取更多信息)

| 方法 | 目的 |
| --- | --- |
| [void setLocation(int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setLocation-int-int-) [void setLocation(Point)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setLocation-java.awt.Point-) | 设置组件的位置，以像素为单位，相对于父级的左上角。两个`int`参数按顺序指定*x*和*y*。在不使用布局管理器时，使用这些方法来定位组件。 |
| [void setSize(int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setSize-int-int-) [void setSize(Dimension)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setSize-java.awt.Dimension-) | 设置以像素为单位的组件大小。两个`int`参数按顺序指定宽度和高度。在不使用布局管理器时，使用这些方法来调整组件大小。 |
| [void setBounds(int, int, int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setBounds-int-int-int-int-) [void setBounds(Rectangle)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setBounds-java.awt.Rectangle-) | 设置组件相对于父组件左上角的大小和位置（以像素为单位）。四个`int`参数按顺序指定*x*、*y*、宽度和高度。在不使用布局管理器时，使用这些方法来定位和调整组件的大小。 |
