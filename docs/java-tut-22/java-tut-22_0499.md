# 如何编写属性更改侦听器

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/events/propertychangelistener.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/propertychangelistener.html)

每当*bean*（符合 JavaBeans™规范的组件）的*bound property*的值更改时，都会触发属性更改事件。您可以从 Java 教程的 JavaBeans trail 中了解更多关于 beans 的信息。所有 Swing 组件也都是 beans。

一个 JavaBeans 属性通过其*get*和*set*方法访问。例如，`JComponent`具有*font*属性，可以通过`getFont`和`setFont`方法访问。

除了*get*和*set*方法外，绑定属性在其值更改时会触发属性更改事件。有关更多信息，请参阅 JavaBeans trail 中的 Bound Properties 页面。

一些常常需要属性更改侦听器的场景包括：

+   您已经实现了一个格式化文本字段，并且需要一种方法来检测用户何时输入了新值。您可以在格式化文本字段上注册一个属性更改侦听器，以侦听*value*属性的更改。请参阅 How to Use Formatted Text Fields 中的`FormattedTextFieldDemo`示例。

+   您已经实现了一个对话框，并且需要知道用户何时单击了对话框的按钮或更改了对话框中的选择。请参阅 How to Make Dialogs 中的`DialogDemo`示例，了解如何在选项面板上注册属性更改侦听器以侦听*value*属性的更改。您还可以查看 How to Use File Choosers 中的`FileChooserDemo2`示例，了解如何注册属性更改侦听器以侦听*directoryChanged*和*selectedFileChanged*属性的更改。

+   您需要在具有焦点的组件更改时收到通知。您可以在键盘焦点管理器上注册一个属性更改侦听器，以侦听*focusOwner*属性的更改。请参阅 How to Use the Focus Subsystem 中的`TrackFocusDemo`和`DragPictureDemo`示例。

尽管这些是属性更改侦听器的一些常见用途，但您可以在符合 JavaBeans 规范的任何组件的绑定属性上注册属性更改侦听器。

您可以通过两种方式注册属性更改侦听器。第一种使用`addPropertyChangeListener(PropertyChangeListener)`方法。当以这种方式注册侦听器时，您将收到有关该对象的每个绑定属性的每次更改的通知。在`propertyChange`方法中，您可以使用`PropertyChangeEvent`的`getPropertyName`方法获取已更改的属性的名称，如以下代码片段所示：

```java
KeyboardFocusManager focusManager =
   KeyboardFocusManager.getCurrentKeyboardFocusManager();
focusManager.addPropertyChangeListener(new FocusManagerListener());
...
public FocusManagerListener implements PropertyChangeListener {
    public void propertyChange(PropertyChangeEvent e) {
        String propertyName = e.getPropertyName();
        if ("focusOwner".equals(propertyName) {
            ...
        } else if ("focusedWindow".equals(propertyName) {
            ...
        }
    }
    ...
}

```

第二种注册属性更改监听器的方法使用了方法 `addPropertyChangeListener(String, PropertyChangeListener)`。`String` 参数是属性的名称。使用这种方法意味着只有在特定属性发生更改时才会收到通知。例如，如果你像这样注册了一个属性更改监听器：

```java
aComponent.addPropertyChangeListener("font",
                                     new FontListener());

```

`FontListener` 只有在组件的 *font* 属性值更改时才会收到通知。它 *不会* 在 *transferHandler*、*opaque*、*border* 或任何其他属性值更改时收到通知。

以下示例展示了如何使用 `addPropertyChangeListener` 的两个参数版本在格式化文本字段的 *value* 属性上注册属性更改监听器：

```java
*//...where initialization occurs:*
double amount;
JFormattedTextField amountField;
...
amountField.addPropertyChangeListener("value",
                                      new FormattedTextFieldListener());
...
class FormattedTextFieldListener implements PropertyChangeListener {
    public void propertyChanged(PropertyChangeEvent e) {
        Object source = e.getSource();
        if (source == amountField) {
            amount = ((Number)amountField.getValue()).doubleValue();
            ...
        }
        *...//re-compute payment and update field...*
    }
}

```

## 属性更改监听器 API

注册 PropertyChangeListener

| 方法 | 目的 |
| --- | --- |
| [addPropertyChangeListener(PropertyChangeListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#addPropertyChangeListener-java.beans.PropertyChangeListener-) | 向监听器列表中添加一个属性更改监听器。 |
| [addPropertyChangeListener(String, PropertyChangeListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#addPropertyChangeListener-java.lang.String-java.beans.PropertyChangeListener-) | 为特定属性添加属性更改监听器。只有当指定属性发生更改时才会调用监听器。 |

PropertyChangeListener 接口

*因为 `PropertyChangeListener` 只有一个方法，所以没有相应的适配器类。*

| 方法 | 目的 |
| --- | --- |
| [propertyChange(PropertyChangeEvent)](https://docs.oracle.com/javase/8/docs/api/java/beans/PropertyChangeListener.html#propertyChange-java.beans.PropertyChangeEvent-) | 当被监听的 bean 更改绑定属性时调用。 |

`PropertyChangeEvent` 类

| 方法 | 目的 |
| --- | --- |
| [Object getNewValue()](https://docs.oracle.com/javase/8/docs/api/java/beans/PropertyChangeEvent.html#getNewValue--) [Object getOldValue()](https://docs.oracle.com/javase/8/docs/api/java/beans/PropertyChangeEvent.html#getOldValue--) | 分别返回属性的新值或旧值。 |
| [String getPropertyName()](https://docs.oracle.com/javase/8/docs/api/java/beans/PropertyChangeEvent.html#getPropertyName--) | 返回已更改的属性的名称。 |
| [void setPropagationId()](https://docs.oracle.com/javase/8/docs/api/java/beans/PropertyChangeEvent.html#setPropagationId--) | 获取或设置传播 ID 值。保留供将来使用。 |

## 使用属性更改监听器的示例

以下表格列出了使用属性更改监听器的示例。

| 示例 | 描述位置 | 备注 |
| --- | --- | --- |
| `FormattedTextFieldDemo` | 如何使用格式化文本字段 | 在多个格式化文本字段上注册了属性更改监听器，以跟踪 *value* 属性的更改。 |
| `DialogDemo` | 如何创建对话框 | `CustomDialog`类在选项面板上注册属性更改监听器，以监听*value*和*inputValue*属性的变化。 |
| `FileChooserDemo2` | 如何使用文件选择器 | `ImagePreview`类在文件选择器上注册属性更改监听器，以监听*directoryChanged*和*selectedFileChanged*属性的变化。 |
| `TrackFocusDemo` | 如何使用焦点子系统 | 在键盘焦点管理器上注册属性更改监听器以跟踪*focusOwner*属性的变化。 |
