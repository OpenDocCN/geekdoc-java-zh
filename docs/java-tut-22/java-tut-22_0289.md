# Java Web Start 和安全性

> 原文：[`docs.oracle.com/javase/tutorial/deployment/webstart/security.html`](https://docs.oracle.com/javase/tutorial/deployment/webstart/security.html)

本节描述了通过 Java Web Start 部署的应用程序的安全基础知识，包括：

使用 Java Web Start 启动的应用程序默认在被称为*沙盒*的受限环境中运行。在这个沙盒中，Java Web Start：

+   保护用户免受可能影响本地文件的恶意代码

+   保护企业免受可能试图访问或破坏网络数据的代码

通过 Java Web Start 启动的沙箱应用程序仍保持在此沙箱中，这意味着它们无法访问本地文件或网络。有关详细信息，请参阅富互联网应用程序中的安全性

## 动态下载 HTTPS 证书

Java Web Start 动态导入证书，就像浏览器通常做的那样。为此，Java Web Start 设置了自己的`https`处理程序，使用`java.protocol.handler.pkgs`系统属性来初始化[`SSLSocketFactory`](https://docs.oracle.com/javase/8/docs/api/javax/net/ssl/SSLSocketFactory.html)和[`HostnameVerifier`](https://docs.oracle.com/javase/8/docs/api/javax/net/ssl/HostnameVerifier.html)的默认值。它使用方法[`HttpsURLConnection.setDefaultSSLSocketFactory`](https://docs.oracle.com/javase/8/docs/api/javax/net/ssl/HttpsURLConnection.html#setDefaultSSLSocketFactory-javax.net.ssl.SSLSocketFactory-)和[`HttpsURLConnection.setDefaultHostnameVerifier`](https://docs.oracle.com/javase/8/docs/api/javax/net/ssl/HttpsURLConnection.html#setDefaultHostnameVerifier-javax.net.ssl.HostnameVerifier-)设置默认值。

如果您的应用程序使用这两种方法，请确保它们在 Java Web Start 初始化`https`处理程序之后被调用，否则您的自定义处理程序将被 Java Web Start 默认处理程序替换。

您可以确保使用自定义的`SSLSocketFactory`和`HostnameVerifier`之一：

+   安装您自己的`https`处理程序，以替换 Java Web Start 的`https`处理程序。

+   在您的应用程序中，只有在创建第一个`https URL`对象之后才调用`HttpsURLConnection.setDefaultSSLSocketFactory`或`HttpsURLConnection.setDefaultHostnameVerifier`，这会首先执行 Java Web Start 的`https`处理程序初始化代码。
