# 理解签署和验证

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/intro.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/intro.html)

Java™ 平台使你能够数字签名 JAR 文件。你数字签名一个文件的原因与你可能用钢笔在纸上签署文件的原因相同--让读者知道你写了这个文件，或者至少这个文件得到了你的批准。

当你签署一封信时，例如，所有认识你签名的人都可以确认你写了这封信。同样地，当你数字签名一个文件时，任何“认识”你数字签名的人都知道这个文件来自你。识别电子签名的过程称为*验证*。

当 JAR 文件被签名时，你还可以选择给签名加上时间戳。类似于在纸质文件上写日期，时间戳签名标识了 JAR 文件签名的时间。时间戳可用于验证签署 JAR 文件时使用的证书在签署时是有效的。

签署和验证文件的能力是 Java 平台安全架构的重要组成部分。安全性由运行时生效的安全*策略*控制。你可以配置策略以授予小程序和应用程序安全权限。例如，你可以授予小程序执行通常被禁止的操作的权限，如读写本地文件或运行本地可执行程序。如果你下载了由受信任实体签名的代码，你可以将这一事实作为决定为代码分配哪些安全权限的标准之一。

一旦你（或你的浏览器）验证了一个小程序来自受信任的来源，你可以让平台放宽安全限制，让小程序执行通常被禁止的操作。一个受信任的小程序可以根据运行时生效的*策略文件*拥有特定的自由。

Java 平台通过使用称为公钥和私钥的特殊数字来实现签署和验证。公钥和私钥是成对出现的，它们扮演互补的角色。

私钥是你可以用来签署文件的电子“笔”。顾名思义，你的私钥只有你知道，这样别人就无法“伪造”你的签名。用你的私钥签署的文件只能通过相应的公钥进行验证。

然而，仅有公钥和私钥还不足以真正验证一个签名。即使你已经验证了一个签名文件包含匹配的密钥对，你仍然需要一种方式来确认公钥确实来自它所声称来自的签署者。

因此，要使签名和验证工作，还需要另一个元素。这个额外的元素是签名者在签名的 JAR 文件中包含的*证书*。证书是来自被认可的*认证机构*的数字签名声明，指示谁拥有特定的公钥。认证机构是受到行业信任的实体（通常是专门从事数字安全的公司），它们负责为密钥及其所有者签署和发布证书。在签名的 JAR 文件中，证书指示谁拥有 JAR 文件中包含的公钥。

当您签署一个 JAR 文件时，您的公钥将与相关证书一起放置在存档中，以便任何想要验证您签名的人可以轻松使用。

总结数字签名：

+   签名者使用私钥对 JAR 文件进行签名。

+   相应的公钥与其证书一起放置在 JAR 文件中，以便任何想要验证签名的人可以使用。

## 摘要和签名文件

当您签署一个 JAR 文件时，存档中的每个文件都会在存档的清单中给出一个摘要条目。以下是这样一个条目可能看起来的示例：

```java
Name: test/classes/ClassOne.class
SHA1-Digest: TD1GZt8G11dXY2p4olSZPc5Rj64=

```

摘要值是文件内容的哈希值或编码表示，这些值是在签名时文件的内容。只有当文件本身发生变化时，文件的摘要值才会改变。

当对 JAR 文件进行签名时，会自动生成一个*签名*文件，并将其放置在 JAR 文件的`META-INF`目录中，该目录与存档的清单文件位于同一目录中。签名文件的文件名具有`.SF`扩展名。以下是签名文件内容的示例：

```java
Signature-Version: 1.0
SHA1-Digest-Manifest: h1yS+K9T7DyHtZrtI+LxvgqaMYM=
Created-By: 1.7.0_06 (Oracle Corporation)

Name: test/classes/ClassOne.class
SHA1-Digest: fcav7ShIG6i86xPepmitOVo4vWY=

Name: test/classes/ClassTwo.class
SHA1-Digest: xrQem9snnPhLySDiZyclMlsFdtM=

Name: test/images/ImageOne.gif
SHA1-Digest: kdHbE7kL9ZHLgK7akHttYV4XIa0=

Name: test/images/ImageTwo.gif
SHA1-Digest: mF0D5zpk68R4oaxEqoS9Q7nhm60=

```

如您所见，签名文件包含存档文件的摘要条目，这些条目看起来类似于清单中的摘要值条目。然而，清单中的摘要值是从文件本身计算出来的，而签名文件中的摘要值是从清单中的相应条目计算出来的。签名文件还包含整个清单的摘要值（请参见上面示例中的`SHA1-Digest-Manifest`头）。

当验证已签名的 JAR 文件时，将重新计算每个文件的摘要，并将其与清单中记录的摘要进行比较，以确保 JAR 文件的内容自签名以来没有发生变化。作为额外的检查，还将重新计算清单文件本身的摘要值，并将其与签名文件中记录的值进行比较。

您可以在 JDK™文档的[清单格式](https://docs.oracle.com/javase/8/docs/technotes/guides/jar/jar.html#JAR_Manifest)页面上阅读有关签名文件的其他信息。

## 签名块文件

除了签名文件外，当对 JAR 文件进行签名时，*签名块*文件会自动放置在`META-INF`目录中。与清单文件或签名文件不同，签名块文件不是人类可读的。

签名块文件包含两个用于验证的关键元素：

+   使用签名者的私钥生成的 JAR 文件的数字签名

+   包含签名者公钥的证书，供任何想要验证已签名 JAR 文件的人使用

签名块文件的文件名通常会有一个`.DSA`扩展名，表示它们是由默认数字签名算法创建的。如果使用与其他标准算法相关的密钥进行签名，则可能会有其他文件扩展名。

* * *

## 相关文档

有关密钥、证书和认证机构的其他信息，请参见

+   [JDK 安全工具](https://docs.oracle.com/javase/8/docs/technotes/tools/index.html#security)

+   [X.509 证书](https://docs.oracle.com/javase/8/docs/technotes/guides/security/cert3.html)

有关 Java 平台安全架构的更多信息，请参阅此相关文档：

+   Java SE 中的安全功能

+   [Java SE 安全性](http://www.oracle.com/technetwork/java/javase/tech/index-jsp-136007.html)

+   [安全工具](https://docs.oracle.com/javase/8/docs/technotes/tools/index.html#security)
