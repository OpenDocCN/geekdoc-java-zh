# 弱点和替代方案

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/enhancements.html`](https://docs.oracle.com/javase/tutorial/security/apisign/enhancements.html)

本课程中的`GenSig`和`VerSig`程序演示了使用 JDK 安全 API 生成数据的数字签名以及验证签名的用法。然而，这些程序描绘的实际场景，即发送方使用 JDK 安全 API 生成新的公钥/私钥对，发送方将编码的公钥字节存储在文件中，接收方读取密钥字节，这并不一定是现实的，并且存在一个潜在的重大缺陷。

在许多情况下，密钥不需要生成；它们已经存在，要么作为文件中的编码密钥，要么作为密钥库中的条目。

潜在的重大缺陷在于没有任何保证接收方收到的公钥的真实性，而`VerSig`程序只有在提供的公钥本身是*真实*的情况下才能正确验证签名的真实性！

## 使用编码的密钥字节

有时，编码的密钥字节已经存在于用于签名和验证的密钥对的文件中。如果是这种情况，`GenSig`程序可以导入编码的私钥字节，并将其转换为签名所需的`PrivateKey`，通过以下方式，假设包含私钥字节的文件名在`privkeyfile`字符串中，并且字节代表已使用 PKCS #8 标准编码的 DSA 密钥。

```java
FileInputStream keyfis = new FileInputStream(privkeyfile);
byte[] encKey = new byte[keyfis.available()];
keyfis.read(encKey);
keyfis.close();

PKCS8EncodedKeySpec privKeySpec = new PKCS8EncodedKeySpec(encKey);

KeyFactory keyFactory = KeyFactory.getInstance("DSA");
PrivateKey privKey = keyFactory.generatePrivate(privKeySpec);

```

`GenSig`不再需要将公钥字节保存在文件中，因为它们已经在一个文件中。

在这种情况下，发送方发送接收方

+   包含编码的公钥字节的已存在文件（除非接收方已经拥有此文件）和

+   由`GenSig`导出的数据文件和签名文件。

`VerSig`程序保持不变，因为它已经期望文件中存在编码的公钥字节。

但是，如果有恶意用户拦截文件并以一种无法检测到其更换的方式替换它们，会出现什么潜在问题呢？在某些情况下，这不是问题，因为人们已经通过面对面或通过信任的第三方进行了公钥交换。之后，可以远程进行多次文件和签名交换（即在不同位置的两个人之间），并且可以使用公钥来验证其真实性。如果有恶意用户尝试更改数据或签名，这将被`VerSig`检测到。

如果无法进行面对面的密钥交换，您可以尝试其他方法来增加正确接收的可能性。例如，您可以在随后的数据和签名文件交换之前，通过尽可能安全的方法发送您的公钥，也许使用不太安全的媒介。

一般来说，将数据和签名与你的公钥分开发送大大降低了攻击的可能性。除非所有三个文件都被更改，并且以下一段讨论的特定方式，否则`VerSig`将检测到任何篡改。

如果所有三个文件（数据文档、公钥和签名）被恶意用户拦截，那个人可以用私钥替换文档并签名，然后将替换后的文档、新签名和用于生成新签名的私钥对应的公钥转发给你。然后`VerSig`会报告验证成功，你会认为文档来自原始发送者。因此，你应该采取措施确保至少公钥完整接收（`VerSig`检测到其他文件的任何篡改），或者可以使用证书来促进公钥的认证，如下一节所述。

## 使用证书

在密码学中，更常见的是交换包含公钥的*证书*，而不是公钥本身。

一个好处是，证书由一个实体（*颁发者*）签名，以验证所包含的公钥是另一个实体（*主体*或*所有者*）的实际公钥。通常，一个受信任的第三方*认证机构*（CA）验证主体的身份，然后通过签署证书来担保其为公钥所有者。

使用证书的另一个好处是，你可以通过使用颁发者（签名者）的公钥验证其数字签名来检查你收到的证书的有效性，该公钥本身可能存储在一个证书中，其签名可以通过使用该证书颁发者的公钥验证；该公钥本身可能存储在一个证书中，依此类推，直到达到你已经信任的公钥。

如果你无法建立信任链（也许因为所需的颁发者证书对你不可用），可以计算证书的**指纹**。每个指纹是一个相对较短的数字，可以唯一可靠地识别证书。（从技术上讲，它是证书信息的哈希值，使用消息摘要，也称为单向哈希函数。）你可以联系证书所有者，比较你收到的证书的指纹与发送的指纹。如果它们相同，证书也相同。

对于`GenSig`来说，更安全的做法是创建包含公钥的证书，然后让`VerSig`导入证书并提取公钥。然而，JDK 没有公共证书 API，允许你从公钥创建证书，因此`GenSig`程序无法从生成的公钥创建证书。（尽管有从证书中提取公钥的公共 API。）

如果您愿意，您可以使用各种安全工具，而不是 API，对您的重要文档进行签名，并与密钥库中的证书一起使用，就像在文件交换课程中所做的那样。

或者，您可以使用 API 修改您的程序以使用来自密钥库的已存在私钥和相应的公钥（在证书中）。首先，修改`GenSig`程序以从密钥库中提取私钥而不是生成新密钥。首先，让我们假设以下内容：

+   密钥库名称在`String``ksName`中

+   密钥库类型为"JKS"，这是来自 Oracle 的专有类型。

+   密钥库密码在字符数组`spass`中

+   包含私钥和公钥证书的密钥库条目的别名在`String``alias`中

+   私钥密码在字符数组`kpass`中

然后，您可以通过以下方式从密钥库中提取私钥。

```java
KeyStore ks = KeyStore.getInstance("JKS");
FileInputStream ksfis = new FileInputStream(ksName); 
BufferedInputStream ksbufin = new BufferedInputStream(ksfis);

