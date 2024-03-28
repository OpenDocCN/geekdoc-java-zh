# 准备初始程序结构

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/vstep1.html`](https://docs.oracle.com/javase/tutorial/security/apisign/vstep1.html)

这是在本课程后续部分创建的`VerSig`程序的基本结构。将此程序结构放在名为`VerSig.java`的文件中。

```java
import java.io.*;
import java.security.*;
import java.security.spec.*;

class VerSig {

    public static void main(String[] args) {

        /* Verify a DSA signature */

        if (args.length != 3) {
            System.out.println("Usage: VerSig " +
                "publickeyfile signaturefile " + "datafile");
        }
        else try {

        // the rest of the code goes here

        } catch (Exception e) {
            System.err.println("Caught exception " + e.toString());
        }
    }

}

```

注意：

+   用于验证数据的方法位于`java.security`包中，因此程序从该包中导入所有内容。程序还从`java.io`包中导入所需的用于输入要签名的文件数据的方法，以及从`java.security.spec`包中导入包含`X509EncodedKeySpec`类的内容。

+   期望有三个参数，分别指定公钥、签名和数据文件。

+   在本课程后续步骤中编写的代码将放在`try`和`catch`块之间。
