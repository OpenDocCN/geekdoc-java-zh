# 对数据进行签名

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/step3.html`](https://docs.oracle.com/javase/tutorial/security/apisign/step3.html)

现在您已经创建了公钥和私钥，可以准备对数据进行签名。在此示例中，您将对文件中包含的数据进行签名。`GenSig` 从命令行获取文件名。使用 `Signature` 类的实例创建数字签名。

对数据进行签名，生成该数据的数字签名，需要执行以下步骤。

**获取签名对象**：以下获取一个 `Signature` 对象，用于使用 DSA 算法生成或验证签名，该算法与程序在上一步中生成密钥的算法相同，生成公钥和私钥。

```java
Signature dsa = Signature.getInstance("SHA1withDSA", "SUN"); 

```

注意：在指定签名算法名称时，还应包括签名算法使用的消息摘要算法的名称。SHA1withDSA 是指定 DSA 签名算法的一种方式，使用 SHA-1 消息摘要算法。

初始化签名对象

在 `Signature` 对象用于签名或验证之前，必须进行初始化。签名的初始化方法需要一个私钥。使用前一步中放入名为 `priv` 的 `PrivateKey` 对象中的私钥。

```java
dsa.initSign(priv);

```

**向签名对象提供要签名的数据** 该程序将使用作为第一个（也是唯一的）命令行参数指定的文件名中的数据。程序将逐个缓冲区读取数据，并通过调用 `update` 方法将其提供给 `Signature` 对象。

```java
FileInputStream fis = new FileInputStream(args[0]);
BufferedInputStream bufin = new BufferedInputStream(fis);
byte[] buffer = new byte[1024];
int len;
while ((len = bufin.read(buffer)) >= 0) {
    dsa.update(buffer, 0, len);
};
bufin.close();

```

生成签名

一旦所有数据都已提供给 `Signature` 对象，就可以生成该数据的数字签名。

```java
byte[] realSig = dsa.sign();

```
