# 编写数据报客户端和服务器

> 原文：[`docs.oracle.com/javase/tutorial/networking/datagrams/clientServer.html`](https://docs.oracle.com/javase/tutorial/networking/datagrams/clientServer.html)

本节中展示的示例由两个应用程序组成：客户端和服务器。服务器持续接收数据报包，每个数据报包都表示客户端请求报价。当服务器接收到数据报时，它会通过数据报包将一行“此刻引用”发送回客户端。

本示例中的客户端应用程序非常简单。它向服务器发送一个数据报包，指示客户端希望接收此刻引用。然后客户端等待服务器发送数据报包作为响应。

两个类实现了服务器应用程序：`QuoteServer` 和 `QuoteServerThread`。一个类实现了客户端应用程序：`QuoteClient`。

让我们从包含服务器应用程序的`main`方法的类开始研究这些类。使用服务器端应用程序包含`QuoteClient`类的小程序版本。

## 引用服务器类

在这里完整显示的`QuoteServer`类包含一个方法：引用服务器应用程序的`main`方法。`main`方法只是创建一个新的`QuoteServerThread`对象并启动它：

```java
import java.io.*;

public class QuoteServer {
    public static void main(String[] args) throws IOException {
        new QuoteServerThread().start();
    }
}

```

`QuoteServerThread` 类实现了引用服务器的主要逻辑。

## `QuoteServerThread` 类

创建时，`QuoteServerThread`在端口 4445 上创建了一个`DatagramSocket`（任意选择）。这是服务器与所有客户端通信的`DatagramSocket`。

```java
public QuoteServerThread() throws IOException {
    this("QuoteServer");
}

public QuoteServerThread(String name) throws IOException {
    super(name);
    socket = new DatagramSocket(4445);

    try {
        in = new BufferedReader(new FileReader("one-liners.txt"));
    }   
    catch (FileNotFoundException e){
        System.err.println("Couldn't open quote file.  Serving time instead.");
    }
}  

```

请记住，某些端口专用于已知服务，您不能使用它们。如果指定一个正在使用的端口，`DatagramSocket`的创建将失败。

构造函数还在名为`one-liners.txt`的文件上打开了一个`BufferedReader`，其中包含引用列表。文件中的每个引用都在单独的一行上。

现在来看`QuoteServerThread`的有趣部分：它的`run`方法。`run`方法覆盖了`Thread`类中的`run`方法，并为线程提供了实现。有关线程的信息，请参见定义和启动线程。

`run` 方法包含一个`while`循环，只要文件中还有更多引用，循环就会继续。在循环的每次迭代中，线程会等待一个`DatagramPacket`通过`DatagramSocket`到达。数据包表示客户端的请求。作为对客户端请求的响应，`QuoteServerThread`从文件中获取引用，将其放入`DatagramPacket`中，并通过`DatagramSocket`将其发送到请求的客户端。

让我们首先看一下接收客户端请求的部分：

```java
byte[] buf = new byte[256];
DatagramPacket packet = new DatagramPacket(buf, buf.length);
socket.receive(packet);

```

第一条语句创建了一个字节数组，然后用它创建了一个`DatagramPacket`。`DatagramPacket`将用于从套接字接收数据报，因为使用了创建它的构造函数。这个构造函数只需要两个参数：一个包含客户端特定数据的字节数组和字节数组的长度。在构造要通过`DatagramSocket`发送的`DatagramPacket`时，还必须提供数据包目的地的互联网地址和端口号。当我们讨论服务器如何响应客户端请求时，你会看到这一点。

前一个代码片段中的最后一条语句从套接字接收一个数据报（从客户端接收的信息被复制到数据包中）。`receive`方法会一直等待，直到接收到一个数据包。如果没有接收到数据包，服务器将不会有进一步的进展，只会等待。

现在假设，服务器收到了客户端请求报价的请求。现在服务器必须做出响应。`run`方法中的这部分代码构建了响应：

```java
String dString = null;
if (in == null)
    dString = new Date().toString();
else
    dString = getNextQuote();
buf = dString.getBytes();

```

如果由于某种原因未打开报价文件，则`in`等于 null。如果是这种情况，报价服务器将提供当天的时间。否则，报价服务器从已打开的文件中获取下一个报价。最后，代码将字符串转换为字节数组。

现在，`run`方法使用以下代码向客户端通过`DatagramSocket`发送响应：

```java
InetAddress address = packet.getAddress();
int port = packet.getPort();
packet = new DatagramPacket(buf, buf.length, address, port);
socket.send(packet);

```

这段代码中的前两条语句分别从客户端接收的数据报中获取了互联网地址和端口号。互联网地址和端口号指示了数据报包的来源。这是服务器必须发送响应的地方。在这个例子中，数据报包的字节数组不包含相关信息。根据报包的到达本身就表示了来自互联网地址和端口号所指示的客户端的请求。

第三条语句创建了一个新的`DatagramPacket`对象，用于通过数据报套接字发送数据报消息。你可以通过创建它的构造函数来判断新的`DatagramPacket`是用于通过套接字发送数据的。这个构造函数需要四个参数。前两个参数与用于创建接收数据报的构造函数相同：包含从发送方到接收方的消息的字节数组和该数组的长度。接下来的两个参数不同：一个互联网地址和一个端口号。这两个参数是数据报包的目的地的完整地址，必须由数据报的发送方提供。最后一行代码将`DatagramPacket`发送出去。

当服务器从报价文件中读取了所有报价后，`while`循环终止，`run`方法进行清理：

```java
socket.close();

```

## QuoteClient 类

`QuoteClient`类实现了`QuoteServer`的客户端应用程序。此应用程序向`QuoteServer`发送请求，等待响应，并在接收到响应时将其显示到标准输出。让我们详细看看代码。

`QuoteClient`类包含一个方法，即客户端应用程序的`main`方法。`main`方法的顶部声明了几个局部变量供其使用：

```java
int port;
InetAddress address;
DatagramSocket socket = null;
DatagramPacket packet;
byte[] sendBuf = new byte[256];

```

首先，`main`方法处理用于调用`QuoteClient`应用程序的命令行参数：

```java
if (args.length != 1) {
    System.out.println("Usage: java QuoteClient <hostname>");
    return;
}

```

`QuoteClient`应用程序需要一个命令行参数：运行`QuoteServer`的机器的名称。

接下来，`main`方法创建一个`DatagramSocket`：

```java
DatagramSocket socket = new DatagramSocket();

```

客户端使用一个不需要端口号的构造函数。此构造函数只是将`DatagramSocket`绑定到任何可用的本地端口。客户端绑定到的端口号并不重要，因为`DatagramPacket`包含寻址信息。服务器从`DatagramPacket`中获取端口号并将其响应发送到该端口。

接下来，`QuoteClient`程序向服务器发送一个请求：

```java
byte[] buf = new byte[256];
InetAddress address = InetAddress.getByName(args[0]);
DatagramPacket packet = new DatagramPacket(buf, buf.length, 
                                address, 4445);
socket.send(packet);

```

代码段获取命令行上命名的主机的 Internet 地址（假设服务器运行的机器的名称）。然后使用此`InetAddress`和端口号 4445（服务器用于创建其`DatagramSocket`的端口号）创建一个发送到该 Internet 地址和端口号的`DatagramPacket`。因此，`DatagramPacket`将被传递到引用服务器。

请注意，代码创建了一个带有空字节数组的`DatagramPacket`。字节数组为空，因为此数据报包仅仅是向服务器请求信息。服务器发送响应所需的所有信息—地址和端口号—自动包含在数据包中。

接下来，客户端从服务器获取响应并显示出来：

```java
packet = new DatagramPacket(buf, buf.length);
socket.receive(packet);
String received = new String(packet.getData(), 0, packet.getLength());
System.out.println("Quote of the Moment: " + received);

```

要从服务器获取响应，客户端创建一个“接收”数据包，并使用`DatagramSocket`的接收方法从服务器接收回复。接收方法会等待直到一个发送给客户端的数据包通过套接字传输过来。请注意，如果服务器的回复某种原因丢失，客户端将永远等待，因为数据报模型没有保证策略。通常，客户端会设置一个计时器，以便不会永远等待回复；如果没有收到回复，计时器会触发，客户端会重新发送。

当客户端从服务器接收到回复时，客户端使用`getData`方法从数据包中检索数据。然后客户端将数据转换为字符串并显示出来。

## 运行服务器和客户端

在成功编译服务器和客户端程序之后，您可以运行它们。必须先运行服务器程序。只需使用 Java 解释器并指定`QuoteServer`类名。

一旦服务器启动，你可以运行客户端程序。记得用一个命令行参数来运行客户端程序：`QuoteServer`运行的主机名。

当客户端发送请求并从服务器接收到响应后，你应该看到类似于以下输出：

```java
Quote of the Moment:
Good programming is 99% sweat and 1% coffee.

```
