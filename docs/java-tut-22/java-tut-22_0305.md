# 部署小程序

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/runAppletFunction.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/runAppletFunction.html)

你可以通过使用[部署工具包](https://www.java.com/js/deployJava.txt)脚本的`runApplet`函数来部署小程序。`runApplet`函数确保客户端上存在所需的最低版本的 Java 运行环境（JRE）软件，然后运行小程序。`runApplet`函数生成一个带有提供信息的 HTML `<applet>`标签。

* * *

**注意：** 根据浏览器类型，当尝试查看网页源代码时，可能无法查看部署工具包脚本生成的 HTML。要查看生成的 HTML，请尝试在加载后保存 HTML 页面，或使用类似 Firebug（Mozilla Firefox 附加组件）的工具。

* * *

你可以通过指定部署选项作为`<applet>`标签的属性和参数来部署小程序。你也可以在 Java 网络启动协议（JNLP）文件中指定部署选项以利用高级功能。有关此协议的更多信息，请参阅 Java 网络启动协议主题。

* * *

**注意：**

如果客户端没有所需的 JRE 软件的最低版本，部署工具包脚本会将浏览器重定向到`http://www.java.com`，以允许用户下载最新的 JRE 软件。在某些平台上，用户可能在查看包含小程序的网页之前被重定向。

* * *

`runApplet`函数的参数取决于是否使用 JNLP。通过 JNLP 部署的小程序只能在客户端机器上存在下一代 Java 插件软件时运行（下一代 Java 插件软件是在 Java 平台标准版 6 更新 10 发布中引入的）。

下一节将展示如何在将显示小程序的 HTML 页面中使用`runApplet`函数。描述了以下使用场景：

+   作为属性和参数名称值对指定部署选项

+   使用`jnlp_href`参数在 JNLP 文件中指定部署选项

+   指定属性和参数名称值对以及 JNLP 文件（使小程序能够在旧版和下一代 Java 插件软件上运行）

**函数签名：** `runApplet: function(attributes, parameters, minimumVersion)`

参数：

+   `attributes` – 生成的`<applet>`标签的属性的名称和值

+   `parameters` – 生成的`<applet>`标签中`<param>`标签的名称和值

+   `minimumVersion` – 运行此小程序所需的 JRE 软件的最低版本

用法：

+   作为属性和参数名称值对指定部署选项

    作为属性名称值对传递的属性和参数将被写入生成的`<applet>`标签中的属性和嵌套的`<param>`标签中。以这种方式部署的小程序可以通过旧版 Java 插件软件运行。

    ```java
    // launch the Java 2D applet on JRE version 1.6.0
    // or higher with one parameter (fontSize)
    <script src=
        "https://www.java.com/js/deployJava.js"></script>
    <script>
        var attributes = {code:'java2d.Java2DemoApplet.class',
            archive:'Java2Demo.jar', width:710, height:540};
        var parameters = { fontSize:16, permissions:'sandbox' };
        var version = '1.6';
        deployJava.runApplet(attributes, parameters, version);
    </script>

    ```

    在浏览器中打开``DeployUsingNameValuePairs.html``以查看 Java2D 小程序。

    * * *

    **注意：** 如果你看不到小程序在运行，你需要至少安装[Java SE 开发工具包（JDK）7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

    * * *

+   使用`jnlp_href`参数在 JNLP 文件中指定部署选项

    作为属性名称值对传递的属性和参数（在这种情况下是`jnlp_href`）将被写入生成的`<applet>`标签中的属性和嵌套的`<param>`标签中。以这种方式部署的小程序只能通过下一代 Java 插件软件运行。最好将小程序的宽度和高度指定为属性，如下所示：

    ```java
    <script src="https://www.java.com/js/deployJava.js"></script>
    <script> 
        var attributes = { code:'java2d.Java2DemoApplet', width:710, height:540 }; 
        var parameters = { jnlp_href: 'java2d.jnlp' }; 
        deployJava.runApplet(attributes, parameters, '1.6'); 
    </script>

    ```

    在浏览器中打开``DeployUsingJNLP.html``以查看 Java2D 小程序。

    * * *

    **注意：** 如果你看不到小程序在运行，你需要至少安装[Java SE 开发工具包（JDK）6 更新 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

    * * *

+   指定属性和参数名称值对以及 JNLP 文件

    通过使用 JNLP 部署的小程序只能在最终用户的浏览器上运行下一代 Java 插件软件。如果你希望你的小程序也在旧版 Java 插件软件上运行，请使用属性和参数名称值对以及 JNLP 文件指定部署选项。

    ```java

    <script src="https://www.java.com/js/deployJava.js"></script>
    <script>  
        var attributes = {code:'java2d.Java2DemoApplet.class', 
                archive:'Java2Demo.jar', width:710, height:540}; 
        var parameters = { fontSize:16, jnlp_href:'java2d.jnlp' }; 
        var version = '1.6' ; 
        deployJava.runApplet(attributes, parameters, version);      
    </script>

    ```

如果一些部署选项在属性名称值对和 JNLP 文件中有不同的值，以下准则会很有帮助：

+   将`width`和`height`指定为属性名称值对（不在 JNLP 文件中）。

+   将参数如`image`和`boxbgcolor`指定为参数名称值对（不在 JNLP 文件中）。这些参数在小程序启动过程中需要提前。

+   在 JNLP 文件中，将`codebase`属性留空或指定绝对 URL。当`codebase`属性留空时，默认为包含 JNLP 文件的目录。

+   如果小程序是使用 JNLP 文件启动的，则`code`、`codebase`和`archive`属性的值将从 JNLP 文件中获取。如果这些属性也分别作为属性名称值对指定，那么属性名称值对将被忽略。

在浏览器中打开``DeployUsingNameValuePairsAndJNLP.html``以查看 Java2D 小程序。

* * *

**注意：** 如果你看不到小程序在运行，你需要至少安装[Java SE 开发工具包（JDK）7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

下载源代码以进一步进行实验。
