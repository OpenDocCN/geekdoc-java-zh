# 部署一个 Applet

> 译文：[`docs.oracle.com/javase/tutorial/deployment/applet/deployingApplet.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/deployingApplet.html)

要部署您的 Java applet，首先编译源代码，将其打包为 JAR 文件，并对 JAR 文件进行签名。

Java applet 可以以两种方式启动。

+   您可以使用 Java 网络启动协议（JNLP）启动您的 applet。使用 JNLP 启动的 applet 可以访问强大的 JNLP API 和扩展。

+   或者，您可以通过直接在 applet 标记中指定 applet 的启动属性来启动 applet。然而，这种部署 applet 的旧方法对 applet 施加了严格的安全限制。

部署工具包脚本包含可用于在网页中部署 applet 的有用 JavaScript 函数。

如果您对这些部署技术不熟悉，请在继续之前查看 深入部署 课程。

以下是一些逐步说明，用于打包和部署您的 applet。Dynamic Tree Demo applet 用于说明 applet 的部署。您可能希望设置构建脚本来执行以下一些步骤。

//&lt;![CDATA[ var attributes = { code:'appletComponentArch.DynamicTreeApplet.class', archive:'examples/dist/applet_ComponentArch_DynamicTreeDemo/DynamicTreeDemo.jar', width:300, height:300} ; var parameters = {jnlp_href: 'examples/dist/applet_ComponentArch_DynamicTreeDemo/dynamictree_applet.jnlp'} ; deployJava.runApplet(attributes, parameters, '1.7'); //]]&gt;

* * *

**注意：** 如果您看不到示例运行，请确保在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

1.  编译您的 applet 的 Java 代码，并确保所有类文件和资源（如图像）位于单独的目录中。

    在 DynamicTree Demo applet 的情况下，编译后的类文件将放置在 `build/classes/appletComponentArch` 目录中。

1.  创建一个包含您的 applet 需要的任何 JAR 文件清单属性的文本文件。

    对于 DynamicTree Demo applet，创建一个名为 `mymanifest.txt` 的文件在 `build/classes` 目录中，并添加 `Permissions`、`Codebase` 和 `Application-Name` 属性。该 applet 不需要访问用户系统资源，因此对于权限使用 `sandbox`。对于代码库，使用您将加载示例的域，例如 `myserver.com`。将以下属性添加到 `mymanifest.txt` 文件中。

    ```java
    Permissions: sandbox
    Codebase: myserver.com
    Application-Name: Dynamic Tree Demo

    ```

    其他清单属性可用于限制 applet 仅使用受信任的代码，并为需要在特权 Java 代码和沙箱 Java 代码之间进行调用，或者具有调用 applet 的 JavaScript 代码的 applet 提供安全性。请参阅 使用清单属性增强安全性 课程以了解更多可用的清单属性。

1.  创建一个包含您 applet 的类文件和资源的 JAR 文件。在您之前创建的 `mymanifest.txt` 文件中包含清单属性。

    例如，以下命令将创建一个 JAR 文件，其中包含 `build/classes/appletComponentArch` 目录中的类文件和 `build/classes` 目录中的清单文件。

    ```java
    % cd build/classes
    % jar cvfm DynamicTreeDemo.jar mymanifest.txt appletComponentArch

    ```

    参阅 在 JAR 文件中打包程序 课程，了解有关创建和使用 JAR 文件的更多信息。

1.  为您的 applet 签署 JAR 文件并为签名加上时间戳。使用由受信任的证书颁发机构颁发的有效的当前代码签名证书，向用户提供确保可以安全运行 applet 的保证。

    请参阅 签署 JAR 文件 课程以获取更多信息。

    如果您想要使用签名的 JNLP 文件以确保安全性，请按照下一步中描述的方式创建 JNLP 文件，并在签名 JAR 文件之前将其包含在 JAR 文件中。有关详细信息，请参阅 Java 平台标准版部署指南中的 [签名的 JNLP 文件](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/signedJNLP.html)。

1.  创建描述您的 applet 应如何启动的 JNLP 文件。

    这是用于启动动态树演示 applet 的 JNLP 文件。

    ``dynamictree_applet.jnlp`` 的源代码如下：

    ```java

    <?xml version="1.0" encoding="UTF-8"?>
    <jnlp spec="1.0+" codebase="" href="">
        <information>
            <title>Dynamic Tree Demo</title>
            <vendor>Dynamic Team</vendor>
        </information>
        <resources>
            <!-- Application Resources -->
            <j2se version="1.7+"
                href="http://java.sun.com/products/autodl/j2se" />
            <jar href="DynamicTreeDemo.jar" main="true" />

        </resources>
        <applet-desc 
             name="Dynamic Tree Demo Applet"
             main-class="components.DynamicTreeApplet"
             width="300"
             height="300">
         </applet-desc>
         <update check="background"/>
    </jnlp>                                   

    ```

    请注意，JNLP 文件中不存在请求额外权限的安全元素，因此 applet 仅在安全沙箱中运行。

    主题，JNLP 文件的结构，描述了 JNLP 文件的语法和选项。

1.  创建将显示 applet 的 HTML 页面。调用部署工具包函数以部署 applet。

    在我们的示例中，动态树演示 applet 部署在 ``AppletPage.html`` 中。

    ```java
    <body>
        <!-- ... -->
        <script src="https://www.java.com/js/deployJava.js"></script>
        <script> 
            var attributes = {
                code:'components.DynamicTreeApplet',  width:300, height:300} ; 
            var parameters = {jnlp_href: 'dynamictree_applet.jnlp'} ; 
            deployJava.runApplet(attributes, parameters, '1.7'); 
        </script>
        <!-- ... -->
    </body>

    ```

1.  将 applet 的 JAR 文件、JNLP 文件和 HTML 页面放置在适当的文件夹中。

    对于此示例，请将 `DynamicTreeDemo.jar`、`dynamictree_applet.jnlp` 和 `AppletPage.html` 放置在本地计算机或 Web 服务器上的同一目录中。建议使用 Web 服务器。要从本地计算机运行，必须将应用程序添加到例外站点列表中，该列表可从 Java 控制面板的安全选项卡中管理。

1.  在浏览器中打开 applet 的 HTML 页面以查看 applet。在提示时同意运行 applet。检查 Java 控制台日志以查看错误和调试消息。

下载源代码 以进一步尝试 *动态树演示 applet* 示例。
