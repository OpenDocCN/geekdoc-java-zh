# 如何使用键绑定

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/misc/keybinding.html`](https://docs.oracle.com/javase/tutorial/uiswing/misc/keybinding.html)

`JComponent`类支持键绑定作为响应用户键入的单个键的一种方式。以下是键绑定适用的一些示例情况：

+   您正在创建自定义组件并希望支持键盘访问。

    例如，您可能希望组件在焦点在其上且用户按下空格键时做出反应。

+   您希望覆盖现有键绑定的行为。

    例如，如果您的应用程序通常对 F2 键的按下做出特定反应，您可能希望它执行不同的操作或忽略按键。

+   您希望为现有操作提供新的键绑定。

    例如，您可能强烈感觉 Control-Shift-Insert 应该执行粘贴操作。

您通常不需要直接使用键绑定。它们在幕后由助记键（所有按钮和选项卡窗格以及[`JLabel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html)支持）和加速键（菜单项支持）使用。您可以在启用键盘操作部分找到助记键和加速键的覆盖范围。

键绑定的替代方法是使用键监听器。键监听器作为与键盘输入的低级接口有其用处，但对于响应单个键，键绑定更合适且更易于维护。如果组件没有焦点时要激活键绑定，则键监听器也会变得困难。键绑定的一些优点是它们在一定程度上是自解释的，考虑了包含层次结构，鼓励可重用的代码块（`Action`对象），并允许轻松删除、自定义或共享操作。此外，它们使更改绑定到操作的键变得容易。另一个`Action`的优点是它们具有启用状态，这提供了一种轻松禁用操作而无需跟踪其附加到的组件的方法。

本节的其余部分将为您提供使用键绑定所需的详细信息：

+   键绑定的工作原理

+   如何创建和删除键绑定

+   键绑定 API

+   使用键绑定的示例

## 键绑定的工作原理

