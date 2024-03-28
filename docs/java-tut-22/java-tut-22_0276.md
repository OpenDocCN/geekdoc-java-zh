# 开发可拖动的小程序

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/draggableApplet.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/draggableApplet.html)

通过指定`draggable`参数部署的 Java 小程序可以在浏览器之外拖动，并动态转换为 Java Web Start 应用程序。可以通过按下 Alt 键和左鼠标按钮并拖动鼠标来拖动 Java 小程序。拖动操作开始时，小程序将从其父容器（`Applet`或`JApplet`）中移除，并放置在一个新的无装饰顶级窗口（`Frame`或`JFrame`）中。拖动的小程序旁边会显示一个小浮动关闭按钮。单击浮动关闭按钮后，小程序将放回浏览器中。可以拖出浏览器的 Java 小程序将从此被称为*可拖动小程序*。

您可以通过以下方式自定义可拖动小程序的行为：

+   您可以更改用于将小程序拖出浏览器的按键和鼠标按钮序列。

+   您可以添加一个桌面快捷方式，用于在浏览器之外启动应用程序。

+   您可以定义小程序在拖出浏览器后应如何关闭。

以下部分描述了如何实现和自定义可拖动小程序。`MenuChooserApplet` 类用于演示可拖动小程序的开发和部署。在浏览器中打开`AppletPage.html`以在新页面上查看菜单选择器小程序。

* * *

**注意：** 如果您看不到小程序运行，请至少安装[Java SE Development Kit (JDK) 6 update 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

* * *

**注意：** 如果您看不到示例运行，请确保在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

## 启用拖动小程序的功能

您可以通过在部署小程序时将`draggable`参数设置为`true`来启用拖动小程序的功能，如下面的代码片段所示：

```java
<script src="https://www.java.com/js/deployJava.js"></script>
<script>
    var attributes = { code:'MenuChooserApplet', width:900, height:300 };
    var parameters = { jnlp_href: 'draggableapplet.jnlp', draggable: 'true' };
    deployJava.runApplet(attributes, parameters, '1.6');
</script>

```

## 更改用于拖动小程序的按键和鼠标按钮序列

您可以通过实现`isAppletDragStart`方法来更改用于拖动小程序的按键和鼠标按钮序列。在以下代码片段中，可以通过按下左鼠标按钮并拖动鼠标来拖动小程序：

```java
 public boolean isAppletDragStart(MouseEvent e) {
        if(e.getID() == MouseEvent.MOUSE_DRAGGED) {
            return true;
        } else {
            return false;
        }
    }

```

## 当小程序与浏览器断开连接时，可以添加桌面快捷方式

如果用户在将 applet 拖出页面后关闭浏览器窗口或离开页面，就说 applet 与浏览器*断开连接*。当 applet 与浏览器断开连接时，您可以为 applet 创建桌面快捷方式。桌面快捷方式可用于在浏览器之外启动应用程序。要启用桌面快捷方式的创建，请将 `offline-allowed` 和 `shortcut` 标签添加到 applet 的 Java 网络启动协议（JNLP）文件中。

```java
<information>
    <!-- ... -->
    <offline-allowed />
    <shortcut online="false">
        <desktop />
    </shortcut>
</information>

```

* * *

**注意：** 根据用户 Java 控制面板中的快捷方式创建设置，用户可能会在创建快捷方式之前收到确认提示。

* * *

## 定义 Applet 如何关闭

您可以定义您的 applet 如何关闭。例如，您的 Swing applet 可以有一个 `JButton` 来关闭 applet，而不是依赖默认的浮动 Close 按钮。

Java 插件软件为 applet 提供了 `ActionListener` 类的实例。这个 `ActionListener` 类的实例，也称为*关闭监听器*，可用于修改 applet 的默认关闭行为。

要定义 applet 如何关闭，请在您的 applet 中实现 `setAppletCloseListener` 和 `appletRestored` 方法。

在以下代码片段中，`MenuChooserApplet` 类接收关闭监听器并将其传递给 `MenuItemChooser` 类的实例：

```java
MenuItemChooser display = null;
// ...
display = new MenuItemChooser();
// ...
public void setAppletCloseListener(ActionListener cl) {
    display.setCloseListener(cl);
}

public void appletRestored() {
    display.setCloseListener(null);
}

```

`MenuItemChooser` 类负责控制 applet 的用户界面。`MenuItemChooser` 类定义了一个标记为“Close”的 `JButton`。当用户点击此 Close 按钮时，将执行以下代码：

```java
private void close() {
    // invoke actionPerformed of closeListener received
    // from the Java Plug-in software.
    if (closeListener != null) {
        closeListener.actionPerformed(null);
    }
}

```

## 请求和自定义 Applet 装饰

从 Java SE 7 发行版开始，在部署 applet 时，您可以指定拖动 applet 窗口应该使用默认或自定义窗口标题进行装饰。

要启用拖动 applet 的窗口装饰，请使用值为`"true"`的 `java_decorated_frame` 参数。还要指定 `java_applet_title` 参数以启用自定义窗口标题。此参数的值应为窗口标题的文本。

```java
<script src="https://www.java.com/js/deployJava.js"></script>
<script>
    var attributes =
      { code:'SomeDraggableApplet', width:100, height:100 };
    var parameters =
      { jnlp_href: 'somedraggableapplet.jnlp', 
          java_decorated_frame: 'true',
          java_applet_title: 'A Custom Title'   
      };
    deployJava.runApplet(attributes, parameters, '1.7');
</script>

```

`java_decorated_frame` 和 `java_applet_title` 参数也可以在 applet 的 JNLP 文件中指定，如下面的代码片段所示：

```java
<applet-desc main-class="SayHello" name="main test" height="150" width="300">
    <param name="java_decorated_frame" value="true" />
    <param name="java_applet_title" value="" />
</applet-desc>

```

下载源代码以进一步实验*可拖动 Applet*示例。
