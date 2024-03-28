# 生成密钥

> 原文：[`docs.oracle.com/javase/tutorial/security/toolfilex/step2.html`](https://docs.oracle.com/javase/tutorial/security/toolfilex/step2.html)

在签署包含`contract`文件的`Contract.jar` JAR 文件之前，如果您尚未拥有合适的密钥，则需要生成密钥。您需要使用私钥对 JAR 文件进行签名，您的接收方需要您相应的公钥来验证您的签名。

本课程假设您尚未拥有密钥对。您将创建一个名为`examplestanstore`的密钥库，并创建一个具有新生成的公钥/私钥对的条目（其中公钥在证书中）。

现在假设你是 Stan Smith，并且在 Example2 公司的法律部门工作。在命令窗口中输入以下内容，创建一个名为`examplestanstore`的密钥库，并为 Stan Smith 生成密钥：

```java
keytool -genkey -alias signLegal -keystore examplestanstore

```

密钥库工具会提示您输入密钥库密码、专有名称信息和密钥密码。以下是提示信息；粗体表示您应该输入的内容。

```java
Enter keystore password:   *<password>*
What is your first and last name?
  [Unknown]:  Stan Smith 
What is the name of your organizational unit?
  [Unknown]:  Legal 
What is the name of your organization?
  [Unknown]:  Example2 
What is the name of your City or Locality?
  [Unknown]:  New York
What is the name of your State or Province?
  [Unknown]:  NY 
What is the two-letter country code for this unit?
  [Unknown]:  US 
Is <CN=Stan Smith, OU=Legal, O=Example2, L=New York, ST=NY, C=US> correct?
  [no]:  y 

Enter key password for <signLegal>
        (RETURN if same as keystore password):

```

前面的`keytool`命令在执行命令的同一目录中（假设指定的密钥库尚不存在）创建名为`examplestanstore`的密钥库，并为具有*Stan Smith*通用名称和*Legal*组织单位的实体生成公钥/私钥对。

你刚刚创建的自签名证书包括公钥和专有名称信息。（自签名证书是由与证书中公钥对应的私钥签名的证书。）该证书有效期为 90 天。如果不指定* -validity*选项，则默认有效期为 90 天。该证书与别名为`signLegal`的密钥库条目中的私钥相关联。私钥分配了输入的密码。

自签名证书对于开发和测试应用程序非常有用。但是，用户会收到警告，应用程序是使用不受信任的证书签名的，并询问他们是否要运行该应用程序。为了让用户更有信心运行您的应用程序，请使用由认可的证书颁发机构颁发的证书。
