# 放置位置渲染

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/droplocation.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/droplocation.html)

这是一个更高级的主题，大多数人不需要担心它。但是，如果您有一个自定义组件，您将需要自行处理放置位置的渲染。

您可以注册以便在`dropLocation`属性更改时收到通知。您可以监听此更改，并在组件的自定义渲染器中或在`paintComponent`方法中使用`getDropLocation`方法自行渲染放置位置。

这里是监听`dropLocation`属性的示例：

```java
class Repainter extends PropertyChangeListener {
    public void propertyChange(PropertyChangeEvent pce) {
        repaintDropLocation(pce.getOldValue());
        repaintDropLocation(pce.getNewValue());
    }
}

comp.addPropertyChangeListener("dropLocation", newRepainter());

```

这里是`paintComponent`方法的一个示例：

```java
public void paintComponent(Graphics g) {
    super.paintComponent(g);

    DropLocation loc= getDropLocation();
    if (loc == null) {
        return;
    }

    renderPrettyIndicatorAt(loc);
}

```
