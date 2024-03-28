# 部署 Java Web Start 应用程序

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/createWebStartLaunchButtonFunction.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/createWebStartLaunchButtonFunction.html)

您可以使用[部署工具包](https://www.java.com/js/deployJava.txt)脚本的`createWebStartLaunchButton`函数部署 Java Web Start 应用程序。Java Web Start 应用程序使用 Java 网络启动协议（JNLP）启动。`createWebStartLaunchButton`函数生成一个链接（HTML 锚标签 - `<a>`）到 Java Web Start 应用程序的 JNLP 文件。

生成的锚标签是 Java Web Start 应用程序的![启动按钮](img/4707a69a17729d71c56b2bdbbb4cc61c.png)按钮。当最终用户点击启动按钮时，部署工具包脚本确保已安装适当的 Java 运行时环境（JRE）软件，然后启动 Java Web Start 应用程序。

* * *

**注意：**根据浏览器类型，当您尝试查看网页源代码时，可能无法查看部署工具包脚本生成的 HTML。要查看生成的 HTML，请尝试在加载后保存 HTML 页面，或使用诸如 Firebug（Mozilla Firefox 附加组件）之类的工具。

* * *

* * *

**注意：**如果客户端没有所需的 JRE 软件最低版本，部署工具包脚本会将浏览器重定向到`http://www.java.com`，以允许用户下载最新的 JRE 软件。

* * *

**函数签名：**`createWebStartLaunchButton: function(jnlp, minimumVersion)`或`createWebStartLaunchButton: function(jnlp)`

参数：

+   `jnlp` – 包含 Java Web Start 应用程序部署信息的 JNLP 文件的 URL。此 URL 应为绝对路径。

+   `minimumVersion` – 运行此应用程序所需的 JRE 软件的最低版本

用法：

+   指定运行应用程序所需的 JRE 软件的最低版本

    ```java
    <script src="https://www.java.com/js/deployJava.js"></script>
    <script>
        var url = "http://java.sun.com/javase/technologies/desktop/javawebstart/apps/notepad.jnlp";
        deployJava.createWebStartLaunchButton(url, '1.6.0');
    </script>

    ```

+   使 Java Web Start 应用程序能够在任何 JRE 软件版本上运行

    如果您的应用程序没有最低 JRE 软件版本要求，请使用`createWebStartLaunchButton: function(jnlp)`函数部署 Java Web Start 应用程序。

* * *

**注意：**当使用任何先前描述的`createWebStartLaunchButton`函数部署时，必须在 Java Web Start 应用程序的 JNLP 文件中指定绝对的 codebase。这样可以通过`javaws <path/to/local JNLP file>`命令从命令行启动 Java Web Start 应用程序。

* * *
