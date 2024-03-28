# 事件

> 原文：[`docs.oracle.com/javase/tutorial/javabeans/writing/events.html`](https://docs.oracle.com/javase/tutorial/javabeans/writing/events.html)

一个 bean 类可以触发任何类型的事件，包括自定义事件。与属性一样，事件通过特定模式的方法名称来识别。

```java
public void add*<Event>*Listener(*<Event>*Listener a)
public void remove*<Event>*Listener(*<Event>*Listener a)

```

监听器类型必须是`java.util.EventListener`的子类。

例如，Swing 的`JButton`是一个 bean，当用户点击它时会触发`action`事件。`JButton`包括以下方法（实际上是从`AbstractButton`继承而来），这些方法是事件的 bean 模式：

```java
public void addActionListener(ActionListener l);
public void removeActionListener(ActionListener l);

```

Bean 事件被构建工具识别，并可用于将组件进行连线。例如，您可以将按钮的`action`事件连接起来，以触发某些操作，比如调用另一个 bean 的方法。
