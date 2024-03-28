# 访问 MIDI 系统资源

> 原文：[`docs.oracle.com/javase/tutorial/sound/accessing-MIDI.html`](https://docs.oracle.com/javase/tutorial/sound/accessing-MIDI.html)

Java Sound API 为 MIDI 系统配置提供了灵活的模型，就像为采样音频系统配置一样。Java Sound API 的实现本身可以提供不同类型的 MIDI 设备，并且服务提供者和用户可以提供并安装其他设备。您可以编写程序，使其对计算机上安装的具体 MIDI 设备做出少量假设。相反，程序可以利用 MIDI 系统的默认设置，或者允许用户从可用设备中选择。

本节展示了您的程序如何了解已安装的 MIDI 资源，以及如何访问所需的资源。在访问并打开设备之后，您可以将它们连接在一起，如后面的 传输和接收 MIDI 消息 中所讨论的。

## MidiSystem 类

Java Sound API 的 MIDI 包中的 [`MidiSystem`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiSystem.html) 类的作用与采样音频包中的 [`AudioSystem`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/AudioSystem.html) 类的作用直接类似。`MidiSystem` 充当访问已安装 MIDI 资源的中转站。

您可以查询`MidiSystem`以了解安装了什么类型的设备，然后可以遍历可用设备并访问所需设备。例如，一个应用程序可能首先询问`MidiSystem`有哪些合成器可用，然后显示一个列表，用户可以从中选择一个。一个更简单的应用程序可能只使用系统的默认合成器。

`MidiSystem` 类还提供了在 MIDI 文件和`Sequences`之间进行转换的方法。它可以报告 MIDI 文件的文件格式，并且可以写入不同类型的文件。

应用程序可以从`MidiSystem`获取以下资源：

+   顺序器

+   合成器

+   发射器（例如与 MIDI 输入端口相关联的发射器）

+   接收器（例如与 MIDI 输出端口相关联的接收器）

+   来自标准 MIDI 文件的数据

+   来自声音库文件的数据

本页重点介绍了这些类型资源中的前四种。其他类型将在本教程的后面讨论。

## 获取默认设备

使用 Java Sound API 的典型 MIDI 应用程序程序首先获取所需的设备，这些设备可以包括一个或多个顺序器、合成器、输入端口或输出端口。

有一个默认的合成器设备，一个默认的定序器设备，一个默认的传输设备和一个默认的接收设备。后两个设备通常代表系统上可用的 MIDI 输入和输出端口，如果有的话。（在这里很容易混淆方向性。将端口的传输或接收视为与软件相关，而不是与连接到物理端口的任何外部物理设备相关。MIDI 输入端口*传输*来自外部设备的数据到 Java Sound API 的`Receiver`，同样，MIDI 输出端口*接收*来自软件对象的数据并将数据中继到外部设备。）

一个简单的应用程序可能只使用默认设备而不探索所有安装的设备。`MidiSystem`类包括以下方法来检索默认资源：

```java
static Sequencer getSequencer()
static Synthesizer getSynthesizer()
static Receiver getReceiver()
static Transmitter getTransmitter()

```

前两种方法获取系统的默认排序和合成资源，这些资源可以代表物理设备或完全在软件中实现。`getReceiver`方法获取一个`Receiver`对象，该对象接收发送给它的 MIDI 消息并将其中继到默认接收设备。类似地，`getTransmitter`方法获取一个 Transmitter 对象，该对象可以代表默认传输设备向某个接收设备发送 MIDI 消息。

## 学习安装了哪些设备

与使用默认设备不同，更彻底的方法是从系统上安装的所有设备中选择所需的设备。应用程序可以通过编程方式选择所需的设备，或者可以显示可用设备列表，让用户选择要使用的设备。`MidiSystem`类提供了一个方法来了解安装了哪些设备，以及一个相应的方法来获取给定类型的设备。

以下是学习已安装设备的方法：

```java
 static MidiDevice.Info[] getMidiDeviceInfo()

```

如您所见，它返回一个信息对象数组。每个返回的`MidiDevice.Info`对象标识已安装的一种类型的定序器、合成器、端口或其他设备。（通常系统最多只有一个给定类型的实例。例如，来自某个供应商的特定型号的合成器只会安装一次。）`MidiDevice.Info`包括以下字符串来描述设备：

+   名称

+   版本号

+   厂商（创建设备的公司）

+   设备的描述

您可以在用户界面中显示这些字符串，让用户从设备列表中进行选择。

然而，要在程序中使用字符串来选择设备（而不是向用户显示字符串），你需要事先知道可能的字符串是什么。每个提供设备的公司应该在其文档中包含这些信息。需要或偏好特定设备的应用程序可以利用这些信息来定位该设备。这种方法的缺点是将程序限制在事先知道的设备实现上。

另一种更一般的方法是继续遍历`MidiDevice.Info`对象，获取每个相应的设备，并以编程方式确定是否适合使用（或至少适合包含在用户可以选择的列表中）。下一节将介绍如何执行此操作。

## 获取所需设备

一旦找到适当设备的信息对象，应用程序调用以下`MidiSystem`方法来获取相应的设备本身：

```java
static MidiDevice getMidiDevice(MidiDevice.Info info)

```

如果您已经找到描述所需设备的信息对象，可以使用此方法。但是，如果无法解释`getMidiDeviceInfo`返回的信息对象以确定所需设备，且不想向用户显示所有设备的信息，您可以尝试以下操作：遍历`getMidiDeviceInfo`返回的所有`MidiDevice.Info`对象，使用上述方法获取相应设备，并测试每个设备以查看其是否合适。换句话说，您可以在将设备包含在向用户显示的列表中之前，查询每个设备的类和功能，或者以编程方式决定设备而不涉及用户。例如，如果您的程序需要合成器，可以获取每个已安装的设备，查看哪些是实现`Synthesizer`接口的类的实例，然后将它们显示在用户可以选择的列表中，如下所示：

```java
// Obtain information about all the installed synthesizers.
Vector synthInfos;
MidiDevice device;
MidiDevice.Info[] infos = MidiSystem.getMidiDeviceInfo();
for (int i = 0; i < infos.length; i++) {
    try {
        device = MidiSystem.getMidiDevice(infos[i]);
    } catch (MidiUnavailableException e) {
          // Handle or throw exception...
    }
    if (device instanceof Synthesizer) {
        synthInfos.add(infos[i]);
    }
}
// Now, display strings from synthInfos list in GUI.    

```

作为另一个示例，您可以以编程方式选择设备，而不涉及用户。假设您想获取可以同时播放最多音符的合成器。您遍历所有`MidiDevice.Info`对象，如上所述，但在确定设备是合成器后，通过调用`Synthesizer`的`getMaxPolyphony`方法查询其功能。您保留具有最大音色的合成器，如下一节所述。即使您不要求用户选择合成器，您可能仍然显示所选`MidiDevice.Info`对象的字符串，仅供用户参考。

## 打开设备

前一节展示了如何获取已安装的设备。然而，设备可能已安装但不可用。例如，另一个应用程序可能独占使用它。要为您的程序实际保留设备，您需要使用`MidiDevice`方法`open`：

```java
if (!(device.isOpen())) {
    try {
      device.open();
  } catch (MidiUnavailableException e) {
          // Handle or throw exception...
  }
}

```

一旦您访问了设备并通过打开它来预留了它，您可能希望将其连接到一个或多个其他设备，以便让 MIDI 数据在它们之间流动。这个过程在传输和接收 MIDI 消息中有描述。

当完成对设备的操作后，通过调用`MidiDevice`的`close`方法释放它，以便其他程序可以使用。
