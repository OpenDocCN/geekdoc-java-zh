# 生成密钥

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/step3.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/step3.html)

如果代码签名者尚未拥有适合签署代码的私钥，必须首先生成该私钥，以及一个相应的公钥，供代码接收方的运行时系统用于验证签名。

由于这节课假设你还没有这样的密钥，你将创建一个名为`examplestore`的密钥库，并创建一个带有新生成的公钥/私钥对的条目（其中公钥在证书中）。

在命令窗口中输入以下命令以创建一个名为`examplestore`的密钥库并生成密钥：

```java
keytool -genkey -alias signFiles -keystore examplestore

```

您将被提示输入密钥和密钥库的密码。

## `keytool`命令的子部分

让我们看看`keytool`的每个子部分的含义。

+   生成密钥的命令是*-genkey*。

+   *-alias signFiles*子部分指示将来用于引用包含将生成的密钥的密钥库条目的别名。

+   *-keystore examplestore*子部分指示您正在创建或已经使用的密钥库的名称（和可选路径）。

+   你被提示输入的*storepass*值指定了密钥库密码。

+   你被提示输入的*keypass*值指定了即将生成的私钥的密码。您始终需要此密码才能访问包含该密钥的密钥库条目。该条目不必有自己的密码。当提示您输入密钥密码时，您可以选择让它与密钥库密码相同。

**注意：**出于安全原因，您不应在命令行上设置密钥或密钥库密码，因为这样更容易被拦截。

## 专有名称信息

如果您使用上述`keystore`命令，将提示您输入您的专有名称信息。以下是提示信息；粗体表示您应该输入的内容。

```java
What is your first and last name?
  [Unknown]:  Susan Jones 
What is the name of your organizational unit?
  [Unknown]:  Purchasing 
What is the name of your organization?
  [Unknown]:  ExampleCompany 
What is the name of your City or Locality?
  [Unknown]:  Cupertino 
What is the name of your State or Province?
  [Unknown]:  CA 
What is the two-letter country code for this unit?
  [Unknown]:  US 
Is <CN=Susan Jones, OU=Purchasing, O=ExampleCompany,
    L=Cupertino, ST=CA, C=US> correct?
  [no]:  y 

```

## 命令结果

`keytool`命令在执行命令的同一目录中创建名为`examplestore`的密钥库（如果尚不存在）。该命令为具有 Susan Jones 作为通用名称和采购部门的实体生成公钥/私钥对。

该命令创建一个包含公钥和专有名称信息的自签名证书。（您提供的专有名称将用作证书中的“主题”字段。）该证书将在 90 天内有效，如果您不指定*-validity*选项，则为默认有效期。该证书与密钥库条目中的私钥关联，该条目由别名`signFiles`引用。

自签名证书对于开发和测试应用程序非常有用。然而，用户会收到警告，指出应用程序是使用不受信任的证书签名的，并询问他们是否要运行该应用程序。为了让用户更有信心地运行您的应用程序，请使用由认可的证书颁发机构颁发的证书。

**注意：** 如果接受选项的默认值或希望提示输入各种值，命令可能会更短。每当执行`keytool`命令时，对于未指定具有默认值的选项，将使用默认值，并提示您输入任何必需的值。对于`genkey`命令，具有默认值的选项包括*别名*（默认为`mykey`）、*有效期*（90 天）和*密钥库*（位于您的主目录中名为`.keystore`的文件）。必需的值包括*dname*、*storepass*和*keypass*。
