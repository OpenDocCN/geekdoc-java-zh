# 验证 JAR 文件签名。

> [`docs.oracle.com/javase/tutorial/security/toolfilex/rstep2.html`](https://docs.oracle.com/javase/tutorial/security/toolfilex/rstep2.html)

作为 Ruth，您现在已将 Stan 的公钥证书导入到`exampleruthstore`密钥库中作为“受信任的证书”。您现在可以使用`jarsigner`工具来验证 JAR 文件签名的真实性。

当您验证已签名的 JAR 文件时，您验证签名是否有效，以及 JAR 文件是否未被篡改。您可以通过以下命令对`sContract.jar`文件进行此操作：

```java
jarsigner -verify -verbose -keystore exampleruthstore sContract.jar 

```

您应该看到类似以下内容：

```java
       183 Fri Jul 31 10:49:54 PDT 1998 META-INF/SIGNLEGAL.SF
       1542 Fri Jul 31 10:49:54 PDT 1998 META-INF/SIGNLEGAL.DSA
       0 Fri Jul 31 10:49:18 PDT 1998 META-INF/
smk    1147 Wed Jul 29 16:06:12 PDT 1998 contract

 s = signature was verified 
 m = entry is listed in manifest
 k = at least one certificate was found in keystore
 i = at least one certificate was found in identity scope

jar verified.

```

请务必使用`-verbose`选项运行命令，以获取足够的信息以确保以下内容：

+   合同文件是 JAR 文件中的文件之一，已签名并验证其签名（这就是`s`的意思）。

+   用于验证签名的公钥位于指定的密钥库中，因此您信任它（这就是`k`的意思）。
