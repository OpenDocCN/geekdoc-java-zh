# 实现远程接口。

> 原文：[`docs.oracle.com/javase/tutorial/rmi/implementing.html`](https://docs.oracle.com/javase/tutorial/rmi/implementing.html)

本节讨论实现计算引擎类的任务。一般来说，实现远程接口的类至少应该执行以下操作：

+   声明正在实现的远程接口。

+   为每个远程对象定义构造函数。

+   为远程接口中的每个远程方法提供实现。

RMI 服务器程序需要创建初始远程对象并将其*导出*到 RMI 运行时，使其可用于接收传入的远程调用。此设置过程可以封装在远程对象实现类本身的方法中，也可以完全包含在另一个类中。设置过程应执行以下操作：

+   创建并安装安全管理器

+   创建并导出一个或多个远程对象。

+   至少在 RMI 注册表（或其他命名服务，如通过 Java 命名和目录接口访问的服务）中注册一个远程对象，用于引导目的。

计算引擎的完整实现如下。``engine.ComputeEngine``类实现了远程接口`Compute`，并包括用于设置计算引擎的`main`方法。以下是`ComputeEngine`类的源代码：

```java
package engine;

import java.rmi.RemoteException;
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;
import java.rmi.server.UnicastRemoteObject;
import compute.Compute;
import compute.Task;

public class ComputeEngine implements Compute {

    public ComputeEngine() {
        super();
    }

    public <T> T executeTask(Task<T> t) {
        return t.execute();
    }

    public static void main(String[] args) {
        if (System.getSecurityManager() == null) {
            System.setSecurityManager(new SecurityManager());
        }
        try {
            String name = "Compute";
            Compute engine = new ComputeEngine();
            Compute stub =
                (Compute) UnicastRemoteObject.exportObject(engine, 0);
            Registry registry = LocateRegistry.getRegistry();
            registry.rebind(name, stub);
            System.out.println("ComputeEngine bound");
        } catch (Exception e) {
            System.err.println("ComputeEngine exception:");
            e.printStackTrace();
        }
    }
}

```

以下各节讨论计算引擎实现的每个组件。

## 声明正在实现的远程接口。

计算引擎的实现类声明如下：

```java
public class ComputeEngine implements Compute

```

此声明说明该类实现了`Compute`远程接口，因此可以用作远程对象。

`ComputeEngine`类定义了一个远程对象实现类，该类实现了一个单独的远程接口，没有其他接口。`ComputeEngine`类还包含两个仅可在本地调用的可执行程序元素。其中一个元素是用于`ComputeEngine`实例的构造函数。另一个元素是用于创建`ComputeEngine`实例并使其对客户端可用的`main`方法。

## 为远程对象定义构造函数。

`ComputeEngine`类有一个不带参数的构造函数。构造函数的代码如下：

```java
public ComputeEngine() {
    super();
}

```

此构造函数只调用了超类构造函数，即`Object`类的无参数构造函数。尽管即使在`ComputeEngine`构造函数中省略了超类构造函数，超类构造函数也会被调用，但为了清晰起见，还是包含在内。

## 为每个远程方法提供实现。

远程对象的类为远程接口中指定的每个远程方法提供实现。`Compute`接口包含一个单独的远程方法，`executeTask`，其实现如下：

```java
public <T> T executeTask(Task<T> t) {
    return t.execute();
}

```

此方法实现了`ComputeEngine`远程对象与其客户端之间的协议。每个客户端都向`ComputeEngine`提供一个具有`Task`接口的特定实现的`Task`对象的`execute`方法。`ComputeEngine`执行每个客户端的任务，并将任务的`execute`方法的结果直接返回给客户端。

## 在 RMI 中传递对象

远程方法的参数或返回值几乎可以是任何类型，包括本地对象、远程对象和基本数据类型。更准确地说，只要实体是基本数据类型、远程对象或*可序列化*对象的实例，就可以将任何类型的实体传递给远程方法或从远程方法传递出来，这意味着它实现了接口`java.io.Serializable`。

一些对象类型不符合这些标准，因此无法传递给远程方法或从远程方法返回。其中大多数对象，如线程或文件描述符，封装的信息只在单个地址空间内有意义。许多核心类，包括`java.lang`和`java.util`包中的类，实现了`Serializable`接口。

关于如何传递参数和返回值的规则如下：

+   远程对象本质上是通过引用传递的。*远程对象引用*是一个存根，是一个客户端代理，实现了远程对象实现的完整远程接口集。

+   本地对象通过对象序列化进行复制传递。默认情况下，除了标记为`static`或`transient`的字段外，所有字段都会被复制。可以按类覆盖默认序列化行为。

通过引用传递远程对象意味着通过远程方法调用对对象状态所做的任何更改都会反映在原始远程对象中。当传递远程对象时，只有接收者可用的是远程接口。在实现类中定义的方法或类实现的非远程接口中定义的任何方法对接收者不可用。

例如，如果您要传递对`ComputeEngine`类实例的引用，则接收方只能访问计算引擎的`executeTask`方法。该接收方将看不到`ComputeEngine`构造函数、其`main`方法或其对`java.lang.Object`的任何方法的实现。

在远程方法调用的参数和返回值中，不是远程对象的对象是按值传递的。因此，在接收方的 Java 虚拟机中创建对象的副本。接收方对对象状态的任何更改仅反映在接收方的副本中，而不是发送方的原始实例中。发送方对对象状态的任何更改仅反映在发送方的原始实例中，而不是接收方的副本中。

## 实现服务器的`main`方法

`ComputeEngine` 实现中最复杂的方法是 `main` 方法。`main` 方法用于启动 `ComputeEngine`，因此需要进行必要的初始化和管理工作，以准备服务器接受来自客户端的调用。这个方法不是一个远程方法，这意味着它不能从不同的 Java 虚拟机中调用。由于 `main` 方法声明为 `static`，该方法根本不与对象关联，而是与类 `ComputeEngine` 关联。

## 创建和安装安全管理器

`main` 方法的第一个任务是创建和安装安全管理器，以保护来自 Java 虚拟机内运行的不受信任的下载代码对系统资源的访问。安全管理器确定下载的代码是否可以访问本地文件系统或执行任何其他特权操作。

如果一个 RMI 程序没有安装安全管理器，RMI 将不会为作为参数接收的对象或远程方法调用的返回值下载类（除了从本地类路径）。这个限制确保下载代码执行的操作受安全策略约束。

这是创建和安装安全管理器的代码：

```java
if (System.getSecurityManager() == null) {
    System.setSecurityManager(new SecurityManager());
}

```

## 使远程对象对客户端可用

接下来，`main` 方法创建了一个 `ComputeEngine` 实例，并使用以下语句将其导出到 RMI 运行时：

```java
Compute engine = new ComputeEngine();
Compute stub =
    (Compute) UnicastRemoteObject.exportObject(engine, 0);

```

静态的 `UnicastRemoteObject.exportObject` 方法导出提供的远程对象，以便它可以接收来自远程客户端的远程方法调用。第二个参数是一个 `int`，指定用于监听对象的传入远程调用请求的 TCP 端口。通常使用值零，指定使用匿名端口。实际端口将由 RMI 或底层操作系统在运行时选择。但也可以使用非零值指定用于监听的特定端口。一旦 `exportObject` 调用成功返回，`ComputeEngine` 远程对象就准备好处理传入的远程调用。

`exportObject` 方法返回导出的远程对象的存根。请注意，变量 `stub` 的类型必须是 `Compute`，而不是 `ComputeEngine`，因为远程对象的存根只实现导出的远程对象实现的远程接口。

`exportObject` 方法声明可以抛出 `RemoteException`，这是一个已检查的异常类型。`main` 方法使用其 `try`/`catch` 块处理此异常。如果不以这种方式处理异常，则必须在 `main` 方法的 `throws` 子句中声明 `RemoteException`。如果请求的端口已绑定到其他用途，尝试导出远程对象可能会抛出 `RemoteException`，例如，如果必要的通信资源不可用。

在客户端可以调用远程对象的方法之前，必须首先获取对远程对象的引用。获取引用可以通过与程序中获取任何其他对象引用的方式相同完成，例如通过将引用作为方法的返回值的一部分或作为包含这样一个引用的数据结构的一部分。

系统提供了一种特定类型的远程对象，即 RMI 注册表，用于查找其他远程对象的引用。RMI 注册表是一个简单的远程对象命名服务，使客户端能够通过名称获取对远程对象的引用。注册表通常仅用于定位 RMI 客户端需要使用的第一个远程对象。然后，该第一个远程对象可能提供支持以查找其他对象。

`java.rmi.registry.Registry`远程接口是在注册表中绑定（或注册）和查找远程对象的 API。`java.rmi.registry.LocateRegistry`类提供了用于在特定网络地址（主机和端口）合成远程引用到注册表的静态方法。这些方法创建包含指定网络地址的远程引用对象，而不执行任何远程通信。`LocateRegistry`还提供了用于在当前 Java 虚拟机中创建新注册表的静态方法，尽管此示例未使用这些方法。一旦远程对象在本地主机上的 RMI 注册表中注册，任何主机上的客户端都可以按名称查找远程对象，获取其引用，然后调用对象上的远程方法。注册表可以被所有运行在主机上的服务器共享，或者单个服务器进程可以创建和使用自己的注册表。

`ComputeEngine`类使用以下语句为对象创建名称：

```java
String name = "Compute";

```

代码然后将名称添加到运行在服务器上的 RMI 注册表中。此步骤稍后通过以下语句完成：

```java
Registry registry = LocateRegistry.getRegistry();
registry.rebind(name, stub);

```

此`rebind`调用会对本地主机上的 RMI 注册表进行远程调用。与任何远程调用一样，此调用可能导致抛出`RemoteException`，该异常由`main`方法末尾的`catch`块处理。

注意`Registry.rebind`调用的以下内容：

+   `LocateRegistry.getRegistry`的无参数重载在本地主机上和默认注册表端口 1099 上合成对注册表的引用。如果注册表在除 1099 之外的端口上创建，则必须使用具有`int`参数的重载。

+   当对注册表进行远程调用时，传递的是远程对象的存根而不是远程对象本身的副本。远程实现对象，例如`ComputeEngine`的实例，永远不会离开它们被创建的 Java 虚拟机。因此，当客户端在服务器的远程对象注册表中执行查找时，会返回存根的副本。在这种情况下，远程对象实际上是通过（远程）引用而不是通过值传递的。

+   出于安全原因，应用程序只能在运行在同一主机上的注册表上`bind`、`unbind`或`rebind`远程对象引用。这种限制防止远程客户端删除或覆盖服务器注册表中的任何条目。然而，可以从任何主机（本地或远程）请求`lookup`。

一旦服务器向本地 RMI 注册表注册，它会打印一条消息，指示它已准备好开始处理调用。然后，`main`方法完成。不需要有一个线程等待来保持服务器处于活动状态。只要在另一个 Java 虚拟机中有对`ComputeEngine`对象的引用，无论是本地还是远程，`ComputeEngine`对象都不会被关闭或垃圾回收。因为程序在注册表中绑定了对`ComputeEngine`的引用，所以它可以从远程客户端，即注册表本身，访问。RMI 系统保持`ComputeEngine`的进程运行。`ComputeEngine`可用于接受调用，并且在其绑定从注册表中移除*且*没有远程客户端持有对`ComputeEngine`对象的远程引用时才会被回收。

在`ComputeEngine.main`方法中的最后一段代码处理可能出现的任何异常。代码中可能抛出的唯一已检查异常类型是`RemoteException`，可能是由`UnicastRemoteObject.exportObject`调用或注册表`rebind`调用引起的。在任何情况下，程序在打印错误消息后不能做更多事情，只能退出。在一些分布式应用中，可以从远程调用失败中恢复。例如，应用程序可以尝试重试操作或选择另一个服务器继续操作。
