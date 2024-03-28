# 验证签名

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/vstep4.html`](https://docs.oracle.com/javase/tutorial/security/apisign/vstep4.html)

您已经向`VerSig`程序添加了代码

+   输入编码的密钥字节并将其转换为名为`pubKey`的`PublicKey`

+   将签名字节输入到名为`sigToVerify`的字节数组中

您现在可以继续进行验证。

初始化用于验证的 Signature 对象

与生成签名一样，签名是通过使用`Signature`类的实例来验证的。您需要创建一个使用与生成签名相同的签名算法的`Signature`对象。`GenSig`程序使用的算法是来自 SUN 提供程序的 SHA1withDSA 算法。

```java
Signature sig = Signature.getInstance("SHA1withDSA", "SUN");

```

接下来，您需要初始化`Signature`对象。验证的初始化方法需要公钥。

```java
sig.initVerify(pubKey);

```

**向签名对象提供要验证的数据** 现在，您需要向`Signature`对象提供生成签名的数据。这些数据位于以第三个命令行参数指定的文件中。与签名时一样，逐个缓冲区读取数据，并通过调用`update`方法将其提供给`Signature`对象。

```java
FileInputStream datafis = new FileInputStream(args[2]);
BufferedInputStream bufin = new BufferedInputStream(datafis);

byte[] buffer = new byte[1024];
int len;
while (bufin.available() != 0) {
    len = bufin.read(buffer);
    sig.update(buffer, 0, len);
};

bufin.close();

```

验证签名

一旦您向`Signature`对象提供了所有数据，您可以验证该数据的数字签名并报告结果。请记住，所谓的签名已读入名为`sigToVerify`的字节数组。

```java
boolean verifies = sig.verify(sigToVerify);

System.out.println("signature verifies: " + verifies);

```

如果所谓的签名（`sigToVerify`）是由与公钥`pubKey`对应的私钥生成的指定数据文件的实际签名，则`verifies`值将为`true`。
