# 开发 Applet

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/developingApplet.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/developingApplet.html)

使用基于组件的架构设计的应用程序可以开发为 Java applet。考虑具有基于 Swing 的图形用户界面（GUI）的 Java applet 的示例。通过组件化设计，GUI 可以使用更小的构建块或组件构建。以下是用于创建 applet GUI 的一般步骤：

+   创建一个名为`MyTopJPanel`的类，它是`javax.swing.JPanel`的子类。在`MyTopJPanel`类的构造函数中布置您的 applet 的 GUI 组件。

+   创建一个名为`MyApplet`的类，它是`javax.swing.JApplet`的子类。

+   在`MyApplet`的`init`方法中，实例化`MyTopJPanel`并将其设置为 applet 的内容窗格。

以下部分通过使用动态树演示 applet 更详细地探讨这些步骤。如果您对 Swing 不熟悉，请参阅使用 Swing 创建 GUI 以了解更多关于使用 Swing GUI 组件的信息。

//&lt;![CDATA[ var attributes = { code:'appletComponentArch.DynamicTreeApplet.class', archive:'examples/dist/applet_ComponentArch_DynamicTreeDemo/DynamicTreeDemo.jar', width:300, height:300} ; var parameters = {jnlp_href: 'examples/dist/applet_ComponentArch_DynamicTreeDemo/dynamictree_applet.jnlp'} ; deployJava.runApplet(attributes, parameters, '1.4'); //]]&gt;

* * *

**注意：** 如果您看不到示例运行，请确保在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

## 创建顶部`JPanel`类

创建一个是`JPanel`子类的类。这个顶部`JPanel`充当所有其他 UI 组件的容器。在下面的示例中，`DynamicTreePanel`类是最顶层的`JPanel`。`DynamicTreePanel`类的构造函数调用其他方法来正确创建和布局 UI 控件。

```java
public class DynamicTreePanel extends JPanel implements ActionListener {
    private int newNodeSuffix = 1;
    private static String ADD_COMMAND = "add";
    private static String REMOVE_COMMAND = "remove";
    private static String CLEAR_COMMAND = "clear";

    private DynamicTree treePanel;

    public DynamicTreePanel() {
        super(new BorderLayout());

        //Create the components.
        treePanel = new DynamicTree();
        populateTree(treePanel);

        JButton addButton = new JButton("Add");
        addButton.setActionCommand(ADD_COMMAND);
        addButton.addActionListener(this);

        JButton removeButton = new JButton("Remove");

        // ...

        JButton clearButton = new JButton("Clear");

        // ...

        //Lay everything out.
        treePanel.setPreferredSize(
            new Dimension(300, 150));
        add(treePanel, BorderLayout.CENTER);

        JPanel panel = new JPanel(new GridLayout(0,3));
        panel.add(addButton);
        panel.add(removeButton); 
        panel.add(clearButton);
        add(panel, BorderLayout.SOUTH);
    }
    // ...
}

```

## 创建 Applet

对于具有基于 Swing 的 GUI 的 Java applet，请创建一个类，它是`javax.swing.JApplet`的子类。不包含基于 Swing 的 GUI 的 applet 可以扩展`java.applet.Applet`类。

覆盖 applet 的`init`方法以实例化您的顶部`JPanel`类并创建 applet 的 GUI。`DynamicTreeApplet`类的`init`方法在 AWT 事件分发线程中调用`createGUI`方法。

```java
package appletComponentArch;

import javax.swing.JApplet;
import javax.swing.SwingUtilities;

public class DynamicTreeApplet extends JApplet {
    //Called when this applet is loaded into the browser.
    public void init() {
        //Execute a job on the event-dispatching thread; creating this applet's GUI.
        try {
            SwingUtilities.invokeAndWait(new Runnable() {
                public void run() {
                    createGUI();
                }
            });
        } catch (Exception e) { 
            System.err.println("createGUI didn't complete successfully");
        }
    }

    private void createGUI() {
        //Create and set up the content pane.
        DynamicTreePanel newContentPane = new DynamicTreePanel();
        newContentPane.setOpaque(true); 
        setContentPane(newContentPane);        
    }        
}

```

## 将核心功能与最终部署机制分离的好处

另一种创建 applet 的方法是只需移除抽象层（单独的顶部`JPanel`）并在 applet 的`init`方法中布置所有控件。直接在 applet 中创建 GUI 的缺点是，如果以后选择将功能部署为 Java Web Start 应用程序，则现在将更难部署您的功能。

在动态树演示示例中，核心功能位于`DynamicTreePanel`类中。现在，将`DynamicTreePanel`类放入`JFrame`并部署为 Java Web Start 应用程序变得轻而易举。

因此，为了保持可移植性并保持部署选项开放，请按照本页描述的基于组件的设计。

下载源代码以进一步实验*动态树演示程序*示例。
