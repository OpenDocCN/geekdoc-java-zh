# 检查客户端 JRE 软件版本

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/jreVersionCheck.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/jreVersionCheck.html)

有许多原因要检查客户端计算机上是否安装了特定版本的 Java 运行时环境（JRE）软件。例如，您可能希望根据客户端的 JRE 软件版本启动不同版本的富互联网应用程序（RIA），或者根据客户端的 JRE 软件版本将用户重定向到不同的页面。

使用部署工具包脚本的`versionCheck`函数来检查客户端是否安装了特定版本或一系列 JRE 版本。

**函数签名：** `versionCheck: function(versionPattern)`

参数：

+   `versionPattern` – 字符串，指定要检查的版本或版本范围，例如"1.4"，"1.5.0*"（1.5.x 系列），以及"1.6.0_02+"（大于或等于 1.6.0_02 的任何版本）。

**用法：** 根据客户端的 JRE 软件版本创建不同的用户体验

在这个例子中，仅当客户端上的 JRE 软件版本大于或等于 1.6 时，才会为记事本应用程序创建一个启动按钮。如果不是，则浏览器将重定向到`oracle.com`。

```java

<script src="https://www.java.com/js/deployJava.js"></script>
<script>
    if (deployJava.versionCheck('1.6+')) {            
        var url = "https://docs.oracle.com/javase/tutorialJWS/deployment/webstart/examples/Notepad.jnlp";

        <!-- you can also invoke deployJava.runApplet here -->
        deployJava.createWebStartLaunchButton(url, '1.6.0'); 
    } else {
        document.location.href="http://oracle.com";
    }
</script>         

```

* * *

**注意：** 根据客户端操作系统和 Java 平台的版本，您可能可以在 JRE 软件的主要版本级别（例如 1.6）或更精细的更新级别（例如 1.6.0_10）上验证版本信息。

* * *
