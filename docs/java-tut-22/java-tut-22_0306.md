# 在小程序标签中嵌入 JNLP 文件

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/embeddingJNLPFileInWebPage.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/embeddingJNLPFileInWebPage.html)

当使用 Java Network Launch Protocol (JNLP) 部署小程序时，Java 插件软件会在从网络下载 JNLP 文件后启动小程序。从 Java SE 7 版本开始，您可以通过将 JNLP 文件嵌入到网页中来减少小程序启动时间，从而避免第一次加载小程序时额外的网络请求。这将导致小程序在 Web 浏览器上快速启动。

在网页中部署小程序时，可以将 Base64 编码的 JNLP 文件嵌入到 `jnlp_embedded` 参数中。`<jnlp>` 元素的属性应满足以下限制：

+   `href` 属性应包含相对路径。

+   不应指定 `codebase` 属性。这意味着代码库将从加载小程序的网页的 URL 派生。

以下步骤描述了如何在网页中嵌入 JNLP 文件以部署小程序。

1.  为您的小程序创建一个``JNLP``文件。下面显示了一个示例文件。

    ```java
    <?xml version="1.0" encoding="UTF-8"?>
    <!-- href attribute contains relative path;
         codebase attribute not specified -->
    <jnlp href="dynamictree_applet.jnlp">
        <information>
            <title>Dynamic Tree Demo</title>
            <vendor>Dynamic Team</vendor>
        </information>
        <resources>
            <!-- Application Resources -->
            <j2se version="1.7+" />
            <jar href=
                "dist/applet_ComponentArch_DynamicTreeDemo/DynamicTreeDemo.jar" 
                 main="true" />
        </resources>
        <applet-desc 
             name="Dynamic Tree Demo Applet"
             main-class="appletComponentArch.DynamicTreeApplet"
             width="300"
             height="300">
         </applet-desc>
         <update check="background"/>
    </jnlp>

    ```

1.  使用 Base64 方案对 JNLP 文件的内容进行编码。您可以使用任何 Base64 编码工具对 JNLP 文件进行编码。查看工具的用法以创建具有 Base64 编码的字符串。可以使用的一些工具和网站示例如下：

    +   UNIX 命令 – `base64`, `uuencode`

    +   网站 – [Base64 编码和解码](http://base64encode.org/), [Base64 编码器](http://www.opinionatedgeek.com/dotnet/tools/base64encode/)

1.  在网页中部署小程序时，指定 `jnlp_embedded` 参数，并将其值设置为 Base64 编码的 JNLP 字符串。确保只包含实际的 Base64 字节，不包含任何编码工具特定的头部或尾部。

    ```java
    <script src="https://www.java.com/js/deployJava.js"></script>
    <script>
        var attributes = {} ;
        <!-- Base64 encoded string truncated below for readability -->
        var parameters = {jnlp_href: 'dynamictree_applet.jnlp',
            jnlp_embedded: 'PCEtLSANCi8qDQogKiBDb ... bmxwPg=='
        } ;
        deployJava.runApplet(attributes, parameters, '1.6');
    </script>

    ```

    一些编码工具可能会将编码后的字符串包装成几个 76 列的行。要在 JavaScript 代码中使用这种多行属性值，请将属性值指定为一组连接的字符串。如果小程序直接使用 `<applet>` HTML 标签部署，则可以按原样包含多行属性值。

在浏览器中打开``AppletPage.html``以查看通过在网页中嵌入 JNLP 文件启动的动态树演示小程序。

* * *

**注意：** 如果您看不到小程序运行，请至少安装[Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

* * *

**注意：** 如果您看不到示例运行，可能需要在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

下载源代码以进一步进行实验的*嵌入式 JNLP* 示例。
