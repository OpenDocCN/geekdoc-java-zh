# 开始使用小程序

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/getStarted.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/getStarted.html)

接下来展示的 HelloWorld 小程序是一个显示字符串"Hello World"的 Java 类。

//&lt;![CDATA[ var attributes = { code:'HelloWorld.class', archive:'examples/dist/applet_HelloWorld/applet_HelloWorld.jar', width:150, height:30} ; var parameters = { permissions:'sandbox' } ; deployJava.runApplet(attributes, parameters, '1.4'); //]]&gt;

* * *

**注意：** 如果你看不到示例运行，可能需要在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

以下是 HelloWorld 小程序的源代码：

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

这样的小程序通常由浏览器中的*Java 插件*软件管理和运行。

下载源代码以进一步进行实验。
