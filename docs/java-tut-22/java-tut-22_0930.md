# 从 URLConnection 读取和写入

> 原文：[`docs.oracle.com/javase/tutorial/networking/urls/readingWriting.html`](https://docs.oracle.com/javase/tutorial/networking/urls/readingWriting.html)

`URLConnection`类包含许多方法，让您可以通过网络与 URL 通信。`URLConnection`是一个以 HTTP 为中心的类；也就是说，当您使用 HTTP URL 时，其中许多方法才有用。然而，大多数 URL 协议允许您从连接中读取和写入。本节描述了这两个功能。

## 从 URLConnection 读取

以下程序执行与直接从 URL 读取中显示的`URLReader`程序相同的功能。

然而，与直接从 URL 获取输入流不同，此程序显式检索`URLConnection`对象并从连接获取输入流。通过调用`getInputStream`隐式打开连接。然后，像`URLReader`一样，此程序在输入流上创建一个`BufferedReader`并从中读取。粗体语句突出了此示例与先前示例之间的区别：

```java
import java.net.*;
import java.io.*;

public class URLConnectionReader {
    public static void main(String[] args) throws Exception {
        URL oracle = new URL("http://www.oracle.com/");
        URLConnection yc = oracle.openConnection();
        BufferedReader in = new BufferedReader(new InputStreamReader(
                                    yc.getInputStream()));
        String inputLine;
        while ((inputLine = in.readLine()) != null) 
            System.out.println(inputLine);
        in.close();
    }
}

```

该程序的输出与直接从 URL 打开流的程序的输出相同。您可以使用任一方式从 URL 读取。但是，与直接从 URL 读取不同，从`URLConnection`读取可能更有用。这是因为您可以同时将`URLConnection`对象用于其他任务（如向 URL 写入）。

再次，如果程序挂起或出现错误消息，您可能需要设置代理主机，以便程序可以找到 Oracle 服务器。

## 向 URLConnection 写入

许多 HTML 页面包含*表单*——文本字段和其他 GUI 对象，让您输入要发送到服务器的数据。在输入所需信息并通过单击按钮启动查询后，您的 Web 浏览器通过网络将数据写入 URL。在另一端，服务器接收数据，处理数据，然后向您发送响应，通常以新的 HTML 页面形式。

这些 HTML 表单中的许多使用 HTTP POST 方法将数据发送到服务器。因此，向 URL 写入通常称为*向 URL 发布*。服务器识别 POST 请求并读取从客户端发送的数据。

要使 Java 程序与服务器端进程交互，它只需能够写入 URL，从而向服务器提供数据。它可以通过以下步骤实现：

1.  创建一个`URL`。

1.  检索`URLConnection`对象。

1.  在`URLConnection`上设置输出功能。

1.  打开到资源的连接。

1.  从连接获取输出流。

1.  写入输出流。

1.  关闭输出流。

这里有一个名为`ReverseServlet`的小`servlet`（或者如果你更喜欢一个 cgi-bin 脚本）。您可以使用这个 servlet 来测试以下示例程序。

运行在容器中的 servlet 从其 InputStream 读取，反转字符串，并将其写入其 OutputStream。servlet 需要形式为`string=string_to_reverse`的输入，其中`string_to_reverse`是您想要以相反顺序显示其字符的字符串。

这是一个通过`URLConnection`在网络上运行`ReverseServlet`的示例程序：

```java
import java.io.*;
import java.net.*;

public class Reverse {
    public static void main(String[] args) throws Exception {

        if (args.length != 2) {
            System.err.println("Usage:  java Reverse "
                + "http://<location of your servlet/script>"
                + " string_to_reverse");
            System.exit(1);
        }

        String stringToReverse = URLEncoder.encode(args[1], "UTF-8");

        URL url = new URL(args[0]);
        URLConnection connection = url.openConnection();
        connection.setDoOutput(true);

        OutputStreamWriter out = new OutputStreamWriter(
                                         connection.getOutputStream());
        out.write("string=" + stringToReverse);
        out.close();

        BufferedReader in = new BufferedReader(
                                    new InputStreamReader(
                                    connection.getInputStream()));
        String decodedString;
        while ((decodedString = in.readLine()) != null) {
            System.out.println(decodedString);
        }
        in.close();
    }
}

```

让我们来检查程序，看看它是如何工作的。首先，程序处理其命令行参数：

```java
if (args.length != 2) {
    System.err.println("Usage:  java Reverse "
        + "http://*<location of your servlet/script>*"
        + " string_to_reverse");
    System.exit(1);
}       

String stringToReverse = URLEncoder.encode(args[1], "UTF-8");

```

这些语句确保用户向程序提供两个且仅有两个命令行参数。命令行参数是`ReverseServlet`的位置和将被反转的字符串。它可能包含空格或其他非字母数字字符。这些字符必须进行编码，因为字符串在传输到服务器时会被处理。`URLEncoder`类的方法对字符进行编码。

接下来，程序创建`URL`对象，并设置连接以便可以向其写入：

```java
URL url = new URL(args[0]);
URLConnection connection = url.openConnection();
connection.setDoOutput(true);

```

程序然后在连接上创建一个输出流，并在其上打开一个`OutputStreamWriter`：

```java
OutputStreamWriter out = new OutputStreamWriter(connection.getOutputStream());

```

如果 URL 不支持输出，`getOutputStream`方法会抛出`UnknownServiceException`。如果 URL 支持输出，则此方法返回一个连接到服务器端 URL 的输入流的输出流 —— 客户端的输出是服务器的输入。

接下来，程序向输出流写入所需的信息并关闭流：

```java
out.write("string=" + stringToReverse);
out.close();

```

这段代码使用`write`方法向输出流写入数据。因此你可以看到，向 URL 写入数据就像向流写入数据一样简单。客户端输出流中写入的数据是服务器端 servlet 的输入。`Reverse`程序通过在要反转的编码字符串前添加`string=`来构造脚本所需的输入形式。

servlet 读取您写入的信息，对字符串值执行反转操作，然后将其发送回给您。现在您需要读取服务器发送回的字符串。`Reverse`程序这样做：

```java
BufferedReader in = new BufferedReader(
                            new InputStreamReader(
                            connection.getInputStream()));
String decodedString;
while ((decodedString = in.readLine()) != null) {
    System.out.println(decodedString);
}
in.close();

```

如果您的`ReverseServlet`位于`http://example.com/servlet/ReverseServlet`，那么当您使用`Reverse`程序运行时

```java
http://example.com/servlet/ReverseServlet "Reverse Me"

```

作为参数（包括双引号），你应该看到这个输出：

```java
Reverse Me
 reversed is: 
eM esreveR

```
