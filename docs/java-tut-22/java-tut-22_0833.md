# 创建一个包含类文件的 JAR 文件

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/step2.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/step2.html)

接下来，在命令窗口中输入以下内容，创建一个包含`Count.class`文件的 JAR 文件：

```java
jar cvf Count.jar Count.class

```

这将创建一个名为`Count.jar`的 JAR 文件，并将`Count.class`文件放入其中。
