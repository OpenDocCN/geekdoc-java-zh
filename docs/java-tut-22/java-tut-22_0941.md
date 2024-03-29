# 检索网络接口

> 原文：[`docs.oracle.com/javase/tutorial/networking/nifs/retrieving.html`](https://docs.oracle.com/javase/tutorial/networking/nifs/retrieving.html)

`NetworkInterface` 类没有公共构造函数。因此，您不能只使用`new`运算符创建此类的新实例。相反，提供了以下静态方法，以便您可以从系统中检索接口详细信息：`getByInetAddress()`、`getByName()` 和 `getNetworkInterfaces()`。前两种方法用于当您已经知道特定接口的 IP 地址或名称时。第三种方法，`getNetworkInterfaces()` 返回计算机上所有接口的完整列表。

网络接口可以按层次结构组织。`NetworkInterface` 类包括两个方法，`getParent()` 和 `getSubInterfaces()`，这些方法与网络接口层次结构相关。`getParent()` 方法返回接口的父`NetworkInterface`。如果网络接口是子接口，则`getParent()` 返回一个非空值。`getSubInterfaces()` 方法返回网络接口的所有子接口。

以下示例程序列出了计算机上所有网络接口和子接口（如果存在）的名称。

```java
import java.io.*;
import java.net.*;
import java.util.*;
import static java.lang.System.out;

public class ListNIFs 
{
    public static void main(String args[]) throws SocketException {
        Enumeration<NetworkInterface> nets = NetworkInterface.getNetworkInterfaces();

        for (NetworkInterface netIf : Collections.list(nets)) {
            out.printf("Display name: %s\n", netIf.getDisplayName());
            out.printf("Name: %s\n", netIf.getName());
            displaySubInterfaces(netIf);
            out.printf("\n");
        }
    }

    static void displaySubInterfaces(NetworkInterface netIf) throws SocketException {
        Enumeration<NetworkInterface> subIfs = netIf.getSubInterfaces();

        for (NetworkInterface subIf : Collections.list(subIfs)) {
            out.printf("\tSub Interface Display name: %s\n", subIf.getDisplayName());
            out.printf("\tSub Interface Name: %s\n", subIf.getName());
        }
     }
}  

```

以下是示例程序的样本输出：

```java
Display name: bge0
Name: bge0
    Sub Interface Display name: bge0:3
    Sub Interface Name: bge0:3
    Sub Interface Display name: bge0:2
    Sub Interface Name: bge0:2
    Sub Interface Display name: bge0:1
    Sub Interface Name: bge0:1

Display name: lo0
Name: lo0

```
