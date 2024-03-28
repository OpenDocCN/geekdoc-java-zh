# 编写套接字的服务器端

> 原文：[`docs.oracle.com/javase/tutorial/networking/sockets/clientServer.html`](https://docs.oracle.com/javase/tutorial/networking/sockets/clientServer.html)

本部分向您展示如何编写一个服务器和与之配套的客户端。客户端/服务器对中的服务器提供叩叩笑话。叩叩笑话深受儿童喜爱，通常是糟糕双关语的载体。它们的形式如下：

**服务器**："叩叩！"

**客户端**："谁在那里？"

**服务器**："迪克斯特尔。"

**客户端**："迪克斯特尔是谁？"

**服务器**："迪克斯特尔大厅挂满冬青树枝。"

**客户端**："呻吟。"

该示例由两个独立运行的 Java 程序组成：客户端程序和服务器程序。客户端程序由一个类`KnockKnockClient`实现，与前一部分的`EchoClient`示例非常相似。服务器程序由两个类实现：`KnockKnockServer`和`KnockKnockProtocol`。`KnockKnockServer`类似于`EchoServer`，包含服务器程序的`main`方法，并负责监听端口、建立连接以及读取和写入套接字。类`KnockKnockProtocol`提供笑话。它跟踪当前笑话、当前状态（发送叩叩、发送提示等），并根据当前状态返回笑话的各个文本部分。该对象实现了协议——客户端和服务器约定用于通信的语言。

以下部分详细介绍了客户端和服务器中每个类的代码，然后演示如何运行它们。

## 叩叩笑话服务器

本部分将介绍实现叩叩笑话服务器程序`KnockKnockServer`的代码。

服务器程序首先通过创建一个新的[`ServerSocket`](https://docs.oracle.com/javase/8/docs/api/java/net/ServerSocket.html)对象来监听特定端口（请参见以下代码段中的粗体语句）。运行此服务器时，请选择一个尚未用于其他服务的端口。例如，以下命令启动服务器程序`KnockKnockServer`，使其监听端口 4444：

```java
java KnockKnockServer 4444

```

服务器程序在`try`-with-resources 语句中创建`ServerSocket`对象：

```java
int portNumber = Integer.parseInt(args[0]);

try ( 
    ServerSocket serverSocket = new ServerSocket(portNumber);
    Socket clientSocket = serverSocket.accept();
    PrintWriter out =
        new PrintWriter(clientSocket.getOutputStream(), true);
    BufferedReader in = new BufferedReader(
        new InputStreamReader(clientSocket.getInputStream()));
) {

```

`ServerSocket`是[`java.net`](https://docs.oracle.com/javase/8/docs/api/java/net/package-frame.html)类，提供了客户端/服务器套接字连接的服务器端的系统独立实现。如果`ServerSocket`的构造函数无法监听指定端口（例如，端口已被使用），则会抛出异常。在这种情况下，`KnockKnockServer`别无选择，只能退出。

如果服务器成功绑定到其端口，则`ServerSocket`对象成功创建，服务器继续下一步——接受来自客户端的连接（`try`-with-resources 语句中的下一条语句）：

```java
clientSocket = serverSocket.accept();

```

[`accept`](https://docs.oracle.com/javase/8/docs/api/java/net/ServerSocket.html#accept--)方法会等待，直到客户端在此服务器的主机和端口上启动并请求连接。（假设您在名为`knockknockserver.example.com`的计算机上运行了服务器程序`KnockKnockServer`。）在这个例子中，服务器正在运行在第一个命令行参数指定的端口号上。当请求连接并成功建立连接时，accept 方法会返回一个新的[`Socket`](https://docs.oracle.com/javase/8/docs/api/java/net/Socket.html)对象，该对象绑定到相同的本地端口，并将其远程地址和远程端口设置为客户端的地址和端口。服务器可以通过这个新的`Socket`与客户端通信，并继续监听原始`ServerSocket`上的客户端连接请求。这个程序的特定版本不会监听更多的客户端连接请求。然而，在支持多个客户端中提供了程序的修改版本。

在服务器成功与客户端建立连接后，使用此代码与客户端通信：

```java
try (
    // ...
    PrintWriter out =
        new PrintWriter(clientSocket.getOutputStream(), true);
    BufferedReader in = new BufferedReader(
        new InputStreamReader(clientSocket.getInputStream()));
) {
    String inputLine, outputLine;

    // Initiate conversation with client
    KnockKnockProtocol kkp = new KnockKnockProtocol();
    outputLine = kkp.processInput(null);
    out.println(outputLine);

    while ((inputLine = in.readLine()) != null) {
        outputLine = kkp.processInput(inputLine);
        out.println(outputLine);
        if (outputLine.equals("Bye."))
            break;
    }

```

该代码执行以下操作：

1.  获取套接字的输入和输出流，并在其上打开读取器和写入器。

1.  通过向套接字写入来与客户端启动通信（以粗体显示）。

1.  通过读取和写入套接字（`while`循环）与客户端通信。

第 1 步已经很熟悉。第 2 步以粗体显示，并值得一些评论。上面代码段中的粗体语句启动了与客户端的对话。代码创建了一个`KnockKnockProtocol`对象，该对象跟踪当前笑话，笑话中的当前状态等。

创建`KnockKnockProtocol`后，代码调用`KnockKnockProtocol`的`processInput`方法，以获取服务器发送给客户端的第一条消息。在这个例子中，服务器首先说的是“敲门！”。接下来，服务器将信息写入连接到客户端套接字的[`PrintWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintWriter.html)，从而将消息发送给客户端。

第 3 步在`while`循环中编码。只要客户端和服务器仍有话要说，服务器就会从套接字中读取并写入，来回发送消息。

服务器通过一个"敲门！敲门！"的方式开始对话，因此服务器必须等待客户端说"谁在那里？"。因此，`while`循环在从输入流读取时进行迭代。`readLine`方法会等待客户端通过写入内容到其输出流（服务器的输入流）来做出响应。当客户端做出响应时，服务器将客户端的响应传递给`KnockKnockProtocol`对象，并要求`KnockKnockProtocol`对象提供合适的回复。服务器立即通过连接到套接字的输出流将回复发送给客户端，使用`println`方法。如果从`KnockKnockServer`对象生成的服务器响应是"再见。"，这表示客户端不想再听笑话了，循环结束。

Java 运行时会自动关闭输入和输出流、客户端套接字和服务器套接字，因为它们是在`try`-with-resources 语句中创建的。

## 敲门协议

`KnockKnockProtocol`类实现了客户端和服务器用于通信的协议。该类跟踪客户端和服务器在对话中的位置，并为客户端的陈述提供服务器的响应。`KnockKnockProtocol`对象包含所有笑话的文本，并确保客户端对服务器的陈述给出正确的回应。当服务器说"敲门！敲门！"时，客户端说"谁在那里？"才是正确的回应。

所有的客户端/服务器对都必须有一些协议来进行通信；否则，来回传递的数据将毫无意义。你自己的客户端和服务器使用的协议完全取决于它们完成任务所需的通信。

## 敲门客户端

`KnockKnockClient`类实现了与`KnockKnockServer`通信的客户端程序。`KnockKnockClient`基于前一节中的`EchoClient`程序从套接字读取和写入，应该对您来说有些熟悉。但我们仍然会查看程序并了解客户端中发生的事情，以及与服务器中发生的情况相比较。

当您启动客户端程序时，服务器应该已经在运行并监听端口，等待客户端请求连接。因此，客户端程序的第一步是打开一个连接到指定主机名和端口上运行的服务器的套接字：

```java
String hostName = args[0];
int portNumber = Integer.parseInt(args[1]);

try (
    Socket kkSocket = new Socket(hostName, portNumber);
    PrintWriter out = new PrintWriter(kkSocket.getOutputStream(), true);
    BufferedReader in = new BufferedReader(
        new InputStreamReader(kkSocket.getInputStream()));
)

```

在创建套接字时，`KnockKnockClient`示例使用第一个命令行参数的主机名，即运行服务器程序`KnockKnockServer`的网络计算机的名称。

`KnockKnockClient`示例在创建套接字时使用第二个命令行参数作为端口号。这是一个*远程端口号*——服务器计算机上的端口号——是`KnockKnockServer`正在监听的端口。例如，以下命令在运行`KnockKnockServer`程序的计算机上以`knockknockserver.example.com`作为服务器程序`KnockKnockServer`的计算机名称，4444 作为远程端口号运行`KnockKnockClient`示例：

```java
java KnockKnockClient knockknockserver.example.com 4444

```

客户端的套接字绑定到任何可用的*本地端口*——客户端计算机上的端口。请记住，服务器也会获得一个新的套接字。如果你使用前面示例中的命令行参数运行`KnockKnockClient`示例，那么这个套接字将绑定到你运行`KnockKnockClient`示例的计算机上的本地端口号 4444。服务器的套接字和客户端的套接字已连接。

接下来是实现客户端和服务器之间通信的`while`循环。服务器先发言，所以客户端必须先听。客户端通过从连接到套接字的输入流中读取来实现这一点。如果服务器发言，它会说“再见”，然后客户端退出循环。否则，客户端将文本显示到标准输出，然后从用户那里读取响应，用户通过标准输入键入。用户键入回车后，客户端通过连接到套接字的输出流将文本发送到服务器。

```java
while ((fromServer = in.readLine()) != null) {
    System.out.println("Server: " + fromServer);
    if (fromServer.equals("Bye."))
        break;

    fromUser = stdIn.readLine();
    if (fromUser != null) {
        System.out.println("Client: " + fromUser);
        out.println(fromUser);
    }
}

```

当服务器询问客户端是否想听另一个笑话时，通信结束，客户端说不想听，服务器说“再见”。

客户端会自动关闭其输入和输出流以及套接字，因为它们是在`try`-with-resources 语句中创建的。

## 运行程序

你必须先启动服务器程序。为此，请使用 Java 解释器运行服务器程序，就像运行任何其他 Java 应用程序一样。指定服务器程序监听的端口号作为命令行参数：

```java
java KnockKnockServer 4444

```

接下来，运行客户端程序。请注意，你可以在网络上的任何计算机上运行客户端；它不必在与服务器相同的计算机上运行。指定运行`KnockKnockServer`服务器程序的计算机的主机名和端口号作为命令行参数：

```java
java KnockKnockClient knockknockserver.example.com 4444

```

如果你太快了，可能会在服务器有机会初始化并开始监听端口之前启动客户端。如果发生这种情况，你将会看到客户端的堆栈跟踪。如果发生这种情况，只需重新启动客户端。

如果在第一个客户端连接到服务器时尝试启动第二个客户端，第二个客户端将会挂起。下一节，支持多个客户端，讨论了支持多个客户端的问题。

当客户端和服务器成功建立连接时，你将在屏幕上看到以下文本显示：

```java
Server: Knock! Knock!

```

现在，你必须回复：

```java
Who's there?

```

客户端会回显你输入的内容并将文本发送到服务器。服务器会回复其中一个众多“敲敲”笑话的第一行。现在你的屏幕应该显示如下内容（你输入的文本用粗体表示）：

```java
Server: Knock! Knock!
Who's there?
Client: Who's there?
Server: Turnip

```

现在，你回复：

```java
Turnip who?

```

再次，客户端会回显你输入的内容并将文本发送到服务器。服务器会回复一个笑话的结尾。现在你的屏幕应该显示如下内容：

```java
Server: Knock! Knock!
Who's there?
Client: Who's there?
Server: Turnip
Turnip who?
Client: Turnip who?
Server: Turnip the heat, it's cold in here! Want another? (y/n)   

```

如果你想听另一个笑话，输入**y**；如果不想，输入**n**。如果输入**y**，服务器会重新开始“敲敲”；如果输入**n**，服务器会说“再见”，导致客户端和服务器都退出。

如果在任何时候你打错字，`KnockKnockServer`对象会捕捉到并回复类似于这样的消息：

```java
Server: You're supposed to say "Who's there?"!

```

然后服务器会重新开始讲笑话：

```java
Server: Try again. Knock! Knock!

```

注意，`KnockKnockProtocol`对象对拼写和标点符号很在意，但对大小写不在意。

## 支持多个客户端

为了保持`KnockKnockServer`示例简单，我们设计它来监听并处理单个连接请求。然而，多个客户端请求可以进入同一端口，因此也进入同一个`ServerSocket`。客户端连接请求在端口处排队，因此服务器必须按顺序接受连接。然而，服务器可以通过使用线程同时为它们提供服务——每个客户端连接一个线程。

这样一个服务器的基本逻辑流程是这样的：

```java
while (true) {
    accept a connection;
    create a thread to deal with the client;
}

```

线程根据需要从客户端连接读取和写入。

* * *

**试一试：**

修改`KnockKnockServer`以便能够同时为多个客户端提供服务。我们的解决方案由两个类组成：`KKMultiServer` 和 `KKMultiServerThread`。`KKMultiServer`永远循环，监听`ServerSocket`上的客户端连接请求。当有请求进来时，`KKMultiServer`接受连接，创建一个新的`KKMultiServerThread`对象来处理它，将从 accept 返回的套接字传递给它，并启动线程。然后服务器继续监听连接请求。`KKMultiServerThread`对象通过读取和写入套接字与客户端通信。运行新的“敲敲”服务器`KKMultiServer`，然后依次运行几个客户端。

* * *
