# 向多个接收者广播

> 原文：[`docs.oracle.com/javase/tutorial/networking/datagrams/broadcasting.html`](https://docs.oracle.com/javase/tutorial/networking/datagrams/broadcasting.html)

除了`DatagramSocket`，它让程序之间可以发送数据包，java.net 还包括一个名为`MulticastSocket`的类。这种类型的套接字用于客户端监听服务器广播给多个客户端的数据包。

让我们重写引用服务器，使其可以向多个接收者广播`DatagramPacket`。而不是向请求的特定客户端发送引用，新服务器现在需要定期广播引用。客户端需要进行修改，以便被动地监听引用，并在`MulticastSocket`上这样做。

这个示例由三个类组成，这三个类是前一个示例中三个类的修改版本：`MulticastServer`，`MulticastServerThread`，和`MulticastClient`。本讨论重点介绍了这些类的有趣部分。

这是服务器主程序的新版本。这段代码与之前版本`QuoteServer`的不同之处用粗体显示出来：

```java
import java.io.*;

public class MulticastServer {
    public static void main(String[] args) throws IOException {
        new MulticastServerThread().start();
    }
}

```

基本上，服务器得到了一个新的名称，并创建了一个`MulticastServerThread`而不是`QuoteServerThread`。现在让我们看看`MulticastServerThread`，其中包含服务器的核心。这是它的类声明：

```java
public class MulticastServerThread extends QuoteServerThread {
    // ...
}

```

我们将这个类作为`QuoteServerThread`的子类，以便它可以使用构造函数，并继承一些成员变量和`getNextQuote`方法。回想一下，`QuoteServerThread`创建了一个绑定到端口 4445 的`DatagramSocket`并打开引用文件。在这个示例中，`DatagramSocket`的端口号实际上并不重要，因为客户端从不向服务器发送任何内容。

`MulticastServerThread`中唯一显式实现的方法是其`run`方法。这个`run`方法与`QuoteServerThread`中的方法的不同之处用粗体显示出来：

```java
public void run() {
    while (moreQuotes) {
        try {
            byte[] buf = new byte[256];
            // don't wait for request...just send a quote

            String dString = null;
            if (in == null)
                dString = new Date().toString();
            else
                dString = getNextQuote();
            buf = dString.getBytes();

            InetAddress group = InetAddress.getByName("203.0.113.0");
            DatagramPacket packet;
            packet = new DatagramPacket(buf, buf.length, group, 4446);
            socket.send(packet);

            try {
                sleep((long)Math.random() * FIVE_SECONDS);
            } 
            catch (InterruptedException e) { }
        }
        catch (IOException e) {
            e.printStackTrace();
            moreQuotes = false;
        }
    }
    socket.close();
}

```

有趣的变化是如何构造`DatagramPacket`，特别是用于构造`DatagramPacket`的`InetAddress`和端口。回想一下，前一个示例从客户端发送到服务器的数据包中检索了`InetAddress`和端口号。这是因为服务器需要直接回复给客户端。现在，服务器需要向多个客户端发送数据。因此，这次`InetAddress`和端口号都是硬编码的。

硬编码的端口号是 4446（客户端必须将`MulticastSocket`绑定到此端口）。`DatagramPacket`的硬编码`InetAddress`是"203.0.113.0"，是一个组标识符（而不是单个客户端所在机器的互联网地址）。这个特定地址是从保留给此目的的地址中任意选择的。

以这种方式创建的`DatagramPacket`将发送到所有监听端口号为 4446 且属于"203.0.113.0"组的客户端。

要监听端口号 4446，新创建的客户端程序只需使用该端口号创建其`MulticastSocket`。要成为"203.0.113.0"组的成员，客户端调用`MulticastSocket`的`joinGroup`方法，并提供标识该组的`InetAddress`。现在，客户端已设置好接收发送到指定端口和组的`DatagramPacket`。以下是新客户端程序中相关的代码（还对其进行了重写，以 passively 接收引用而不是主动请求）。粗体语句是与`MulticastSocket`交互的语句：

```java
MulticastSocket socket = new MulticastSocket(4446);
InetAddress group = InetAddress.getByName("203.0.113.0");
socket.joinGroup(group);

DatagramPacket packet;
for (int i = 0; i < 5; i++) {
    byte[] buf = new byte[256];
    packet = new DatagramPacket(buf, buf.length);
    socket.receive(packet);

    String received = new String(packet.getData());
    System.out.println("Quote of the Moment: " + received);
}

socket.leaveGroup(group);
socket.close();

```

注意，服务器使用`DatagramSocket`广播客户端接收的`DatagramPacket`，而客户端使用`MulticastSocket`。或者，它可以使用`MulticastSocket`。服务器用于发送`DatagramPacket`的套接字并不重要。在广播数据包时重要的是`DatagramPacket`中包含的寻址信息，以及客户端用于监听的套接字

* * *

**试试这个：**

运行`MulticastServer`和几个客户端。观察客户端如何都收到相同的引用。

* * *
