# 确保 JRE 软件的存在

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/ensuringJRE.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/ensuringJRE.html)

富互联网应用程序（RIA）通常需要客户端计算机上存在 Java 运行时环境（JRE）软件的最低版本。在部署 RIA 时，您需要确保客户端计算机安装了所需版本的 JRE 软件，以便您的 RIA 能够正常运行。使用部署工具包脚本，您至少有两种处理此要求的方式。

+   您可以在用户访问您的网站时立即检查客户端 JRE 软件的版本，并在必要时安装最新版本。

+   您可以让用户浏览网站，并在他们尝试使用您的 RIA 时检查并安装最新的 JRE。

## 当用户访问您的网站时检查并安装最新的 JRE 软件

以下示例检查用户是否安装了至少版本为 1.6.0_13 的 JRE 软件。如果没有，则代码会安装最新的 JRE 软件。请参见代码中的内联注释。

```java
<script src="https://www.java.com/js/deployJava.js"></script>
<script>

    // check if current JRE version is greater than 1.6.0 
    alert("versioncheck " + deployJava.versionCheck('1.6.0_10+'));
    if (deployJava.versionCheck('1.6.0_10+') == false) {                   
        userInput = confirm(
            "You need the latest Java(TM) Runtime Environment. " +
            "Would you like to update now?");        
        if (userInput == true) {  

            // Set deployJava.returnPage to make sure user comes back to 
            // your web site after installing the JRE
            deployJava.returnPage = location.href;

            // Install latest JRE or redirect user to another page to get JRE
            deployJava.installLatestJRE(); 
        }
    }
</script>

```

## 仅在用户尝试使用您的 RIA 时安装正确的 JRE 软件

当您在 `runApplet` 或 `createWebStartLaunchButton` 函数中指定 JRE 软件的最低版本时，部署工具包脚本会确保客户端上存在所需版本的 JRE 软件才能运行您的 RIA。

使用 `runApplet` 函数部署小程序，如下例所示。`runApplet` 函数的最后一个参数是运行您的小程序所需的最低版本（版本 1.6）。

```java

<script src="https://www.java.com/js/deployJava.js"></script>
<script>
    var attributes = { code:'components.DynamicTreeApplet',
        width:300, height:300};
    var parameters = {jnlp_href: 'dynamictree_applet.jnlp'};
    deployJava.runApplet(attributes, parameters, '1.6');
</script>

```

要部署 Java Web Start 应用程序，请使用 `createWebStartLaunchButton` 函数并使用正确的最低版本参数（版本 1.6）。

```java
<script src="https://www.java.com/js/deployJava.js"></script>
<script>
    var url = "dynamictree_applet.jnlp";
    deployJava.createWebStartLaunchButton(url, '1.6.0');
</script>

```

`runApplet` 和 `createWebStartLaunchButton` 函数检查客户端的 JRE 软件版本。如果未安装最低版本，则函数会安装最新版本的 JRE 软件。
