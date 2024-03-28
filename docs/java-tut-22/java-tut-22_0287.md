# 设置 Web 服务器

> 原文：[`docs.oracle.com/javase/tutorial/deployment/webstart/settingUpWebServerMimeType.html`](https://docs.oracle.com/javase/tutorial/deployment/webstart/settingUpWebServerMimeType.html)

您可能需要配置您的 web 服务器来处理 Java 网络启动协议（JNLP）文件。如果 web 服务器没有正确设置，当您单击 JNLP 文件的链接时，Java Web 启动应用程序将无法启动。

配置 web 服务器，使具有`.jnlp`扩展名的文件设置为`application/x-java-jnlp-file` MIME 类型。

设置 JNLP MIME 类型的具体步骤将根据 web 服务器的不同而有所变化。例如，要配置 Apache web 服务器，您应该在`mime.types`文件中添加以下行。

```java
application/x-java-jnlp-file JNLP

```

对于其他 web 服务器，请查阅文档以获取设置 MIME 类型的说明。
