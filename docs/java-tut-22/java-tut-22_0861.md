# 编译并运行程序

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/step5.html`](https://docs.oracle.com/javase/tutorial/security/apisign/step5.html)

`这里`是`GenSig.java`程序的完整源代码，添加了一些注释。编译并运行它。请记住，您需要指定要签名的文件名，如

```java
java GenSig data

```

您可以下载并使用名为`data`的示例文件或您喜欢的任何其他文件。该文件不会被修改。它将被读取，以便为其生成签名。

执行程序后，您应该看到保存的`suepk`（公钥）和`sig`（签名）文件。
