# 添加外部库

> 原文：[`docs.oracle.com/javase/tutorial/deployment/selfContainedApps/addlibrary.html`](https://docs.oracle.com/javase/tutorial/deployment/selfContainedApps/addlibrary.html)

自包含应用程序包含应用程序运行所需的一切。 如果您的应用程序需要外部库，则可以将该库添加到应用程序的包中。 添加库可以通过不同的方式完成。

使用文件关联中描述的文件关联演示在构建过程中下载 Groovy 库。 该库被放置在项目的`/lib`目录中供应用程序使用。 然后，该目录被复制到生成自包含应用程序包的`/dist`目录中。

`build.xml`文件中`-pre-init`任务中的以下代码显示了如何下载库：

```java
<!-- download and copy groovy library -->
<copy toFile="lib/groovy-all-2.3.8.jar">
    <resources>
      <url url="http://central.maven.org/maven2/org/codehaus/groovy/groovy-all/2.3.8/groovy-all-2.3.8.jar"/>
    </resources>
</copy>

```

查看`build.xml`以获取完整的构建代码。

你可以从自包含应用示例下载文件关联演示的源文件。
