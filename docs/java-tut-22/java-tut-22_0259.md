# 定义一个小程序子类

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/subclass.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/subclass.html)

每个 Java 小程序都必须定义`Applet`或`JApplet`类的子类。在 Hello World 小程序中，这个子类被称为`HelloWorld`。以下是``HelloWorld``类的源代码。

```java

import javax.swing.JApplet;
import javax.swing.SwingUtilities;
import javax.swing.JLabel;

public class HelloWorld extends JApplet {
    //Called when this applet is loaded into the browser.
    public void init() {
        //Execute a job on the event-dispatching thread; creating this applet's GUI.
        try {
            SwingUtilities.invokeAndWait(new Runnable() {
                public void run() {
                    JLabel lbl = new JLabel("Hello World");
                    add(lbl);
                }
            });
        } catch (Exception e) {
            System.err.println("createGUI didn't complete successfully");
        }
    }
}

```

Java applets inherit significant functionality from the `Applet` or `JApplet` class, including the capabilities to communicate with the browser and present a graphical user interface (GUI) to the user.

一个将使用来自 Swing（Java 的 GUI 工具包）的 GUI 组件的小程序应该扩展[`javax.swing.JApplet`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html)基类，它提供了与 Swing 的 GUI 设施最佳集成。

`JApplet`提供了一个根窗格，它与 Swing 的`JFrame`和`JDialog`组件具有相同的顶层组件结构，而`Applet`只提供了一个基本面板。有关如何使用此功能的更多详细信息，请参见如何使用根窗格。

一个小程序可以扩展[`java.applet.Applet`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html)类，当它不使用 Swing 的 GUI 组件时。