`JComponent`提供的键绑定支持依赖于[`InputMap`](https://docs.oracle.com/javase/8/docs/api/javax/swing/InputMap.html)和[`ActionMap`](https://docs.oracle.com/javase/8/docs/api/javax/swing/ActionMap.html)类。输入映射将键盘按键绑定到操作名称，而操作映射指定与每个操作名称对应的 action。从技术上讲，您不需要在映射中使用操作名称；您可以使用任何对象作为映射中的“键”。但是，按照惯例，您使用命名操作的字符串作为“键”。

每个`InputMap`/`ActionMap`都有一个通常来自 UI 的父级。每当外观和感觉发生变化时，父级都会被重置。通过这种方式，开发人员指定的任何绑定在外观和感觉变化时都不会丢失。

每个`JComponent`都有一个动作映射和三个输入映射。输入映射对应以下焦点情况：

`JComponent.WHEN_FOCUSED`

组件具有键盘焦点。`WHEN_FOCUSED`输入映射通常在组件没有子组件时使用。例如，按钮使用`WHEN_FOCUSED`映射绑定 Space 键。

这些绑定只在组件获得焦点时生效。

`JComponent.WHEN_ANCESTOR_OF_FOCUSED_COMPONENT`

该组件包含（或是）具有焦点的组件。这个输入映射通常用于复合组件  一个其实现依赖于子组件的组件。例如，`JTable`使用`WHEN_ANCESTOR_OF_FOCUSED_COMPONENT`制作所有绑定，以便如果用户正在编辑，上箭头键（例如）仍然会更改所选单元格。

`JComponent.WHEN_IN_FOCUSED_WINDOW`

组件的窗口具有焦点或包含具有焦点的组件。这个输入映射通常用于助记键或加速键，无论焦点在窗口的哪个位置，它们都需要处于活动状态。

当用户按下一个键时，`JComponent`键事件处理代码会在一个或多个输入映射中搜索有效的按键绑定。当找到一个绑定时，它会在动作映射中查找相应的动作。如果动作已启用，则绑定有效且动作被执行。如果它被禁用，则继续搜索有效绑定。

如果一个按键存在多个绑定，只有找到的第一个有效绑定会被使用。输入映射按照以下顺序进行检查：

1.  焦点组件的`WHEN_FOCUSED`输入映射。

1.  焦点组件的`WHEN_ANCESTOR_OF_FOCUSED_COMPONENT`输入映射。

1.  焦点组件的父级的`WHEN_ANCESTOR_OF_FOCUSED_COMPONENT`输入映射，然后是其父级的父级，依此类推，沿着包含关系层次结构向上继续。注意：跳过禁用组件的输入映射。

1.  所有焦点窗口中启用的组件的`WHEN_IN_FOCUSED_WINDOW`输入映射都会被搜索。因为搜索组件的顺序是不可预测的，**避免重复的`WHEN_IN_FOCUSED_WINDOW`绑定！**

让我们考虑两种典型的按键绑定情况：一个按钮对 Space 键做出反应，一个带有默认按钮对 Enter 键做出反应的框架。

在第一种情况下，假设用户在`JButton`具有键盘焦点时按下 Space 键。首先，按钮的键监听器会收到事件通知。假设没有键监听器消耗事件（通过在`KeyEvent`上调用[`consume`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#consume--)方法），则会查阅按钮的`WHEN_FOCUSED`输入映射。会找到一个绑定，因为`JButton`使用该输入映射将 Space 绑定到一个动作名称。然后在按钮的动作映射中查找动作名称，并调用该动作的`actionPerformed`方法。`KeyEvent`被消耗，处理停止。

在第二种情况下，假设在具有默认按钮的框架内的任何位置按下 Enter 键（使用`JRootPane`的[`setDefaultButton`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRootPane.html#setDefaultButton-javax.swing.JButton-)方法设置默认按钮）。无论焦点在哪个组件上，首先通知其键监听器。假设它们中没有一个消耗了键事件，则会查阅焦点组件的`WHEN_FOCUSED`输入映射。如果它没有与该键绑定的内容，或者与该键绑定的操作已禁用，则会查阅焦点组件的`WHEN_ANCESTOR_OF_FOCUSED_COMPONENT`输入映射，然后（如果未找到绑定或与该键绑定的操作已禁用）查阅组件在包含层次结构中的每个祖先的`WHEN_ANCESTOR_OF_FOCUSED_COMPONENT`输入映射。最终，会搜索根窗格的`WHEN_ANCESTOR_OF_FOCUSED_COMPONENT`输入映射。由于该输入映射对 Enter 有有效绑定，因此会执行该操作，导致默认按钮被点击。

## 如何创建和删除键绑定

这是一个指定组件应该对 F2 键做出反应的示例：

```java
component.getInputMap().put(KeyStroke.getKeyStroke("F2"),
                            "doSomething");
component.getActionMap().put("doSomething",
                             anAction);
*//where anAction is a javax.swing.Action*

```

如前面的代码所示，要获取组件的动作映射，您可以使用`getActionMap`方法（从`JComponent`继承而来）。要获取输入映射，您可以使用`getInputMap(int)`方法，其中整数是前面列表中显示的`JComponent.WHEN_*FOCUSED*`常量之一。或者，在通常情况下，常量为`JComponent.WHEN_FOCUSED`，您可以直接使用不带参数的`getInputMap`。

要向其中一个映射添加条目，请使用`put`方法。您可以使用`KeyStroke.getKeyStroke(String)`方法获取`KeyStroke`对象来指定键。您可以在如何使用操作中找到创建`Action`（放入动作映射中）的示例。

这里是一个稍微复杂的示例，指定组件应该对 Space 键做出反应，就像用户点击鼠标一样。

```java
component.getInputMap().put(KeyStroke.getKeyStroke("SPACE"),
                            "pressed");
component.getInputMap().put(KeyStroke.getKeyStroke("released SPACE"),
                            "released");
component.getActionMap().put("pressed",
                             pressedAction);
component.getActionMap().put("released",
                             releasedAction);
*//where pressedAction and releasedAction are javax.swing.Action objects*

```

要使组件忽略其通常会响应的按键，您可以使用特殊的动作名称"none"。例如，以下代码使组件忽略 F2 键。

```java
component.getInputMap().put(KeyStroke.getKeyStroke("F2"),
                            "none");

```

* * *

**注意：**

前面的代码不会阻止搜索相关的 `WHEN_ANCESTOR_OF_FOCUSED_COMPONENT` 和 `WHEN_IN_FOCUSED_WINDOW` 输入映射以查找 F2 键绑定。要阻止此搜索，必须使用有效的操作而不是 "none"。例如：

```java
Action doNothing = new AbstractAction() {
    public void actionPerformed(ActionEvent e) {
        //do nothing
    }
};
component.getInputMap().put(KeyStroke.getKeyStroke("F2"),
                            "doNothing");
component.getActionMap().put("doNothing",
                             doNothing);

```

* * *

## 键绑定 API

以下表格列出了常用的键绑定 API。还请参阅 API 表格创建和使用操作，在如何使用操作部分。

+   创建和使用 InputMaps

+   创建和使用 ActionMaps

获取和使用 InputMaps

| 方法 | 目的 |
| --- | --- |

| [获取输入映射](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getInputMap--) [获取输入映射（int）](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getInputMap-int-) |

*(在 `JComponent` 中)* | 获取组件的一个输入映射。参数可以是这些 `JComponent` 常量之一：`WHEN_FOCUSED`、`WHEN_IN_FOCUSED_WINDOW` 或 `WHEN_ANCESTOR_OF_FOCUSED_COMPONENT`。无参数方法获取 `WHEN_FOCUSED` 输入映射。 |

| [设置（KeyStroke，Object）](https://docs.oracle.com/javase/8/docs/api/javax/swing/InputMap.html#put-javax.swing.KeyStroke-java.lang.Object-) *(在 `InputMap` 中)* | 设置与指定键盘按键关联的操作名称。如果第二个参数为 `null`，此方法将移除键盘按键的绑定。要忽略键盘按键，请使用 `"none"` 作为第二个参数。 |
| --- | --- |
| [获取指定用户键盘活动的对象](https://docs.oracle.com/javase/8/docs/api/javax/swing/KeyStroke.html#getKeyStroke-java.lang.String-) *(在 `KeyStroke` 中)* | 获取指定用户键盘活动的对象。典型参数为 "alt shift X"、"INSERT" 和 "typed a"。查看[`KeyStroke` API 文档](https://docs.oracle.com/javase/8/docs/api/javax/swing/KeyStroke.html)获取完整详情和`getKeyStroke`方法的其他形式。 |

获取和使用 ActionMaps

| 方法 | 目的 |
| --- | --- |
| [获取操作映射](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getActionMap--) *(在 `JComponent` 中)* | 获取将名称映射到组件操作的对象。 |
| [设置（Object，Action）](https://docs.oracle.com/javase/8/docs/api/javax/swing/ActionMap.html#put-java.lang.Object-javax.swing.Action-) *(在 `ActionMap` 中)* | 设置与指定名称关联的操作。如果第二个参数为 `null`，此方法将移除名称的绑定。 |

## 使用键绑定的示例

以下表格列出了使用键绑定的示例：

| 示例 | 描述位置 | 注释 |
| --- | --- | --- |
| TableFTFEditDemo | 如何使用表格 | IntegerEditor 类在格式化文本字段上注册一个键绑定，以在用户按下 Enter 键时验证输入。 |
| 文本组件演示 | 文本组件特性 | 在文本窗格上注册了按键绑定，用户按下 Control-B、Control-F、Control-P 和 Control-N 键时可以在文本中导航。 |
