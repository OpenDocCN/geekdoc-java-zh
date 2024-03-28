# 列出网络接口地址

> 原文：[`docs.oracle.com/javase/tutorial/networking/nifs/listing.html`](https://docs.oracle.com/javase/tutorial/networking/nifs/listing.html)

从网络接口获取的最有用的信息之一是分配给它的 IP 地址列表。您可以通过使用两种方法之一从`NetworkInterface`实例中获取此信息。第一种方法`getInetAddresses()`返回一个`InetAddress`的`Enumeration`。另一种方法`getInterfaceAddresses()`返回一个[`java.net.InterfaceAddress`](https://docs.oracle.com/javase/8/docs/api/java/net/InterfaceAddress.html)实例的列表。当您需要有关接口地址的更多信息时，可以使用此方法。例如，当地址是 IPv4 地址时，您可能需要有关子网掩码和广播地址的附加信息，以及在 IPv6 地址的情况下的网络前缀长度。

以下示例程序列出了计算机上所有网络接口及其地址：

```java
import java.io.*;
import java.net.*;
import java.util.*;
import static java.lang.System.out;

public class ListNets {

    public static void main(String args[]) throws SocketException {
        Enumeration<NetworkInterface> nets = NetworkInterface.getNetworkInterfaces();
        for (NetworkInterface netint : Collections.list(nets))
            displayInterfaceInformation(netint);
    }

    static void displayInterfaceInformation(NetworkInterface netint) throws SocketException {
        out.printf("Display name: %s\n", netint.getDisplayName());
        out.printf("Name: %s\n", netint.getName());
        Enumeration<InetAddress> inetAddresses = netint.getInetAddresses();
        for (InetAddress inetAddress : Collections.list(inetAddresses)) {
            out.printf("InetAddress: %s\n", inetAddress);
        }
        out.printf("\n");
     }
}  

```

以下是示例程序的输出样本：

```java
Display name: TCP Loopback interface
Name: lo
InetAddress: /127.0.0.1

Display name: Wireless Network Connection
Name: eth0
InetAddress: /192.0.2.0

```
