# 如何编写项目监听器

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/events/itemlistener.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/itemlistener.html)

项目事件由实现[`ItemSelectable`](https://docs.oracle.com/8/docs/api/java/awt/ItemSelectable.html)接口的组件触发。通常，`ItemSelectable`组件维护一个或多个项目的开/关状态。触发项目事件的 Swing 组件包括按钮，如复选框、复选菜单项、切换按钮等...以及组合框。

这里是从`ComponentEventDemo.java`中提取的一些项目事件处理代码：

```java
*//where initialization occurs*
checkbox.addItemListener(this);
...
public void itemStateChanged(ItemEvent e) {
    if (e.getStateChange() == ItemEvent.SELECTED) {
        label.setVisible(true);
        ...
    } else {
        label.setVisible(false);
    }
}

```

## 项目监听器 API

项目监听器接口

*因为`ItemListener`只有一个方法，所以没有相应的适配器类。*

| 方法 | 目的 |
| --- | --- |
| [itemStateChanged(ItemEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ItemListener.html#itemStateChanged-java.awt.event.ItemEvent-) | 在被监听组件状态改变后调用。 |

项目事件类

| 方法 | 目的 |
| --- | --- |
| [Object getItem()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ItemEvent.html#getItem--) | 返回与状态改变的项目相关联的组件特定对象。通常这是一个包含所选项目文本的`String`。 |
| [ItemSelectable getItemSelectable()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ItemEvent.html#getItemSelectable--) | 返回触发项目事件的组件。您可以使用这个方法代替`getSource`方法。 |
| [int getStateChange()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ItemEvent.html#getStateChange--) | 返回项目的新状态。`ItemEvent`类定义了两种状态：`SELECTED`和`DESELECTED`。 |

## 使用项目监听器的示例

下表列出了一些使用项目监听器的示例。

| 示例 | 描述位置 | 注释 |
| --- | --- | --- |
| `ComponentEventDemo` | 本节和如何编写组件监听器 | 监听复选框上的项目事件，确定标签是否可见。 |
| `CheckBoxDemo` | 如何使用复选框 | 四个复选框共享一个项目监听器，该监听器使用`getItemSelected`来确定哪个复选框触发了事件。 |
| `MenuDemo` | 如何使用菜单 | 监听复选框菜单项上的项目事件。 |
| `MenuDemo` | 如何使用滚动窗格 | 监听切换按钮上的项目事件。 |
