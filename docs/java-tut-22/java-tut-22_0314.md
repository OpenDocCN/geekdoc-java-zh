# 减少下载时间

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/reducingDownloadTime.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/reducingDownloadTime.html)

当用户尝试访问时，从网站下载富互联网应用程序（RIA）。 （初始下载后，可以缓存 RIA 以提高性能）。 下载 RIA 所需的时间取决于 RIA 的 JAR 文件大小。 更大的 JAR 文件下载时间更长。

通过应用以下技术，您可以减少 RIA 的下载时间：

+   使用[`pack200`](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/pack200.html)工具压缩 RIA 的 JAR 文件。

+   从 Java 网络启动协议（JNLP）文件和 JavaScript 文件中删除不必要的空白。

+   优化图像和动画。

以下步骤描述了如何为已签名的 RIA 创建和部署压缩的 JAR 文件。

1.  使用`--repack`选项对 JAR 文件进行规范化。

    此步骤确保在启动 RIA 时安全证书和 JAR 文件将通过验证检查。

    ```java
    pack200 --repack DynamicTreeDemo.jar

    ```

1.  对规范化的 JAR 文件进行签名。

    ```java
    jarsigner -keystore myKeyStore DynamicTreeDemo.jar me

    ```

    其中`myKeyStore`是密钥库的名称，`me`是密钥库的别名。

1.  打包已签名的 JAR 文件

    ```java
    pack200 DynamicTreeDemo.jar.pack.gz DynamicTreeDemo.jar    

    ```

1.  在 RIA 的 JNLP 文件中将`jnlp.packEnabled`属性设置为`true`。

    ```java
    <resources>    
        <j2se version="1.6+"
            href="http://java.sun.com/products/autodl/j2se"
                  max-heap-size="128m" />
        <jar href="DynamicTreeDemo.jar"
            main="true"/>
        <property name="jnlp.packEnabled"
            value="true"/>
        <!-- ... -->
    </resources>

    ```

当在 JNLP 文件中设置`jnlp.packEnabled`属性时，Java 插件软件会查找具有`.pack.gz`扩展名的压缩 JAR 文件（例如，`DynamicTreeDemo.jar.pack.gz`）。 如果找到，则 Java 插件软件会自动解压缩和加载 JAR 文件。 如果找不到具有`.pack.gz`扩展名的文件，则 Java 插件软件会尝试加载常规 JAR 文件（例如，`DynamicTreeDemo.jar`）。

* * *

**注意：** 您需要将 RIA 部署在 Web 服务器上以测试`jnlp.packEnabled`属性。

* * *
