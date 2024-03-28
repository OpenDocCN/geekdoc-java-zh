# 如何编写一个动作监听器

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/events/actionlistener.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/actionlistener.html)

动作监听器可能是最容易实现的事件处理程序，也是最常见的。您实现一个动作监听器来定义用户执行某些操作时应该执行的操作。

每当用户执行操作时，都会发生一个动作事件。例如：当用户点击一个按钮，选择一个菜单项，在文本字段中按 Enter 键。结果是向所有注册在相关组件上的动作监听器发送一个`actionPerformed`消息。

要编写一个动作监听器，请按照以下步骤进行：

1.  声明一个事件处理程序类，并指定该类要么实现 ActionListener 接口，要么扩展实现 ActionListener 接口的类。例如：

    ```java
    public class MyClass implements ActionListener { 

    ```

1.  在一个或多个组件上注册事件处理程序类的实例作为监听器。例如：

    ```java
    someComponent.addActionListener(instanceOfMyClass);

    ```

1.  包含实现监听器接口中方法的代码。例如：

    ```java
    public void actionPerformed(ActionEvent e) { 
        ...//code that reacts to the action... 
    }

    ```

通常，要检测用户何时点击屏幕按钮（或执行键盘等效操作），程序必须有一个实现 ActionListener 接口的对象。程序必须使用 addActionListener 方法将此对象注册为按钮（事件源）的动作监听器。当用户点击屏幕按钮时，按钮会触发一个动作事件。这将导致调用动作监听器的 actionPerformed 方法（ActionListener 接口中的唯一方法）。该方法的唯一参数是一个 ActionEvent 对象，提供有关事件及其来源的信息。

让我们编写一个简单的程序，显示用户点击按钮的次数。首先，这是设置 TextField、按钮和 numClicks 变量的代码：

```java
public class AL extends Frame implements WindowListener,ActionListener {
TextField text = new TextField(20);
Button b;
private int numClicks = 0;

```

在上面的示例中，事件处理程序类是 AL，它实现了 ActionListener。

我们想要处理按钮点击事件，因此我们将一个动作监听器添加到按钮 b 上，如下所示：

```java
b = new Button("Click me");
b.addActionListener(this); 

```

在上述代码中，按钮 b 是一个组件，事件处理程序类 AL 的实例已注册在其上。

现在，我们想要显示文本，显示用户点击按钮的次数。我们可以通过以下代码实现：

```java
public void actionPerformed(ActionEvent e) {
         numClicks++;
         text.setText("Button Clicked " + numClicks + " times");

```

现在，当用户点击按钮 b 时，按钮会触发一个动作事件，调用动作监听器的 actionPerformed 方法。每次用户按下按钮时，numClicks 变量都会被追加，并且消息会显示在文本字段中。

这里是完整的程序（AL.java）：

```java

import java.awt.*;
import java.awt.event.*;

public class AL extends Frame implements WindowListener,ActionListener {
        TextField text = new TextField(20);
        Button b;
        private int numClicks = 0;

        public static void main(String[] args) {
                AL myWindow = new AL("My first window");
                myWindow.setSize(350,100);
                myWindow.setVisible(true);
        }

        public AL(String title) {

                super(title);
                setLayout(new FlowLayout());
                addWindowListener(this);
                b = new Button("Click me");
                add(b);
                add(text);
                b.addActionListener(this);
        }

        public void actionPerformed(ActionEvent e) {
                numClicks++;
                text.setText("Button Clicked " + numClicks + " times");
        }

        public void windowClosing(WindowEvent e) {
                dispose();
                System.exit(0);
        }

        public void windowOpened(WindowEvent e) {}
        public void windowActivated(WindowEvent e) {}
        public void windowIconified(WindowEvent e) {}
        public void windowDeiconified(WindowEvent e) {}
        public void windowDeactivated(WindowEvent e) {}
        public void windowClosed(WindowEvent e) {}

}

```

更多示例：`Beeper` 程序示例可以在本教程的事件简介中找到，事件监听器简介。你可以在 `Beeper.java` 中找到整个程序。在该部分描述的另一个示例，`MultiListener.java`，有两个动作源和两个动作监听器，一个监听器同时监听两个源，另一个只监听一个。

## 动作监听器 API

ActionListener 接口

*因为 `ActionListener` 只有一个方法，所以它没有对应的适配器类。*

| 方法 | 目的 |
| --- | --- |
| [actionPerformed(actionEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ActionListener.html#actionPerformed-java.awt.event.ActionEvent-) | 在用户执行动作后立即调用。 |

ActionEvent 类

| 方法 | 目的 |
| --- | --- |
| [String getActionCommand()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ActionEvent.html#getActionCommand--) | 返回与此动作关联的字符串。大多数可以触发动作事件的对象支持一个名为 `setActionCommand` 的方法，让您设置这个字符串。 |

| [int getModifiers()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ActionEvent.html#getModifiers--) | 返回一个整数，表示用户在动作事件发生时按下的修饰键。您可以使用 `ActionEvent` 定义的常量 `SHIFT_MASK`、`CTRL_MASK`、`META_MASK` 和 `ALT_MASK` 来确定按下了哪些键。例如，如果用户 Shift-选择一个菜单项，则以下表达式不为零：

```java
actionEvent.getModifiers() & ActionEvent.SHIFT_MASK

```

|

| [Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--) (*在 `java.util.EventObject` 中*) | 返回触发事件的对象。 |
| --- | --- |

## 使用动作监听器的示例

以下表格列出了一些使用动作监听器的示例。

| 示例 | 描述位置 | 注释 |
| --- | --- | --- |
| `Beeper` | 本节和 事件监听器简介 | 包含一个按钮和一个在点击按钮时发出哔声的动作监听器。 |
| `MultiListener` | 事件监听器简介 | 在一个按钮上注册两个不同的动作监听器。同时在两个不同的按钮上注册相同的动作监听器。 |
| `RadioButtonDemo` | 如何使用单选按钮 | 在五个单选按钮上注册相同的动作监听器。监听器使用 `getActionCommand` 方法来确定哪个单选按钮触发了事件。 |
| `MenuDemo` | 如何使用菜单 | 展示如何监听菜单项的动作事件。 |
| `TextDemo` | 如何使用文本字段 | 在文本字段上注册动作监听器的小程序。 |
| `IconDemo` | 如何使用图标 | 在动作监听器中加载图像。由于加载图像可能需要一些时间，该程序使用`SwingWorker`在后台线程中加载图像。 |
| `TableDialogEditDemo` | 如何使用表格 | 通过颜色选择对话框的确定按钮上的工厂方法注册动作监听器。 |
| `SliderDemo` | 如何使用滑块 | 在控制动画循环的计时器上注册动作监听器。 |
