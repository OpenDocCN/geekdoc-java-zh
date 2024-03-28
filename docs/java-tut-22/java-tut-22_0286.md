# 部署 Java Web Start 应用程序

> 原文：[`docs.oracle.com/javase/tutorial/deployment/webstart/deploying.html`](https://docs.oracle.com/javase/tutorial/deployment/webstart/deploying.html)

要部署您的 Java Web Start 应用程序，首先编译源代码，将其打包为 JAR 文件，并对 JAR 文件进行签名。

Java Web Start 应用程序通过使用 Java 网络启动协议（JNLP）启动。因此，您必须创建一个 JNLP 文件来部署您的应用程序。

部署工具包脚本包含有用的 JavaScript 函数，可用于在网页上部署 Java Web Start 应用程序。

如果您对这些部署技术不熟悉，请在继续之前查看深入部署课程。

以下是一些逐步说明，用于打包和部署您的应用程序。动态树演示应用程序用于说明 Java Web Start 应用程序的部署。您可能希望设置构建脚本来执行以下一些步骤。

单击以下启动按钮以启动动态树演示应用程序。

//&lt;![CDATA[ var url = "https://docs.oracle.com/javase/tutorialJWS/samples/deployment/dynamictree_webstartJWSProject/dynamictree_webstart.jnlp" deployJava.createWebStartLaunchButton(url, '1.7.0'); //]]&gt;

* * *

**注意：** 如果您看不到示例运行，请确保在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

1.  编译您的应用程序的 Java 代码，并确保所有类文件和资源（如图像）位于单独的目录中。

    在动态树演示应用程序中，编译后的类文件放置在`build/classes/webstartComponentArch`目录中。

1.  创建一个包含您的小程序所需的任何 JAR 文件清单属性的文本文件。

    对于 DynamicTree 演示小程序，创建一个名为`mymanifest.txt`的文件，放在`build/classes`目录中，并添加`Permissions`，`Codebase`和`Application-Name`属性。该小程序不需要访问用户系统资源，因此对于权限使用`sandbox`。对于代码库，请使用将加载示例的域，例如`myserver.com`。将以下属性添加到`mymanifest.txt`文件中。

    ```java
    Permissions: sandbox
    Codebase: myserver.com
    Application-Name: Dynamic Tree Demo

    ```

    其他清单属性可用于限制小程序仅使用受信任的代码，并为需要在特权 Java 代码和沙箱 Java 代码之间进行调用，或具有调用小程序的 JavaScript 代码的小程序提供安全性。请参阅使用清单属性增强安全性课程，了解更多可用的清单属性。

1.  创建一个包含应用程序类文件和资源的 JAR 文件。在上一步中创建的`mymanifest.txt`文件中包含清单属性。

    例如，以下命令将在`build/classes/webstartComponentArch`目录中的类文件和`build/classes`目录中的清单文件创建一个 JAR 文件。

    ```java
    % cd build/classes
    % jar cvfm  DynamicTreeDemo.jar  mymanifest.txt webstartComponentArch

    ```

    参见在 JAR 文件中打包程序课程，了解有关创建和使用 JAR 文件的更多信息。

1.  为您的小程序签署 JAR 文件并为签名加上时间戳。使用由受信任的证书颁发机构颁发的有效、当前的代码签名证书，向您的用户提供确保可以安全运行小程序的保证。

    有关更多信息，请参见签署 JAR 文件课程。

    如果您想要为安全性使用已签名的 JNLP 文件，请按照下一步中的说明创建 JNLP 文件，并在签名 JAR 文件之前将其包含在 JAR 文件中。有关详细信息，请参阅[已签名的 JNLP 文件](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/signed_jnlp.html)中的 Java 平台标准版部署指南。

1.  创建一个描述应用程序如何启动的 JNLP 文件。

    以下是用于启动动态树演示应用程序的 JNLP 文件。对于此应用程序，不会请求权限，因此它在安全沙箱中运行。``dynamictree_webstart.jnlp``的源代码如下：

    ```java

    <?xml version="1.0" encoding="UTF-8"?>
    <jnlp spec="1.0+" codebase=
    "https://docs.oracle.com/javase/tutorialJWS/samples/deployment/webstart_ComponentArch_DynamicTreeDemo" 
        href="dynamictree_webstart.jnlp">
        <information>
            <title>Dynamic Tree Demo</title>
            <vendor>Dynamic Team</vendor>
        </information>
        <resources>
            <!-- Application Resources -->
            <j2se version="1.7+"
                  href="http://java.sun.com/products/autodl/j2se"/>
            <jar href="DynamicTreeDemo.jar"
                main="true" />

        </resources>
        <application-desc
             name="Dynamic Tree Demo Application"
             main-class=
               "webstartComponentArch.DynamicTreeApplication"
             width="300"
             height="300">
         </application-desc>
         <update check="background"/>
    </jnlp>                                   

    ```

    JNLP 文件的结构描述了 JNLP 文件的语法和选项。

    * * *

    **注意：** 在部署将在至少 Java SE 6 更新 18 版本或更高版本上运行的 Java Web Start 应用程序时，`codebase` 和 `href` 属性是可选的。在部署将在早期版本的 Java Runtime Environment 软件上运行的 Java Web Start 应用程序时，必须指定 `codebase` 和 `href` 属性。

    * * *

1.  创建 HTML 页面，从中启动您的应用程序。调用部署工具包函数来部署 Java Web Start 应用程序。

    在示例中，动态树演示应用程序部署在``JavaWebStartAppPage.html``中。

    ```java
    <body>
        <!-- ... -->
        <script src=
          "https://www.java.com/js/deployJava.js"></script>
        <script>
            // using JavaScript to get location of JNLP
            // file relative to HTML page
            var dir = location.href.substring(0,
                location.href.lastIndexOf('/')+1);
            var url = dir + "dynamictree_webstart.jnlp";
            deployJava.createWebStartLaunchButton(url, '1.7.0');
        </script>
        <!-- ... -->
    </body>

    ```

    如果您不确定最终用户的浏览器中是否启用了 JavaScript 解释器，您可以通过直接创建指向 JNLP 文件的链接来部署 Java Web Start 应用程序，如下所示：

    ```java
    <a href="/*absolute path to JNLP file*/dynamictree_webstart.jnlp">Launch Notepad Application</a>

    ```

    如果您通过直接链接部署 Java Web Start 应用程序，则无法利用部署工具包函数提供的额外检查。有关详细信息，请参阅部署 Java Web Start 应用程序中的深入部署课程。

1.  将应用程序的 JAR 文件、JNLP 文件和 HTML 页面放在适当的文件夹中。

    对于这个示例，将`DynamicTreeDemo.jar`、`dynamictree_webstart.jnlp`和`JavaWebStartAppPage.html`放在本地计算机或 Web 服务器上的同一目录中。最好使用 Web 服务器。要从本地计算机运行，必须将应用程序添加到例外站点列表中，该列表可从 Java 控制面板的安全选项卡中管理。

1.  在浏览器中打开应用程序的 HTML 页面以查看应用程序。在提示时同意运行应用程序。检查 Java 控制台日志以查看错误和调试消息。

下载源代码以进一步尝试*动态树演示*示例。
