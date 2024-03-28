# 避免不必要的更新检查

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/avoidingUnnecessaryUpdateChecks.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/avoidingUnnecessaryUpdateChecks.html)

富互联网应用程序（RIA）在本地缓存以提高启动时间。但是，在启动 RIA 之前，启动软件会检查确保 RIA 的 Java 网络启动协议（JNLP）文件中引用的每个 JAR 文件都是最新的。换句话说，启动软件确保您运行的是 RIA 的最新版本，而不是旧的缓存副本。这些更新检查可能需要几百毫秒，具体取决于 JAR 文件数量和网络速度。使用本主题中描述的技术来避免不必要的更新检查，并提高 RIA 的启动时间。

* * *

**注意：**

此处使用术语“启动软件”来统称 Java 插件软件和 Java Web Start 软件。Java 插件软件用于启动小程序，而 Java Web Start 软件用于启动 Java Web Start 应用程序。

* * *

## 利用版本下载协议

您可以利用*版本下载协议*来消除不必要的版本检查。请参阅以下步骤以启用此协议。

1.  将 JAR 文件重命名为包含版本号后缀的命名约定如下：

    ```java

        *<JAR file name>*__V*<version number>*.jar

    ```

    例如，将`DynamicTreeDemo.jar`重命名为`DynamicTreeDemo__V1.0.jar`。

1.  在 JNLP 文件中为每个 JAR 文件指定一个版本，并将`jnlp.versionEnabled`属性设置为`true`。

    ```java
    <resources>
        <!-- Application Resources -->
        <j2se version="1.6+"
            href="http://java.sun.com/products/autodl/j2se"
                max-heap-size="128m" />
        <jar href="DynamicTreeDemo.jar"
            main="true" version="1.0"/>   
        <jar href="SomeOther.jar" version="2.0"/>
        <property name="jnlp.versionEnabled"
            value="true"/>
        <!-- ... -->
    </resources>

    ```

    当启用`jnlp.versionEnabled`属性时，启动软件仅执行*一次*更新检查，以确保 JNLP 文件是最新的。软件将 JNLP 文件中指定的版本号与相应的 JAR 文件版本（根据第 1 步中提到的命名约定）进行比较，并仅更新过时的 JAR 文件。这种方法非常高效，因为仅在网络上进行 JNLP 文件的更新检查。所有其他版本检查都在本地进行。

    如果未找到具有正确版本号的文件，则启动软件将尝试加载默认的 JAR 文件（例如，`DynamicTreeDemo.jar`）。

## 在后台执行更新检查

如果用户立即运行您的 RIA 的最新版本并不是关键，您可以指定所有更新检查应在后台进行。在这种情况下，启动软件将启动本地缓存副本以供立即使用，并在后台下载 RIA 的新版本。下次用户尝试使用您的 RIA 时，将启动新版本的 RIA。要启用后台更新检查，请将以下行添加到您的 JNLP 文件中：

```java
<update check='background'/>

```

以下代码片段显示了启用后台更新检查的示例 JNLP 文件：

```java
<?xml version="1.0" encoding="UTF-8"?>
<jnlp spec="1.0+" codebase="" href="">
    <information>
        <title>Applet Takes Params</title>
        <vendor>Dynamic Team</vendor>
    </information>
    <resources>
        <!-- Application Resources -->
        <j2se version="1.6+" href=
            "http://java.sun.com/products/autodl/j2se"/>
        <jar href="applet_AppletWithParameters.jar"
            main="true" />
    </resources>
    <applet-desc 
         name="Applet Takes Params"
         main-class="AppletTakesParams"
         width="800"
         height="50">
             <param name="paramStr" value="someString"/>
             <param name="paramInt" value="22"/>
     </applet-desc>
     <update check="background"/>
</jnlp>

```
