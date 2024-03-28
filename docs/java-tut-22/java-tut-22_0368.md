# 如何制作 Applets

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/components/applet.html`](https://docs.oracle.com/javase/tutorial/uiswing/components/applet.html)

本节涵盖了`JApplet`  一个使 applets 能够使用 Swing 组件的类。`JApplet`是[`java.applet.Applet`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html)的子类，该类在 Java Applets 教程中有介绍。如果你以前从未编写过常规 applet，我们建议你在继续本节之前阅读该教程。该教程中提供的信息也适用于 Swing applets，但本节会解释一些例外情况。

任何包含 Swing 组件的 applet 必须使用[`JApplet`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html)的子类来实现。这里是一个帮助 Java 走红的 applet 的 Swing 版本  一个动画 applet（在其最著名的配置中）展示了我们的吉祥物 Duke 在做空翻：

//&lt;![CDATA[ var attributes = { archive: 'https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TumbleItemProject/TumbleItem.jar', codebase: 'https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TumbleItemProject', code:'components.TumbleItem', width:'600', height:'95' }; var parameters = { permissions:'sandbox', nimgs:'17', offset:'-57', img: 'images/tumble', maxwidth:'120' }; deployJava.runApplet(attributes, parameters, '1.7'); //]]&gt;

* * *

**注意：** 如果你看不到 applet 运行，你需要安装至少[Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

你可以在`TumbleItem.java`中找到这个 applet 的主要源代码。

本节讨论以下主题：

+   JApplet 提供的功能

+   Applets 中的线程

+   在 Swing Applet 中使用图像

+   在 HTML 页面中嵌入 Applet

+   JApplet API

+   Applet 示例

## `JApplet` 提供的功能

因为`JApplet`是一个顶层的 Swing 容器，每个 Swing applet 都有一个根窗格。根窗格存在的最明显效果是支持添加菜单栏和需要使用内容窗格。

如使用顶层容器中所述，每个顶层容器（如`JApplet`）都有一个内容窗格。内容窗格使得 Swing applets 与常规 applets 有以下不同之处：

+   你将组件添加到 Swing applet 的内容窗格，而不是直接添加到 applet。向内容窗格添加组件展示了如何操作。

+   你在 Swing applet 的内容窗格上设置布局管理器，而不是直接在 applet 上设置。

+   Swing applet 的内容窗格的默认布局管理器是`BorderLayout`。这与`Applet`的默认布局管理器`FlowLayout`不同。

+   您不应该直接在`JApplet`对象中放置绘图代码。请参考执行自定义绘图了解如何在小程序中执行自定义绘图的示例。

## 小程序中的线程

Swing 组件应该在事件分派线程上创建、查询和操作，但浏览器不会从该线程调用小程序的“里程碑”方法。因此，“里程碑”方法——`init`、`start`、`stop`和`destroy`应该使用`SwingUtilities`方法`invokeAndWait`（或者如果适用，`invokeLater`），以便引用 Swing 组件的代码在事件分派线程上执行。有关这些方法和事件分派线程的更多信息，请参考 Swing 中的并发。

这里是一个`init`方法的示例：

```java
public void init() {
    //Execute a job on the event-dispatching thread:
    //creating this applet's GUI.
    try {
        javax.swing.SwingUtilities.invokeAndWait(new Runnable() {
            public void run() {
                createGUI();
            }
        });
    } catch (Exception e) {
        System.err.println("createGUI didn't successfully complete");
    }
}

private void createGUI() {
    JLabel label = new JLabel(
                       "You are successfully running a Swing applet!");
    label.setHorizontalAlignment(JLabel.CENTER);
    label.setBorder(BorderFactory.createMatteBorder(1,1,1,1,Color.black));
    getContentPane().add(label, BorderLayout.CENTER);
}

```

`invokeLater`方法不适用于这个实现，因为它允许`init`在初始化完成之前返回，这可能会导致难以调试的小程序问题。

`TumbleItem`中的`init`方法更复杂，如下面的代码所示。与第一个示例一样，这个`init`方法的实现使用`SwingUtilities.invokeAndWait`在事件分派线程上执行 GUI 创建代码。这个`init`方法设置了一个 Swing 定时器来触发动画更新的动作事件。此外，`init`使用[`javax.swing.SwingWorker`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html)创建一个后台任务，加载动画图像文件，让小程序立即呈现 GUI，而不必等待所有资源加载完毕。

```java
private void createGUI() {
    ...
    animator = new Animator();
    animator.setOpaque(true);
    animator.setBackground(Color.white);
    setContentPane(animator);
    ...
}

public void init() {
    loadAppletParameters();

    //Execute a job on the event-dispatching thread:
    //creating this applet's GUI.
    try {
        javax.swing.SwingUtilities.invokeAndWait(new Runnable() {
            public void run() {
                createGUI();
            }
        });
    } catch (Exception e) { 
        System.err.println("createGUI didn't successfully complete");
    }

    //Set up the timer that will perform the animation.
    timer = new javax.swing.Timer(speed, this);
    timer.setInitialDelay(pause);
    timer.setCoalesce(false);
    timer.start(); //Start the animation.

    //Background task for loading images.
    SwingWorker worker = (new SwingWorker<ImageIcon[], Object>() {
            public ImageIcon[] doInBackground() {
                final ImageIcon[] innerImgs = new ImageIcon[nimgs];
            *...//Load all the images...*
            return imgs;
        }
        public void done() {
            //Remove the "Loading images" label.
            animator.removeAll();
            loopslot = -1;
            try {
                imgs = get();
            } *...//Handle possible exceptions*
        }

    }).execute();
}

