# 编译示例程序

> 原文：[`docs.oracle.com/javase/tutorial/rmi/compiling.html`](https://docs.oracle.com/javase/tutorial/rmi/compiling.html)

在一个真实的场景中，例如部署了像计算引擎这样的服务，开发人员可能会创建一个包含 `Compute` 和 `Task` 接口的 Java 存档（JAR）文件，供服务器类实现和客户端程序使用。接下来，一个开发人员，也许是接口 JAR 文件的相同开发人员，会编写 `Compute` 接口的实现，并将该服务部署在客户端可用的机器上。客户端程序的开发人员可以使用 JAR 文件中包含的 `Compute` 和 `Task` 接口，独立开发一个任务和客户端程序，使用 `Compute` 服务。

在本节中，您将学习如何设置 JAR 文件、服务器类和客户端类。您将看到客户端的 `Pi` 类将在运行时下载到服务器上。此外，`Compute` 和 `Task` 接口将在运行时从服务器下载到注册表中。

该示例将接口、远程对象实现和客户端代码分为三个包：

+   `compute` – `Compute` 和 `Task` 接口

+   `engine` – `ComputeEngine` 实现类

+   `client` – `ComputePi` 客户端代码和 `Pi` 任务实现

首先，您需要构建接口 JAR 文件，以提供给服务器和客户端开发人员。

## 构建接口类的 JAR 文件

首先，您需要编译 `compute` 包中的接口源文件，然后构建一个包含它们类文件的 JAR 文件。假设用户 `waldo` 已经编写了这些接口，并将源文件放在 Windows 的目录 `c:\home\waldo\src\compute` 或 Solaris OS 或 Linux 的目录 `/home/waldo/src/compute` 中。给定这些路径，您可以使用以下命令编译接口并创建 JAR 文件：

**微软 Windows**：

```java
cd c:\home\waldo\src
javac compute\Compute.java compute\Task.java
jar cvf compute.jar compute\*.class

```

**Solaris OS 或 Linux**：

```java
cd /home/waldo/src
javac compute/Compute.java compute/Task.java
jar cvf compute.jar compute/*.class

```

* * *

`jar` 命令由于 `-v` 选项显示如下输出：

```java
added manifest
adding: compute/Compute.class(in = 307) (out= 201)(deflated 34%)
adding: compute/Task.class(in = 217) (out= 149)(deflated 31%)

```

现在，您可以将 `compute.jar` 文件分发给服务器和客户端应用程序的开发人员，以便他们可以利用这些接口。

使用 `javac` 编译器构建服务器端或客户端类后，如果其中任何类需要由其他 Java 虚拟机动态下载，您必须确保它们的类文件放在网络可访问的位置。在本例中，对于 Solaris OS 或 Linux，这个位置是 `/home/*user*/public_html/classes`，因为许多 Web 服务器允许通过构造为 `http://host/~*user*/` 的 HTTP URL 访问用户的 `public_html` 目录。如果您的 Web 服务器不支持这种约定，您可以在 Web 服务器的层次结构中使用不同的位置，或者您可以使用文件 URL。在 Solaris OS 或 Linux 上，文件 URL 的形式为 `file:/home/*user*/public_html/classes/`，在 Windows 上的形式为 `file:/c:/home/*user*/public_html/classes/`。您也可以根据需要选择另一种类型的 URL。

类文件的网络可访问性使得 RMI 运行时在需要时可以下载代码。RMI 不会为代码下载定义自己的协议，而是使用 Java 平台支持的 URL 协议（例如 HTTP）来下载代码。请注意，使用完整的重量级 Web 服务器来提供这些类文件是不必要的。例如，可以在 [](http://www.oracle.com/webfolder/technetwork/java/core/basic/rmi/class-server.zip) 找到一个简单的 HTTP 服务器，提供了在 RMI 通过 HTTP 下载类所需的功能。

另请参阅 [远程方法调用主页](http://www.oracle.com/technetwork/java/javase/tech/index-jsp-136424.html)。

## 构建服务器类

`engine` 包仅包含一个服务器端实现类 `ComputeEngine`，这是远程接口 `Compute` 的实现。

假设开发 `ComputeEngine` 类的用户 `ann` 已经将 `ComputeEngine.java` 放在 Windows 的目录 `c:\home\ann\src\engine` 或 Solaris OS 或 Linux 的目录 `/home/ann/src/engine` 中。她正在部署供客户下载的类文件在她的 `public_html` 目录的子目录中，Windows 上为 `c:\home\ann\public_html\classes` 或 Solaris OS 或 Linux 上为 `/home/ann/public_html/classes`。这个位置可以通过一些 Web 服务器访问，如 `http://*host*:*port*/~ann/classes/`。

`ComputeEngine` 类依赖于 `Compute` 和 `Task` 接口，这些接口包含在 `compute.jar` JAR 文件中。因此，在构建服务器类时，您需要将 `compute.jar` 文件放在类路径中。假设 `compute.jar` 文件位于 Windows 的目录 `c:\home\ann\public_html\classes` 或 Solaris OS 或 Linux 的目录 `/home/ann/public_html/classes` 中。有了这些路径，您可以使用以下命令构建服务器类：

**Microsoft Windows**:

```java
cd c:\home\ann\src
javac -cp c:\home\ann\public_html\classes\compute.jar
    engine\ComputeEngine.java

```

**Solaris OS 或 Linux**:

```java
cd /home/ann/src
javac -cp /home/ann/public_html/classes/compute.jar
    engine/ComputeEngine.java

```

`ComputeEngine`的存根类实现了`Compute`接口，该接口引用了`Task`接口。因此，这两个接口的类定义需要对其他 Java 虚拟机（如注册表的 Java 虚拟机）可访问，以便存根能够接收。客户端 Java 虚拟机将已经在其类路径中包含了这些接口，因此实际上不需要下载它们的定义。`public_html`目录下的`compute.jar`文件可以起到这个作用。

现在，计算引擎已经准备好部署。你现在可以这样做，或者等到构建客户端之后再部署。

## 构建客户端类

`client`包含两个类，`ComputePi`，主要客户端程序，和`Pi`，客户端对`Task`接口的实现。

假设开发客户端类的用户`jones`已经将`ComputePi.java`和`Pi.java`放置在 Windows 系统的`c:\home\jones\src\client`目录下，或者 Solaris OS 或 Linux 系统的`/home/jones/src/client`目录下。他正在将计算引擎的类文件部署在他的`public_html`目录的子目录中，Windows 系统为`c:\home\jones\public_html\classes`，Solaris OS 或 Linux 系统为`/home/jones/public_html/classes`。这个位置可以通过一些 Web 服务器访问，如`http://*host*:*port*/~jones/classes/`。

客户端类依赖于`Compute`和`Task`接口，这些接口包含在`compute.jar` JAR 文件中。因此，在构建客户端类时，你需要将`compute.jar`文件放在类路径中。假设`compute.jar`文件位于 Windows 系统的`c:\home\jones\public_html\classes`目录下，或者 Solaris OS 或 Linux 系统的`/home/jones/public_html/classes`目录下。有了这些路径，你可以使用以下命令来构建客户端类：

**Microsoft Windows**：

```java
cd c:\home\jones\src
javac -cp c:\home\jones\public_html\classes\compute.jar
    client\ComputePi.java client\Pi.java
mkdir c:\home\jones\public_html\classes\client
cp client\Pi.class
    c:\home\jones\public_html\classes\client

```

**Solaris OS 或 Linux**：

```java
cd /home/jones/src
javac -cp /home/jones/public_html/classes/compute.jar
    client/ComputePi.java client/Pi.java
mkdir /home/jones/public_html/classes/client
cp client/Pi.class
    /home/jones/public_html/classes/client

```

只有`Pi`类需要放置在`public_html\classes\client`目录中，因为只有`Pi`类需要在计算引擎的 Java 虚拟机中可用以供下载。现在，你可以先运行服务器，然后再运行客户端。
