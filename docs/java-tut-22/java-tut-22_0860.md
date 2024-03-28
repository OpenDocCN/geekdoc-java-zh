# 将签名和公钥保存在文件中

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/step4.html`](https://docs.oracle.com/javase/tutorial/security/apisign/step4.html)

现在您已经为某些数据生成了签名，您需要将签名字节保存在一个文件中，将公钥字节保存在另一个文件中，这样您就可以通过调制解调器、软盘、邮件等方式将其发送给其他人。

+   生成签名的数据，

+   签名，

+   公钥

接收方可以通过运行您将在接下来的验证数字签名步骤中生成的`VerSig`程序来验证数据是否来自您，并且在传输过程中没有被修改。该程序使用公钥来验证接收到的签名是否是接收到的数据的真实签名。

回想一下，签名是放在一个名为`realSig`的字节数组中的。您可以通过以下方式将签名字节保存在名为`sig`的文件中。

```java
/* save the signature in a file */
FileOutputStream sigfos = new FileOutputStream("sig");
sigfos.write(realSig);
sigfos.close();

```

从生成公钥和私钥步骤中回想一下，公钥是放在一个名为`pub`的 PublicKey 对象中的。您可以通过调用`getEncoded`方法获取编码后的密钥字节，然后将编码后的字节存储在一个文件中。您可以随意命名文件。例如，如果您的名字是 Susan，您可以将其命名为`suepk`（代表"Sue 的公钥"），如下所示：

```java
/* save the public key in a file */
byte[] key = pub.getEncoded();
FileOutputStream keyfos = new FileOutputStream("suepk");
keyfos.write(key);
keyfos.close();

```
