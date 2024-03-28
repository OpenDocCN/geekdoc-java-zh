# 直接从 URL 读取

> 原文：[`docs.oracle.com/javase/tutorial/networking/urls/readingURL.html`](https://docs.oracle.com/javase/tutorial/networking/urls/readingURL.html)

成功创建`URL`后，您可以调用`URL`的`openStream()`方法来获取一个流，从中可以读取 URL 的内容。`openStream()`方法返回一个[`java.io.InputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/InputStream.html)对象，因此从 URL 读取就像从输入流读取一样简单。

以下的小型 Java 程序使用`openStream()`来在 URL `http://www.oracle.com/`上获取输入流。然后在输入流上打开一个`BufferedReader`，并从`BufferedReader`读取，从而从 URL 读取。所有读取的内容都被复制到标准输出流：

```java
import java.net.*;
import java.io.*;

public class URLReader {
    public static void main(String[] args) throws Exception {

        URL oracle = new URL("http://www.oracle.com/");
        BufferedReader in = new BufferedReader(
        new InputStreamReader(oracle.openStream()));

        String inputLine;
        while ((inputLine = in.readLine()) != null)
            System.out.println(inputLine);
        in.close();
    }
}

```

当您运行该程序时，您应该在命令窗口中看到从`http://www.oracle.com/`位置的 HTML 文件中滚动显示的 HTML 命令和文本内容。或者，程序可能会挂起，或者您可能会看到异常堆栈跟踪。如果出现后两种情况中的任何一种，您可能需要设置代理主机以便程序可以找到 Oracle 服务器。
