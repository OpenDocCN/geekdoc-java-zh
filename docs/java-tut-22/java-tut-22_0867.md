# 编译并运行程序

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/vstep5.html`](https://docs.oracle.com/javase/tutorial/security/apisign/vstep5.html)

`这里`是`VerSig.java`程序的完整源代码，附加了一些注释。

编译并运行程序。请记住，您需要在命令行上指定三个参数：

+   包含编码的公钥字节的文件的名称

+   包含签名字节的文件的名称

+   数据文件的名称（生成签名的文件）

由于您将测试`GenSig`程序的输出，您应该使用的文件名是

+   `suepk`

+   `sig`

+   `data`

这是一个示例运行；**粗体**表示您需要键入的内容。

```java
%java VerSig suepk sig data
signature verifies: true

```
