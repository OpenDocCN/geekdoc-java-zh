# 从套接字读取和写入

> 译文：[`docs.oracle.com/javase/tutorial/networking/sockets/readingWriting.html`](https://docs.oracle.com/javase/tutorial/networking/sockets/readingWriting.html)

让我们看一个简单的示例，说明程序如何使用`Socket`类与服务器程序建立连接，然后客户端如何通过套接字向服务器发送数据并从服务器接收数据。

该示例程序实现了一个客户端，`EchoClient`，连接到一个回显服务器。回显服务器从其客户端接收数据并将其回显。示例`EchoServer`实现了一个回显服务器。（或者，客户端可以连接到支持[Echo Protocol](http://tools.ietf.org/html/rfc862)的任何主机。）

`EchoClient`示例创建一个套接字，从而连接到回显服务器。它从标准输入流中读取用户输入，然后通过将文本写入套接字将该文本转发到回显服务器。服务器通过套接字将输入回显到客户端。客户端程序读取并显示从服务器传回的数据。

请注意，`EchoClient`示例既向其套接字写入数据，也从其套接字读取数据，从而向回显服务器发送数据并接收数据。

让我们逐步走过程序并研究有趣的部分。在`EchoClient`示例中的`try`-with-resources 语句中，以下语句至关重要。这些行建立了客户端和服务器之间的套接字连接，并在套接字上打开了一个[`PrintWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintWriter.html)和一个[`BufferedReader`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)：

```java
String hostName = args[0];
int portNumber = Integer.parseInt(args[1]);

try (
    Socket echoSocket = new Socket(hostName, portNumber);        // 1st statement
    PrintWriter out =                                            // 2nd statement
        new PrintWriter(echoSocket.getOutputStream(), true);
    BufferedReader in =                                          // 3rd statement 
        new BufferedReader(
            new InputStreamReader(echoSocket.getInputStream()));
    BufferedReader stdIn =                                       // 4th statement 
        new BufferedReader(
            new InputStreamReader(System.in))
)

```

`try`-with 资源语句中的第一条语句创建了一个新的[`Socket`](https://docs.oracle.com/javase/8/docs/api/java/net/Socket.html)对象，并命名为`echoSocket`。此处使用的`Socket`构造函数需要要连接的计算机的名称和端口号。示例程序使用第一个命令行参数作为计算机的名称（主机名），第二个命令行参数作为端口号。当您在计算机上运行此程序时，请确保您使用的主机名是要连接的计算机的完全限定 IP 名称。例如，如果您的回显服务器在计算机`echoserver.example.com`上运行，并且正在侦听端口号 7，则如果您想将`EchoServer`示例用作您的回显服务器，请先从计算机`echoserver.example.com`运行以下命令：

```java
java EchoServer 7

```

后续，使用以下命令运行`EchoClient`示例：

```java
java EchoClient echoserver.example.com 7

```

`try`-with 资源语句中的第二个语句获取套接字的输出流，并在其上打开名为`out`的`PrintWriter`。类似地，第三个语句获取套接字的输入流，并在其上打开名为`in`的`BufferedReader`。该示例使用读取器和写入器，以便可以通过套接字写入 Unicode 字符。如果您还不熟悉 Java 平台的 I/O 类，请阅读基本 I/O。

程序的下一个有趣部分是`while`循环。该循环从标准输入流中逐行读取数据，使用在`try`-with 资源语句中创建的`BufferedReader`对象`stdIn`。然后，该循环立即将该行数据通过写入到与套接字连接的`PrintWriter`发送到服务器：

```java
String userInput;
while ((userInput = stdIn.readLine()) != null) {
    out.println(userInput);
    System.out.println("echo: " + in.readLine());
}

```

`while`循环中的最后一个语句从与套接字连接的`BufferedReader`中读取一行信息。`readLine`方法会等待服务器将信息回显给`EchoClient`。当`readline`返回时，`EchoClient`将信息打印到标准输出。

`while`循环会一直持续，直到用户输入结束符号。也就是说，`EchoClient`示例从用户那里读取输入，将其发送到 Echo 服务器，从服务器获取响应，并显示它，直到达到输入结束。 （您可以通过按下**Ctrl-C**键来输入结束符号。）然后`while`循环终止，并且 Java 运行时会自动关闭与套接字和标准输入流连接的读取器和写入器，并关闭与服务器的套接字连接。Java 运行时会自动关闭这些资源，因为它们是在`try`-with 资源语句中创建的。Java 运行时会按照它们创建的相反顺序关闭这些资源。（这很好，因为连接到套接字的流应该在套接字本身关闭之前关闭。）

该客户端程序非常简单直接，因为回显服务器实现了一个简单的协议。客户端向服务器发送文本，服务器将其回显。当您的客户端程序与更复杂的服务器（如 HTTP 服务器）通信时，您的客户端程序也会更复杂。但是，基本原理与本程序中的相同：

1.  打开一个套接字。

1.  打开一个输入流和输出流到套接字。

1.  根据服务器的协议从流中读取和写入数据。

1.  关闭流。

1.  关闭套接字。

仅步骤 3 因客户端而异，取决于服务器。其他步骤基本保持不变。
