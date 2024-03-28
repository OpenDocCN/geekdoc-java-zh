# 使用事件处理程序处理初始化状态

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/appletStatus.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/appletStatus.html)

在 applet 初始化之前，applet 无法处理来自网页中 JavaScript 代码的请求。从 JavaScript 代码调用 applet 方法或访问 applet 变量将被阻塞，直到 applet 的`init()`方法完成或 applet 首次从部署的网页中调用 JavaScript 代码。由于许多浏览器中 JavaScript 实现是单线程的，因此在 applet 启动期间，网页可能会出现冻结的情况。

从 JDK 7 版本开始，您可以在 applet 加载时检查`status`变量，以确定 applet 是否准备好处理来自 JavaScript 代码的请求。您还可以注册事件处理程序，在 applet 初始化的各个阶段自动调用。为了利用这个功能，应该将 applet 部署时的`java_status_events`参数设置为`"true"`。

在状态和事件处理程序示例中，JavaScript 代码向 applet 注册了一个`onLoad`处理程序。当 applet 初始化完成时，Java 插件软件会自动调用`onLoad`处理程序。`onLoad`处理程序调用 applet 的其他方法，在网页上绘制图形。``DrawingApplet`` 类的`init`方法休眠两秒，模拟长时间的 applet 初始化过程。

以下步骤描述了如何注册事件处理程序并检查 applet 的状态。请参阅[Applet 状态和事件处理程序](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/applet_dev_guide.html#JSDPG719)，了解可以注册事件处理程序的完整的 applet 状态值和 applet 事件列表。

1.  创建一个 JavaScript 函数来注册事件处理程序。以下代码片段显示了`registerAppletStateHandler`函数，如果 applet 尚未加载，则注册一个`onLoad`事件处理程序。

    ```java
    <script>
    <!-- ... -->
        var READY = 2;
        function registerAppletStateHandler() {
            // register onLoad handler if applet has
            // not loaded yet
            if (drawApplet.status < READY)  {                 
                drawApplet.onLoad = onLoadHandler;
            } else if (drawApplet.status >= READY) {
                // applet has already loaded or there
                // was an error
                document.getElementById("mydiv").innerHTML = 
                  "Applet event handler not registered because applet status is: "
                   + drawApplet.status;    
            }
        }

        function onLoadHandler() {
            // event handler for ready state
            document.getElementById("mydiv").innerHTML =
                "Applet has loaded";
            draw();
        }
    <!-- ... -->
    </script>        

    ```

1.  在`body`标签的 onload 方法中调用先前创建的`registerAppletStateHandler`函数。这样可以确保在注册 applet 的事件处理程序之前，applet 的 HTML 标签已经在网页的文档对象模型（DOM）树中创建。

    ```java
    <body onload="registerAppletStateHandler()">

    ```

1.  将 applet 部署时的`java_status_events`参数设置为`"true"`。

    ```java
    <script src=
      "https://www.java.com/js/deployJava.js"></script>
    <script>
        // set java_status_events parameter to true 
        var attributes = { id:'drawApplet',
            code:'DrawingApplet.class',
            archive: 'applet_StatusAndCallback.jar',
            width:600, height:400} ;
        var parameters = {java_status_events: 'true', permissions:'sandbox' } ;
        deployJava.runApplet(attributes, parameters, '1.7');
    </script>

    ```

在浏览器中打开``AppletPage.html``以查看 applet 事件处理程序的行为。在``AppletPageUpdatedDuringLoading.html``页面中，检查 applet 的`status`变量以确定 applet 是否已加载。根据状态，当 applet 被加载时，网页会持续更新。

* * *

**注意：** 如果您看不到 applet 运行，请安装至少 [Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 版本。

* * *

* * *

**注意：** 如果您看不到示例运行，您可能需要在浏览器中启用 JavaScript 解释器，以便 Deployment Toolkit 脚本能够正常运行。

* * *

下载源代码 以进一步进行实验的 *状态和事件处理程序* 示例。
