# 访问音频系统资源

> 原文：[`docs.oracle.com/javase/tutorial/sound/accessing.html`](https://docs.oracle.com/javase/tutorial/sound/accessing.html)

Java Sound API 对系统配置采取了灵活的方法。计算机上可以安装不同类型的音频设备（混音器）。该 API 对已安装的设备及其功能能力几乎不做任何假设。相反，它提供了系统报告可用音频组件的方法，以及您的程序访问它们的方法。

以下部分展示了您的程序如何了解计算机上已安装的采样音频资源以及如何访问可用资源。资源包括混音器和混音器拥有的各种类型的线路等。

## `AudioSystem` 类

[`AudioSystem`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/AudioSystem.html) 类充当音频组件的集散地，包括来自第三方提供商的内置服务和单独安装的服务。 `AudioSystem` 作为应用程序访问这些已安装的采样音频资源的入口点。您可以查询 `AudioSystem` 以了解已安装了哪些资源，然后可以访问这些资源。例如，应用程序可能首先询问 `AudioSystem` 类是否有具有特定配置的混音器，例如在前面讨论线路时所示的输入或输出配置之一。然后，程序将从混音器获取数据线路，依此类推。

以下是应用程序可以从 `AudioSystem` 获取的一些资源：

+   混音器 — 系统通常安装了多个混音器。通常至少有一个用于音频输入和一个用于音频输出。还可能有一些混音器没有 I/O 端口，而是接受应用程序的音频并将混合后的音频传递回程序。 `AudioSystem` 类提供了所有已安装混音器的列表。

+   线路 — 即使每条线路都与混音器相关联，应用程序也可以直接从 `AudioSystem` 获取线路，而无需明确处理混音器。

+   格式转换 — 应用程序可以使用格式转换将音频数据从一种格式转换为另一种格式。

+   文件和流 — `AudioSystem` 类提供了在音频文件和音频流之间进行转换的方法。它还可以报告声音文件的文件格式，并且可以以不同格式写入文件。

## 信息对象

Java Sound API 中的几个类提供有关相关接口的有用信息。例如，[`Mixer.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Mixer.Info.html)提供有关已安装混音器的详细信息，如混音器的供应商、名称、描述和版本。[`Line.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Line.Info.html)获取特定线路的类。`Line.Info`的子类包括[`Port.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Port.Info.html)和[`DataLine.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/DataLine.Info.html)，分别获取与特定端口和数据线相关的详细信息。这些类中的每一个在下面的适当部分中进一步描述。重要的是不要混淆`Info`对象与其描述的混音器或线路对象。

### 获取混音器

通常，使用 Java Sound API 的程序需要做的第一件事情之一是获取一个混音器，或者至少获取一个混音器的一条线路，以便将声音输入或输出计算机。您的程序可能需要特定类型的混音器，或者您可能希望显示所有可用混音器的列表，以便用户可以选择一个。在任何情况下，您需要了解安装了哪些类型的混音器。`AudioSystem`提供以下方法：

```java
static Mixer.Info[] getMixerInfo()

```

由此方法返回的每个[`Mixer.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Mixer.Info.html)对象标识安装的一种混音器类型。（通常系统最多只有一个给定类型的混音器。如果恰好有多个给定类型的混音器，则返回的数组仍然只有一个该类型的`Mixer.Info`。）应用程序可以遍历`Mixer.Info`对象，根据自身需求找到合适的混音器。`Mixer.Info`包括以下字符串来标识混音器的类型：

+   名称

+   版本

+   供应商

+   描述

这些都是任意字符串，因此需要特定混音器的应用程序必须知道可以期望什么以及将字符串与什么进行比较。提供混音器的公司应在其文档中包含此信息。或者，也许更典型的是，应用程序将向用户显示所有`Mixer.Info`对象的字符串，让用户选择相应的混音器。

一旦找到合适的混音器，应用程序调用以下`AudioSystem`方法来获取所需的混音器：

```java
static Mixer getMixer(Mixer.Info info)

```

如果您的程序需要具有某些功能的混音器，但不需要特定供应商制造的特定混音器怎么办？如果您不能依赖用户知道应选择哪个混音器怎么办？在这种情况下，`Mixer.Info`对象中的信息将没有太大用处。相反，您可以通过调用`getMixer`为每个`Mixer.Info`对象返回的所有`Mixer.Info`对象迭代，获取每个混音器，并查询每个混音器的功能。例如，您可能需要一个可以将其混合音频数据同时写入一定数量的目标数据线的混音器。在这种情况下，您将使用此`Mixer`方法查询每个混音器：

```java
int getMaxLines(Line.Info info)

```

在这里，[`Line.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Line.Info.html)将指定一个`TargetDataLine`。`Line.Info`类将在下一节中讨论。

### 获取所需类型的线

有两种方法可以获取一条线：

+   直接从[`AudioSystem`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/AudioSystem.html)对象

+   从您已从`AudioSystem`对象获取的混音器获取

### 直接从`AudioSystem`获取一条线

假设您尚未获得混音器，并且您的程序是一个真正只需要某种类型的线的简单程序；混音器的细节对您并不重要。您可以使用`AudioSystem`方法：

```java
static Line getLine(Line.Info info)

```

这类似于先前讨论的`getMixer`方法。与[`Mixer.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Mixer.Info.html)不同，作为参数使用的[`Line.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Line.Info.html)不存储文本信息以指定所需的线。相反，它存储有关所需线类的信息。

`Line.Info`是一个抽象类，因此您可以使用其子类([`Port.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Port.Info.html)或[`DataLine.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/DataLine.Info.html))来获取一条线。以下代码摘录使用`DataLine.Info`子类来获取和打开目标数据线：

```java
TargetDataLine line;
DataLine.Info info = new DataLine.Info(TargetDataLine.class, 
    format); // format is an AudioFormat object
if (!AudioSystem.isLineSupported(info)) {
    // Handle the error.
    }
    // Obtain and open the line.
try {
    line = (TargetDataLine) AudioSystem.getLine(info);
    line.open(format);
} catch (LineUnavailableException ex) {
        // Handle the error.
    //... 
}

```

此代码获取一个[`TargetDataLine`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/TargetDataLine.html)对象，除了其类和音频格式之外，没有指定任何属性。您可以使用类似的代码来获取其他类型的线。对于`SourceDataLine`或`Clip`，只需将该类替换为`TargetDataLine`作为线变量的类，并且在`DataLine.Info`构造函数的第一个参数中也进行替换。

对于`Port`，您可以在以下代码中使用`Port.Info`的静态实例：

```java
if (AudioSystem.isLineSupported(Port.Info.MICROPHONE)) {
    try {
        line = (Port) AudioSystem.getLine(
            Port.Info.MICROPHONE);
    }
}

```

请注意使用`isLineSupported`方法来查看混音器是否具有所需类型的线。

请记住，源线是混音器的输入，即，如果混音器代表音频输入设备，则是一个`Port`对象，如果混音器代表音频输出设备，则是一个`SourceDataLine`或`Clip`对象。同样，目标线是混音器的输出：对于音频输出混音器，是一个`Port`对象，对于音频输入混音器，是一个`TargetDataLine`对象。如果一个混音器根本没有连接到任何外部硬件设备怎么办？例如，考虑一个仅从应用程序获取音频并将混合音频传递回程序的内部或仅软件混音器。这种混音器的输入线有`SourceDataLine`或`Clip`对象，输出线有`TargetDataLine`对象。

您还可以使用以下`AudioSystem`方法了解任何已安装混音器支持的指定类型的源和目标线路：

```java
static Line.Info[] getSourceLineInfo(Line.Info info)
static Line.Info[] getTargetLineInfo(Line.Info info)

```

请注意，每个方法返回的数组表示唯一类型的线路，不一定是所有线路。例如，如果一个混音器的两条线路，或者两个不同混音器的两条线路，具有相同的`Line.Info`对象，则返回的数组中只会表示一个`Line.Info`。

### 从混音器获取线路

`Mixer`接口包括上述`AudioSystem`访问方法的变体，用于源和目标线路。这些`Mixer`方法包括接受`Line.Info`参数的方法，就像`AudioSystem`的方法一样。但是，`Mixer`还包括这些不带参数的变体：

```java
Line.Info[] getSourceLineInfo()
Line.Info[] getTargetLineInfo()

```

这些方法返回特定混音器的所有`Line.Info`对象的数组。一旦获得数组，您可以遍历它们，调用`Mixer`的`getLine`方法获取每条线路，然后调用`Line`的`open`方法为您的程序保留每条线路的使用权。

### 选择输入和输出端口

关于如何获取所需类型的线路的上一节也适用于端口以及其他类型的线路。您可以通过将`Port.Info`对象传递给接受`Line.Info`参数的`AudioSystem`（或`Mixer`）方法`getSourceLineInfo`和`getTargetLineInfo`来获取所有源（即输入）和目标（即输出）端口。然后，您遍历返回的对象数组并调用 Mixer 的`getLine`方法以获取每个端口。

然后，通过调用`Line`的`open`方法打开每个`Port`。打开端口意味着您打开它 - 也就是说，您允许声音进出端口。同样，您可以关闭您不希望声音通过的端口，因为有些端口可能在您获取它们之前已经打开。一些平台默认打开所有端口；或者用户或系统管理员可能已经选择使用另一个应用程序或操作系统软件打开或关闭某些端口。

**警告：** 如果您想选择特定端口并确保声音实际上进出该端口，可以按照描述打开端口。但是，这可能被视为对用户不友好的行为！例如，用户可能已关闭扬声器端口以免打扰同事。如果您的程序突然推翻了她的意愿并开始播放音乐，她会感到非常沮丧。另一个例子，用户可能希望确保计算机的麦克风在没有他知情的情况下永远不会打开，以避免窃听。一般来说，建议不要打开或关闭端口，除非您的程序响应用户通过用户界面表达的意图。相反，尊重用户或操作系统已选择的设置。

在连接到混音器之前，不需要打开或关闭端口才能使其正常工作。例如，即使所有输出端口关闭，您也可以开始将声音播放到音频输出混音器中。数据仍然流入混音器；播放不会被阻止。用户只是听不到任何声音。一旦用户打开输出端口，声音将通过该端口听到，从媒体播放已经到达的任何点开始。

此外，您不需要访问端口来了解混音器是否具有某些端口。例如，要了解混音器是否实际上是音频输出混音器，可以调用`getTargetLineInfo`来查看它是否具有输出端口。除非您想更改其设置（例如打开或关闭状态或它们可能具有的任何控件的设置），否则没有理由访问端口本身。

### 使用音频资源的权限

Java Sound API 包括一个[`AudioPermission`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/AudioPermission.html)类，指示 applet（或在安全管理器下运行的应用程序）对采样音频系统可以具有哪些访问权限。录制声音的权限是单独控制的。应谨慎授予此权限，以帮助防止未经授权的窃听等安全风险。默认情况下，applet 和应用程序被授予以下权限：

+   使用 applet 安全管理器运行的*applet*可以播放音频，但不能录制。

+   没有安全管理器运行的*应用程序*可以播放和录制音频。

+   使用默认安全管理器运行的应用程序可以播放音频，但不能录制。

一般来说，applet 在安全管理器的监督下运行，不允许录制声音。另一方面，应用程序不会自动安装安全管理器，并且可以录制声音。（但是，如果为应用程序显式调用默认安全管理器，则不允许应用程序录制声音。）

即使在安全管理器下运行，只要已被明确授予权限，applet 和应用程序都可以录制声音。

如果您的程序没有录制（或播放）声音的权限，在尝试打开线路时会抛出异常。在程序中，除了捕获异常并向用户报告问题外，您无法做任何事情，因为权限无法通过 API 更改。（如果可以的话，它们将毫无意义，因为没有任何东西是安全的！）通常，权限是在一个或多个策略配置文件中设置的，用户或系统管理员可以使用文本编辑器或策略工具程序进行编辑。