ks.load(ksbufin, spass);
PrivateKey priv = (PrivateKey) ks.getKey(alias, kpass);

```

您可以从密钥库中提取公钥证书，并将其编码字节保存到名为`suecert`的文件中，通过以下方式。

```java
java.security.cert.Certificate cert = ks.getCertificate(alias);
byte[] encodedCert = cert.getEncoded();

// Save the certificate in a file named "suecert" 

FileOutputStream certfos = new FileOutputStream("suecert");
certfos.write(encodedCert);
certfos.close();

```

然后，您将数据文件、签名和证书发送给接收者。接收者通过首先使用`keytool -printcert`命令获取证书的指纹来验证证书的真实性。

```java
keytool -printcert -file suecert
Owner: CN=Susan Jones, OU=Purchasing, O=ABC, L=Cupertino, ST=CA, C=US
Issuer: CN=Susan Jones, OU=Purchasing, O=ABC, L=Cupertino, ST=CA, C=US
Serial number: 35aaed17
Valid from: Mon Jul 13 22:31:03 PDT 1998 until:
Sun Oct 11 22:31:03 PDT 1998
Certificate fingerprints:
MD5:  1E:B8:04:59:86:7A:78:6B:40:AC:64:89:2C:0F:DD:13
SHA1: 1C:79:BD:26:A1:34:C0:0A:30:63:11:6A:F2:B9:67:DF:E5:8D:7B:5E

```

然后接收者验证指纹，可能通过给发送者打电话并将其与发送者的证书进行比较，或者通过在公共存储库中查找它们来进行验证。

接收者的验证程序（修改后的`VerSig`）然后可以通过以下方式导入证书并从中提取公钥，假设证书文件名（例如，`suecert`）在`String``certName`中。

```java
FileInputStream certfis = new FileInputStream(certName);
java.security.cert.CertificateFactory cf =
    java.security.cert.CertificateFactory.getInstance("X.509");
java.security.cert.Certificate cert =  cf.generateCertificate(certfis);
PublicKey pub = cert.getPublicKey();

```

## 确保数据机密性

假设您希望保持数据的内容机密性，以便在传输过程中（或在您自己的计算机或磁盘上）无意或恶意尝试查看数据的人无法这样做。为了保持数据的机密性，您应该对其进行加密，仅存储和发送加密结果（称为*ciphertext*）。接收者可以解密密文以获得原始数据的副本。
