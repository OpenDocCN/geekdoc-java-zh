# 生成数字签名

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/gensig.html`](https://docs.oracle.com/javase/tutorial/security/apisign/gensig.html)

即将创建的`GenSig`程序将使用 JDK 安全 API 生成密钥和使用私钥为数据生成数字签名，并将公钥和签名导出到文件中。应用程序从命令行获取数据文件名。

以下步骤创建`GenSig`示例程序。

1.  准备初始程序结构

    创建一个名为`GenSig.java`的文本文件。输入初始程序结构（导入语句、类名、`main`方法等）。

1.  生成公钥和私钥

    生成一对密钥（公钥和私钥）。私钥用于对数据进行签名。公钥将被`VerSig`程序用于验证签名。

1.  对数据进行签名

    获取一个`Signature`对象并初始化以进行签名。提供要签名的数据，并生成签名。

1.  保存签名和公钥到文件中

    将签名字节保存在一个文件中，将公钥字节保存在另一个文件中。

1.  编译并运行程序
