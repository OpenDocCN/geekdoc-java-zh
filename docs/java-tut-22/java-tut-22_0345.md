# 验证已签名的 JAR 文件

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/verify.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/verify.html)

通常，验证已签名的 JAR 文件将由您的 Java™运行环境负责。您的浏览器将验证下载的已签名小程序。使用解释器的`-jar`选项调用的已签名应用程序将由运行环境验证。

然而，您可以使用`jarsigner`工具自行验证已签名的 JAR 文件。例如，您可能想要这样做来测试您准备的已签名 JAR 文件。

用于验证已签名的 JAR 文件的基本命令是：

```java
jarsigner -verify *jar-file*

```

这个命令将验证 JAR 文件的签名，并确保存档中的文件在签名后没有发生变化。如果验证成功，您将看到以下消息：

```java
jar verified.

```

如果您尝试验证一个未签名的 JAR 文件，将会出现以下消息：

```java
jar is unsigned. (signatures missing or not parsable)

```

如果验证失败，将显示相应的消息。例如，如果 JAR 文件的内容自签名以来发生了变化，当您尝试验证文件时，将会出现类似以下消息：

```java
jarsigner: java.lang.SecurityException: invalid SHA1 
signature file digest for test/classes/Manifest.class

```

* * *

**注意：** 如果已签名的 JAR 文件使用了`jdk.jar.disabledAlgorithms`安全属性中指定的任何算法（其中`*java.home*`是您安装 JRE 的目录），JDK 将把已签名的 JAR 文件视为未签名。

* * *
