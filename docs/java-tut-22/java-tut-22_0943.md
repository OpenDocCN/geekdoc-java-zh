# 网络接口参数

> 原文：[`docs.oracle.com/javase/tutorial/networking/nifs/parameters.html`](https://docs.oracle.com/javase/tutorial/networking/nifs/parameters.html)

你可以访问有关网络接口的网络参数，除了分配给它的名称和 IP 地址之外。

你可以使用`isUP()`方法来发现网络接口是否处于“up”（即运行）状态。以下方法指示网络接口类型：

+   `isLoopback()`指示网络接口是否为环回接口。

+   `isPointToPoint()`指示接口是否为点对点接口。

+   `isVirtual()`指示接口是否为虚拟接口。

`supportsMulticast()`方法指示网络接口是否支持多播。`getHardwareAddress()`方法在可用时返回网络接口的物理硬件地址，通常称为 MAC 地址。`getMTU()`方法返回最大传输单元（MTU），即最大数据包大小。

以下示例在列出网络接口地址的基础上，通过添加本页描述的额外网络参数进行了扩展：

```java
import java.io.*;
import java.net.*;
import java.util.*;
import static java.lang.System.out;

public class ListNetsEx {

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

        out.printf("Up? %s\n", netint.isUp());
        out.printf("Loopback? %s\n", netint.isLoopback());
        out.printf("PointToPoint? %s\n", netint.isPointToPoint());
        out.printf("Supports multicast? %s\n", netint.supportsMulticast());
        out.printf("Virtual? %s\n", netint.isVirtual());
        out.printf("Hardware address: %s\n",
                    Arrays.toString(netint.getHardwareAddress()));
        out.printf("MTU: %s\n", netint.getMTU());
        out.printf("\n");
     }
}  

```

以下是示例程序的输出样本：

```java
Display name: bge0
Name: bge0
InetAddress: /fe80:0:0:0:203:baff:fef2:e99d%2
InetAddress: /129.156.225.59
Up? true
Loopback? false
PointToPoint? false
Supports multicast? false
Virtual? false
Hardware address: [0, 3, 4, 5, 6, 7]
MTU: 1500

Display name: lo0
Name: lo0
InetAddress: /0:0:0:0:0:0:0:1%1
InetAddress: /127.0.0.1
Up? true
Loopback? true
PointToPoint? false
Supports multicast? false
Virtual? false
Hardware address: null
MTU: 8232

```
