# 输入签名字节

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/vstep3.html`](https://docs.oracle.com/javase/tutorial/security/apisign/vstep3.html)

下一步，输入作为第二个命令行参数指定的文件中的签名字节。

```java
FileInputStream sigfis = new FileInputStream(args[1]);
byte[] sigToVerify = new byte[sigfis.available()]; 
sigfis.read(sigToVerify);
sigfis.close();

```

现在字节数组`sigToVerify`包含了签名字节。
