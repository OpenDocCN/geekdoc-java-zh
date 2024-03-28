# 生成公钥和私钥

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/step2.html`](https://docs.oracle.com/javase/tutorial/security/apisign/step2.html)

要能够创建数字签名，您需要一个私钥。（为了验证签名的真实性，还需要相应的公钥。）

在某些情况下，*密钥对*（私钥和相应的公钥）已经存在于文件中。在这种情况下，程序可以导入并使用私钥进行签名，如 Weaknesses and Alternatives 中所示。

在其他情况下，程序需要生成密钥对。通过使用`KeyPairGenerator`类生成密钥对。

在此示例中，您将为数字签名算法（DSA）生成公钥/私钥对。您将生成长度为 1024 位的密钥。

生成密钥对需要几个步骤：

创建密钥对生成器

第一步是获取用于生成 DSA 签名算法密钥的密钥对生成器对象。

与所有引擎类一样，获取特定类型算法的`KeyPairGenerator`对象的方法是在`KeyPairGenerator`类上调用`getInstance`静态工厂方法。该方法有两种形式，都有一个`String algorithm`作为第一个参数；其中一种形式还有一个`String provider`作为第二个参数。

调用者可以选择指定提供程序的名称，这将确保所请求的算法实现来自指定的提供程序。本课程的示例代码始终指定内置于 JDK 中的默认 SUN 提供程序。

在上述声明之后放置

```java
else try {

```

在上一步创建的文件中的行，Prepare Initial Program Structure：

```java
KeyPairGenerator keyGen = KeyPairGenerator.getInstance("DSA", "SUN");

```

初始化密钥对生成器

下一步是初始化密钥对生成器。所有密钥对生成器都共享密钥大小和随机源的概念。`KeyPairGenerator`类有一个`initialize`方法，接受这两种类型的参数。

DSA 密钥生成器的密钥大小是密钥长度（以位为单位），您将设置为 1024。

随机源必须是`SecureRandom`类的实例，提供一个密码学强随机数生成器（RNG）。有关`SecureRandom`的更多信息，请参阅[SecureRandom API Specification](https://docs.oracle.com/javase/8/docs/api/java/security/SecureRandom.html)和[Java Cryptography Architecture Reference Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/security/crypto/CryptoSpec.html#SecureRandom)。

以下示例请求一个使用内置 SUN 提供程序提供的 SHA1PRNG 算法的`SecureRandom`实例。然后将此`SecureRandom`实例传递给密钥对生成器初始化方法。

```java
SecureRandom random = SecureRandom.getInstance("SHA1PRNG", "SUN");
keyGen.initialize(1024, random);

```

有些情况需要强随机值，比如创建高价值和长期存在的秘密，如 RSA 公钥和私钥。为了帮助应用程序选择合适的强`SecureRandom`实现，从 JDK 8 开始，Java 发行版在`java.security.Security`类的`securerandom.strongAlgorithms`属性中包含了已知的强`SecureRandom`实现列表。当您创建这样的数据时，应考虑使用`SecureRandom.getInstanceStrong()`，因为它获取已知强算法的实例。

生成密钥对

最后一步是生成密钥对，并将密钥存储在`PrivateKey`和`PublicKey`对象中。

```java
KeyPair pair = keyGen.generateKeyPair();
PrivateKey priv = pair.getPrivate();
PublicKey pub = pair.getPublic();

```
