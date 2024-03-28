# 开发 Java Web Start 应用程序

> 原文：[`docs.oracle.com/javase/tutorial/deployment/webstart/developing.html`](https://docs.oracle.com/javase/tutorial/deployment/webstart/developing.html)

使用基于组件的架构设计的软件可以轻松地开发和部署为 Java Web Start 应用程序。考虑具有基于 Swing 的图形用户界面（GUI）的 Java Web Start 应用程序的示例。通过基于组件的设计，GUI 可以使用更小的构建块或组件构建。以下是用于创建应用程序 GUI 的一般步骤：

+   创建一个`MyTopJPanel`类，它是`JPanel`的子类。在`MyTopJPanel`类的构造函数中布局应用程序的 GUI 组件。

+   创建一个名为`MyApplication`的类，它是`JFrame`类的子类。

+   在`MyApplication`类的`main`方法中，实例化`MyTopJPanel`类，并将其设置为`JFrame`的内容窗格。

以下各节通过使用 Dynamic Tree Demo 应用程序更详细地探讨了这些步骤。如果您对 Swing 不熟悉，请参阅使用 Swing 创建 GUI 以了解更多关于使用 Swing GUI 组件的信息。

单击以下启动按钮以启动 Dynamic Tree Demo 应用程序。

//&lt;![CDATA[ var url = "https://docs.oracle.com/javase/tutorialJWS/samples/deployment/dynamictree_webstartJWSProject/dynamictree_webstart.jnlp"; deployJava.createWebStartLaunchButton(url, '1.7.0'); //]]&gt;

* * *

**注意：** 如果您看不到示例运行，请确保在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

## 创建顶级`JPanel`类

创建一个`JPanel`的子类。这个顶级`JPanel`充当所有其他 UI 组件的容器。在下面的示例中，`DynamicTreePanel`类是最顶层的`JPanel`。`DynamicTreePanel`类的构造函数调用其他方法来正确创建和布局 UI 控件。

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
        ....

        JButton clearButton = new JButton("Clear");
        ...

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

## 创建应用程序

对于具有基于 Swing 的 GUI 的应用程序，请创建一个是`javax.swing.JFrame`的子类的类。

在应用程序的`main`方法中，实例化您的顶级`JPanel`类，并将其设置为`JFrame`的内容窗格。`DynamicTreeApplication`类的`main`方法在 AWT 事件分发线程中调用`createGUI`方法。

```java
package webstartComponentArch;

import javax.swing.JFrame;

public class DynamicTreeApplication extends JFrame {
    public static void main(String [] args) {
        DynamicTreeApplication app = new DynamicTreeApplication();
        app.createGUI();
    }

    private void createGUI() {
        //Create and set up the content pane.
        DynamicTreePanel newContentPane = new DynamicTreePanel();
        newContentPane.setOpaque(true); 
        setContentPane(newContentPane);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        pack();
        setVisible(true);
    }    
}

```

## 将核心功能与最终部署机制分离的好处

另一种创建应用程序的方法是去掉抽象层（单独的顶级`JPanel`）并在应用程序的`main`方法中布局所有控件。直接在应用程序的`main`方法中创建 GUI 的缺点是，如果以后选择将功能部署为小程序，部署将更加困难。

在动态树演示示例中，核心功能被分离到`DynamicTreePanel`类中。现在，将`DynamicTreePanel`类放入`JApplet`中并将其部署为小程序变得非常简单。

因此，为了保持可移植性并保持部署选项开放，请按照本主题中描述的基于组件的设计方法进行操作。

下载源代码以进一步进行*动态树演示*示例的实验。
