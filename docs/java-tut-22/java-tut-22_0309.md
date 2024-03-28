# 在没有 Codebase 的情况下部署

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/deployingWithoutCodebase.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/deployingWithoutCodebase.html)

从 Java SE 7 发行版开始，您不必为 Java Web Start 应用程序的 Java 网络启动协议 (JNLP) 文件中的`codebase`属性指定绝对路径。您可以在不修改`codebase`属性中的路径的情况下在不同环境中开发和测试应用程序。如果未指定 codebase，则 Java Web Start 软件会假定 codebase 相对于启动 Java Web Start 应用程序的网页。

当 JNLP 文件不包含`codebase`属性时，可以使用部署工具脚本的以下函数在网页中部署 Java Web Start 应用程序：

+   `launchWebStartApplication` – 在 HTML 链接中使用此函数部署您的 Java Web Start 应用程序。

+   `createWebStartLaunchButtonEx` – 使用此函数为您的 Java Web Start 应用程序创建一个启动按钮。

* * *

**注意:** 要运行通过先前指定的函数部署的 Java Web Start 应用程序，至少需要 Java SE 7 发行版。如果客户端没有至少 Java SE 7 发行版，函数会指示用户在启动 Java Web Start 应用程序之前安装所需的 Java Runtime Environment (JRE) 软件。

* * *

**函数签名:** `launchWebStartApplication: function(jnlp)`

参数:

`jnlp` – 包含 Java Web Start 应用程序部署信息的 JNLP 文件的路径。此路径可以相对于部署 Java Web Start 应用程序的网页。

用法:

在以下示例中，`launchWebStartApplication`函数在 HTML `anchor (a)` 标签的`href`属性中被调用。

``dynamictree_webstart_no_codebase.jnlp`` JNLP 文件用于部署动态树演示应用程序。

```java
<script src="https://www.java.com/js/deployJava.js"></script>
<a href="javascript:deployJava.launchWebStartApplication('dynamictree_webstart_no_codebase.jnlp');">Launch</a>

```

当用户点击生成的 HTML 链接时，将启动 Java Web Start 应用程序。

**函数签名:** `createWebStartLaunchButtonEx: function(jnlp)`

参数:

`jnlp` – 包含 Java Web Start 应用程序部署信息的 JNLP 文件的路径。此路径可以相对于部署 Java Web Start 应用程序的网页。

用法:

以下示例展示了`createWebStartLaunchButtonEx`函数的用法。

``dynamictree_webstart_no_codebase.jnlp`` JNLP 文件用于部署动态树演示应用程序。

```java
<script src="https://www.java.com/js/deployJava.js"></script>
<script>        
    var jnlpFile = "dynamictree_webstart_no_codebase.jnlp";
    deployJava.createWebStartLaunchButtonEx(jnlpFile);
</script>

```

当用户点击生成的启动按钮时，将启动 Java Web Start 应用程序。

在浏览器中打开``JavaWebStartAppPage_No_Codebase.html``以查看通过本主题中描述的功能部署的动态树演示应用程序。

* * *

**注意：**

您还可以通过在系统命令提示符中使用完整的 JNLP 文件的 URL 调用`javaws`命令来启动 Java Web Start 应用程序，如下面的代码片段所示。

```java
javaws http://example.com/dynamictree_webstart_no_codebase.jnlp

```

* * *

下载源代码以进一步进行实验的*动态树演示*示例。
