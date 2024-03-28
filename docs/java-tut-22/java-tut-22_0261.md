# 小程序的生命周期

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/lifeCycle.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/lifeCycle.html)

小程序可以对以下重大事件做出反应：

+   它可以*初始化*自身。

+   它可以*开始*运行。

+   它可以*停止*运行。

+   它可以执行*最终清理*，为即将卸载做准备。

本节介绍了一个新的小程序`Simple`，它使用了所有这些方法。与 Java 应用程序不同，小程序不需要实现`main`方法。

这是`Simple`小程序。

//&lt;![CDATA[ var attributes = { code:'Simple.class', archive:'examples/dist/applet_Simple/applet_Simple.jar', width:500, height:20} ; var parameters = { permissions:'sandbox' } ; deployJava.runApplet(attributes, parameters, '1.4'); //]]&gt;

* * *

**注意：** 如果您看不到示例运行，请确保在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

以下是`Simple`小程序的源代码。该小程序在其生命周期中遇到重要里程碑时会显示描述性字符串，例如当用户首次访问包含小程序的页面时。

```java

import java.applet.Applet;
import java.awt.Graphics;

//No need to extend JApplet, since we don't add any components;
//we just paint.
public class Simple extends Applet {

    StringBuffer buffer;

    public void init() {
        buffer = new StringBuffer();
        addItem("initializing... ");
    }

    public void start() {
        addItem("starting... ");
    }

    public void stop() {
        addItem("stopping... ");
    }

    public void destroy() {
        addItem("preparing for unloading...");
    }

    private void addItem(String newWord) {
        System.out.println(newWord);
        buffer.append(newWord);
        repaint();
    }

    public void paint(Graphics g) {
	//Draw a Rectangle around the applet's display area.
        g.drawRect(0, 0, 
		   getWidth() - 1,
		   getHeight() - 1);

	//Draw the current string inside the rectangle.
        g.drawString(buffer.toString(), 5, 15);
    }
}

```

* * *

**注意：** 在此示例中，`Applet` 类被扩展，而不是 Swing `JApplet` 类，因为 Swing 组件不需要添加到此小程序中。

* * *

## 加载小程序

由于小程序被加载，您应该看到文本“初始化...启动...”。小程序加载时，会发生以下情况：

+   创建小程序控制类的一个实例（`Applet`子类）。

+   小程序初始化自身。

+   小程序开始运行。

## 离开并返回到小程序的页面

当用户离开页面，例如前往另一页时，浏览器会停止并销毁小程序。小程序的状态不会被保留。当用户返回页面时，浏览器会初始化并启动小程序的新实例。

## 重新加载小程序

当您刷新或重新加载浏览器页面时，当前小程序实例会被停止和销毁，并创建一个新实例。

## 退出浏览器

当用户退出浏览器时，小程序有机会在浏览器退出之前停止自身并执行最终清理。

下载源代码以进一步实验*Simple 小程序*示例。
