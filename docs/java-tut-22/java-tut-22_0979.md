# Java 应用程序设置

> 原文：[`docs.oracle.com/javase/tutorial/jndi/software/package.html`](https://docs.oracle.com/javase/tutorial/jndi/software/package.html)

要在程序中使用 JNDI，您需要设置其编译和执行环境。

## 导入 JNDI 类

以下是 JNDI 包：

+   [`javax.naming`](https://docs.oracle.com/javase/8/docs/api/javax/naming/package-summary.html)

+   [`javax.naming.directory`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/package-summary.html)

+   [`javax.naming.event`](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/package-summary.html)

+   [`javax.naming.ldap`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/package-summary.html)

+   [`javax.naming.spi`](https://docs.oracle.com/javase/8/docs/api/javax/naming/spi/package-summary.html)

本教程中的示例使用来自前两个包的类和接口。您需要将这两个包导入到您的程序中，或者导入您使用的单个类和接口。以下两行导入了两个包`javax.naming`和`javax.naming.directory`中的所有类和接口。

```java
import javax.naming.*;
import javax.naming.directory.*;

```

## 编译环境

要编译使用 JNDI 的程序，您需要访问 JNDI 类。 Java SE 6 已经包含了 JNDI 类，因此如果您正在使用它，您无需采取进一步的操作。

## 执行环境

要运行使用 JNDI 的程序，您需要访问 JNDI 类和程序使用的任何服务提供商的类。 Java Runtime Environment（JRE）6 已经包含了 LDAP、COS 命名、RMI 注册表和 DNS 的 JNDI 类和服务提供商。

如果您正在使用其他服务提供商，则需要下载并安装它们的存档文件到*JAVA_HOME*`/jre/lib/ext`目录中，其中*JAVA_HOME*是包含 JRE 的目录。 [JNDI 页面](http://www.oracle.com/technetwork/java/jndi/index.html#download)列出了一些服务提供商。您可以下载这些提供商或使用其他供应商的提供商。