```

您可以在`TumbleItem.java`中找到小程序的源代码。要找到小程序所需的所有文件，请参阅示例索引。

## 在 Swing 小程序中使用图像

`Applet`类提供了`getImage`方法来将图片加载到小程序中。`getImage`方法创建并返回一个代表加载图片的`Image`对象。因为 Swing 组件使用`Icon`而不是`Image`来引用图片，所以 Swing 小程序往往不使用`getImage`。相反，Swing 小程序创建`ImageIcon`的实例，从图像文件加载图标。`ImageIcon`具有一个节省代码的好处：它自动处理图像跟踪。更多信息请参考如何使用图标。

Duke 做空翻的动画需要 17 张不同的图片。该小程序使用每张图片一个`ImageIcon`，并在其`init`方法中加载这些图片。由于图片加载可能需要很长时间，这些图标是在一个`SwingWorker`对象实现的单独线程中加载的。以下是代码：

```java
public void init() {
    ...
    imgs = new ImageIcon[nimgs];
    (new SwingWorker<ImageIcon[], Object>() {
        public ImageIcon[] doInBackground() {
            //Images are numbered 1 to nimgs,
            //but fill array from 0 to nimgs-1.
            for (int i = 0; i < nimgs; i++) {
                imgs[i] = loadImage(i+1);
            }
            return imgs;
        }
        ...
    }).execute();

}
...
protected ImageIcon loadImage(int imageNum) {
    String path = dir + "/T" + imageNum + ".gif";
    int MAX_IMAGE_SIZE = 2400;  //Change this to the size of
                                 //your biggest image, in bytes.
    int count = 0;
    BufferedInputStream imgStream = new BufferedInputStream(
       this.getClass().getResourceAsStream(path));
    if (imgStream != null) {
        byte buf[] = new byte[MAX_IMAGE_SIZE];
        try {
            count = imgStream.read(buf);
            imgStream.close();
        } catch (java.io.IOException ioe) {
            System.err.println("Couldn't read stream from file: " + path);
            return null;
        }
        if (count <= 0) {
            System.err.println("Empty file: " + path);
            return null;
        }
        return new ImageIcon(Toolkit.getDefaultToolkit().createImage(buf));
    } else {
        System.err.println("Couldn't find file: " + path);
        return null;
    }
}

```

`loadImage` 方法为指定的动画帧加载图像。它使用 `getResourceAsStream` 方法而不是通常的 `getResource` 方法来获取图像。生成的代码不太美观，但是对于从 JAR 文件加载图像到使用 Java Plug-in™ 软件执行的小程序来说，`getResourceAsStream` 比 `getResource` 更有效。有关更多详细信息，请参见将图像加载到小程序中。

## 在 HTML 页面中嵌入小程序

您可以使用 `applet` 标签部署一个简单的小程序。或者，您可以使用部署工具包。以下是翻筋斗的杜克小程序的代码：

```java
<script src="https://www.java.com/js/deployJava.js" type="text/javascript">
</script><script type="text/javascript">
//<![CDATA[
    var attributes = { archive: 'https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TumbleItemProject/TumbleItem.jar',
                       codebase: 'https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TumbleItemProject',
                       code:'components.TumbleItem', width:'600', height:'95' };
    var parameters = { permissions:'sandbox', nimgs:'17', offset:'-57',
                       img: 'images/tumble', maxwidth:'120' };
    deployJava.runApplet(attributes, parameters, '1.7');
//]]>
</script><noscript>A browser with Javascript enabled is required for this page to operate properly.</noscript>

```

有关更多信息，请参见部署小程序中的 Java 小程序课程。

## 小程序 JApplet API

下表列出了 `JApplet` 添加到小程序 API 中的有趣方法。它们让您访问根窗格提供的功能。您可能使用的其他方法由 [`Component`](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html) 和 [`Applet`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html) 类定义。请参见组件方法以获取常用的 `Component` 方法列表，并参见 Java 小程序以获取有关使用 `Applet` 方法的帮助。

| 方法 | 目的 |
| --- | --- |
| [void setContentPane(Container)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#setContentPane-java.awt.Container-) [Container getContentPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#getContentPane--) | 设置或获取小程序的内容窗格。内容窗格包含小程序的可见 GUI 组件，应该是不透明的。 |
| [void setRootPane(JRootPane)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#setRootPane-javax.swing.JRootPane-) [JRootPane getRootPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#getRootPane--) | 创建、设置或获取小程序的根窗格。根窗格管理小程序的内部，包括内容窗格、玻璃窗格等。 |
| [void setJMenuBar(JMenuBar)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#setJMenuBar-javax.swing.JMenuBar-) [JMenuBar getJMenuBar()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#getJMenuBar--) | 设置或获取小程序的菜单栏以管理小程序的一组菜单。 |
| [void setGlassPane(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#setGlassPane-java.awt.Component-) [Component getGlassPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#getGlassPane--) | 设置或获取小程序的玻璃窗格。您可以使用玻璃窗格拦截鼠标事件。 |
| [void setLayeredPane(JLayeredPane)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#setLayeredPane-javax.swing.JLayeredPane-) [JLayeredPane getLayeredPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html#getLayeredPane--) | 设置或获取 applet 的分层窗格。您可以使用 applet 的分层窗格将组件放在其他组件的前面或后面。 |

## Applet 示例

这个表格展示了 Swing applet 的示例以及这些示例在哪里描述。

| 示例 | 描述位置 | 备注 |
| --- | --- | --- |
| `TumbleItem` | 本页 | 一个动画 applet |
