# 准备初始程序结构

> 原文：[`docs.oracle.com/javase/tutorial/security/apisign/step1.html`](https://docs.oracle.com/javase/tutorial/security/apisign/step1.html)

这是`GenSig`程序的基本结构。将其放在名为`GenSig.java`的文件中。

```java
import java.io.*;
import java.security.*;

class GenSig {

    public static void main(String[] args) {

        /* Generate a DSA signature */

        if (args.length != 1) {
            System.out.println("Usage: GenSig nameOfFileToSign");
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

+   签署数据的方法位于`java.security`包中，因此程序从该包中导入所有内容。程序还导入了`java.io`包，其中包含输入要签名的文件数据所需的方法。

+   期望提供一个参数，指定要签名的数据文件。

+   后续步骤中编写的代码将放在`try`和`catch`块之间。
