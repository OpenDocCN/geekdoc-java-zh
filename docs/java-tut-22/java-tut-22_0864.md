# 输入并转换编码的公钥字节

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/vstep2.html`](https://docs.oracle.com/javase/tutorial/security/apisign/vstep2.html)

接下来，`VerSig`需要从指定为第一个命令行参数的文件中导入编码的公钥字节，并将其转换为`PublicKey`。需要一个`PublicKey`，因为`Signature`的`initVerify`方法需要它来初始化用于验证的`Signature`对象。

首先，读取编码的公钥字节。

```java
FileInputStream keyfis = new FileInputStream(args[0]);
byte[] encKey = new byte[keyfis.available()];  
keyfis.read(encKey);

keyfis.close();

```

现在字节数组`encKey`包含了编码的公钥字节。

你可以使用`KeyFactory`类来实例化一个 DSA 公钥，从其编码中。`KeyFactory`类提供了不透明密钥（类型为`Key`）和密钥规范之间的转换，密钥规范是底层密钥材料的透明表示。通过不透明密钥，你可以获取算法名称、格式名称和编码的密钥字节，但不能获取密钥材料，例如，可能包括密钥本身和用于计算密钥的算法参数。 （请注意，`PublicKey`，因为它扩展了`Key`，本身也是一个`Key`。）

所以，首先你需要一个密钥规范。假设密钥是根据 X.509 标准编码的，你可以通过以下方式获取一个，例如，如果密钥是使用 SUN 提供的内置 DSA 密钥对生成器生成的：

```java
X509EncodedKeySpec pubKeySpec = new X509EncodedKeySpec(encKey);

```

现在你需要一个`KeyFactory`对象来进行转换。该对象必须是一个可以处理 DSA 密钥的对象。

```java
KeyFactory keyFactory = KeyFactory.getInstance("DSA", "SUN");

```

最后，你可以使用`KeyFactory`对象从密钥规范生成一个`PublicKey`。

```java
PublicKey pubKey =
    keyFactory.generatePublic(pubKeySpec);

```
