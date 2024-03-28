# 传输和接收 MIDI 消息

> 原文：[`docs.oracle.com/javase/tutorial/sound/MIDI-messages.html`](https://docs.oracle.com/javase/tutorial/sound/MIDI-messages.html)

## 理解设备、发射器和接收器

Java Sound API 为 MIDI 数据指定了一种灵活且易于使用的消息路由架构，一旦理解其工作原理，就会变得灵活且易于使用。该系统基于模块连接设计：不同的模块，每个模块执行特定任务，可以相互连接（组网），使数据能够从一个模块流向另一个模块。

Java Sound API 的消息系统中的基本模块是[`MidiDevice`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiDevice.html)接口。`MidiDevices`包括序列器（记录、播放、加载和编辑时间戳 MIDI 消息序列）、合成器（触发 MIDI 消息时生成声音）以及 MIDI 输入和输出端口，通过这些端口数据来自外部 MIDI 设备并传输到外部 MIDI 设备。通常所需的 MIDI 端口功能由基本的`MidiDevice`接口描述。[`Sequencer`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Sequencer.html)和[`Synthesizer`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Synthesizer.html)接口扩展了`MidiDevice`接口，分别描述了 MIDI 序列器和合成器的附加功能。作为序列器或合成器的具体类应实现这些接口。

一个`MidiDevice`通常拥有一个或多个实现`Receiver`或`Transmitter`接口的辅助对象。这些接口代表连接设备的“插头”或“门户”，允许数据流入和流出。通过将一个`MidiDevice`的`Transmitter`连接到另一个`MidiDevice`的`Receiver`，可以创建一个模块网络，其中数据从一个模块流向另一个模块。

`MidiDevice`接口包括用于确定设备可以同时支持多少个发射器和接收器对象的方法，以及用于访问这些对象的其他方法。MIDI 输出端口通常至少有一个`Receiver`，通过该接收器可以接收传出消息；同样，合成器通常会响应发送到其`Receiver`或`Receivers`的消息。MIDI 输入端口通常至少有一个`Transmitter`，用于传播传入消息。功能齐全的序列器支持在录制过程中接收消息的`Receivers`和在播放过程中发送消息的`Transmitters`。

`Transmitter`接口包括用于设置和查询发射器发送其`MidiMessages`的接收器的方法。设置接收器建立了两者之间的连接。`Receiver`接口包含一个将`MidiMessage`发送到接收器的方法。通常，此方法由`Transmitter`调用。`Transmitter`和`Receiver`接口都包括一个`close`方法，用于释放先前连接的发射器或接收器，使其可用于不同的连接。

现在我们将讨论如何使用发射器和接收器。在涉及连接两个设备的典型情况之前（例如将一个音序器连接到合成器），我们将研究一个更简单的情况，即直接从应用程序向设备发送 MIDI 消息。研究这种简单的情况应该会更容易理解 Java Sound API 如何安排在两个设备之间发送 MIDI 消息。

## 发送消息到接收器而不使用发射器

假设你想从头开始创建一个 MIDI 消息，然后将其发送到某个接收器。你可以创建一个新的空白`ShortMessage`，然后使用以下`ShortMessage`方法填充它的 MIDI 数据：

```java
void setMessage(int command, int channel, int data1,
         int data2) 

```

一旦您准备好发送消息，您可以使用这个`Receiver`方法将其发送到一个`Receiver`对象：

```java
void send(MidiMessage message, long timeStamp)

```

时间戳参数将在稍后解释。现在，我们只会提到，如果您不关心指定精确时间，则其值可以设置为-1。在这种情况下，接收消息的设备将尽快响应消息。

应用程序可以通过调用设备的`getReceiver`方法来获取`MidiDevice`的接收器。如果设备无法向程序提供接收器（通常是因为设备的所有接收器已经在使用中），则会抛出`MidiUnavailableException`。否则，此方法返回的接收器可立即供程序使用。当程序使用完接收器后，应调用接收器的`close`方法。如果程序在调用`close`后尝试在接收器上调用方法，则可能会抛出`IllegalStateException`。

作为一个发送消息而不使用发射器的具体简单示例，让我们向默认接收器发送一个 Note On 消息，通常与设备（如 MIDI 输出端口或合成器）相关联。我们通过创建一个合适的`ShortMessage`并将其作为参数传递给`Receiver`的`send`方法来实现这一点：

```java
  ShortMessage myMsg = new ShortMessage();
  // Start playing the note Middle C (60), 
  // moderately loud (velocity = 93).
  myMsg.setMessage(ShortMessage.NOTE_ON, 0, 60, 93);
  long timeStamp = -1;
  Receiver       rcvr = MidiSystem.getReceiver();
  rcvr.send(myMsg, timeStamp);

```

此代码使用`ShortMessage`的静态整数字段，即`NOTE_ON`，用作 MIDI 消息的状态字节。 MIDI 消息的其他部分作为参数给出了`setMessage`方法的显式数值。零表示音符将使用 MIDI 通道号 1 播放；60 表示中央 C 音符；93 是一个任意的按键按下速度值，通常表示最终播放音符的合成器应该以相当大的音量播放它。（MIDI 规范将速度的确切解释留给合成器对其当前乐器的实现。）然后将此 MIDI 消息以时间戳 -1 发送到接收器。现在我们需要仔细研究时间戳参数的确切含义，这是下一节的主题。

## 理解时间戳

正如您已经知道的，MIDI 规范有不同的部分。一部分描述了 MIDI "线"协议（实时设备之间发送的消息），另一部分描述了标准 MIDI 文件（作为"序列"中事件存储的消息）。在规范的后半部分，存储在标准 MIDI 文件中的每个事件都标记有指示何时播放该事件的时间值。相比之下，MIDI 传输协议中的消息总是应立即处理，一旦被设备接收，因此它们没有附带的时间值。

Java Sound API 添加了一个额外的变化。毫不奇怪，`MidiEvent` 对象中存在时间值，这些对象存储在序列中（可能是从 MIDI 文件中读取的），就像标准 MIDI 文件规范中一样。但是在 Java Sound API 中，甚至在设备之间发送的消息——换句话说，对应 MIDI 传输协议的消息——也可以被赋予时间值，称为*时间戳*。我们关心的是这些时间戳。

### 发送到设备的消息上的时间戳

Java Sound API 中在设备之间发送的消息上可选附带的时间戳与标准 MIDI 文件中的时间值有很大不同。 MIDI 文件中的时间值通常基于音乐概念，如拍子和速度，每个事件的时间度量了自上一个事件以来经过的时间。相比之下，发送到设备的`Receiver`对象的消息上的时间戳始终以微秒为单位测量绝对时间。具体来说，它测量了自拥有接收器的设备打开以来经过的微秒数。

这种时间戳旨在帮助补偿操作系统或应用程序引入的延迟。重要的是要意识到，这些时间戳用于对时间进行微小调整，而不是实现可以在完全任意时间安排事件的复杂队列（就像`MidiEvent`的时间值那样）。

发送到设备（通过`Receiver`）的消息上的时间戳可以为设备提供精确的时间信息。设备在处理消息时可能会使用这些信息。例如，它可能通过几毫秒来调整事件的时间以匹配时间戳中的信息。另一方面，并非所有设备都支持时间戳，因此设备可能会完全忽略消息的时间戳。

即使设备支持时间戳，也可能不会按照您请求的时间安排事件。您不能期望发送一个时间戳在未来很远处的消息，并让设备按照您的意图处理它，当然也不能期望设备正确安排一个时间戳在过去的消息！设备决定如何处理时间戳偏离太远或在过去的消息。发送方不知道设备认为什么是太远，或者设备是否对时间戳有任何问题。这种无知模仿了外部 MIDI 硬件设备的行为，它们发送消息而从不知道是否被正确接收。（MIDI 线协议是单向的。）

一些设备通过`Transmitter`发送带有时间戳的消息。例如，MIDI 输入端口发送的消息可能会标记上消息到达端口的时间。在某些系统中，事件处理机制会导致在对消息进行后续处理过程中丢失一定量的时间精度。消息的时间戳允许保留原始的时间信息。

要了解设备是否支持时间戳，请调用`MidiDevice`的以下方法：

```java
    long getMicrosecondPosition()

```

如果设备忽略时间戳，则此方法返回-1。否则，它将返回设备当前的时间概念，您作为发送方可以在确定随后发送的消息的时间戳时使用它作为偏移量。例如，如果您想发送一个时间戳为未来五毫秒的消息，您可以获取设备当前的微秒位置，加上 5000 微秒，并将其用作时间戳。请记住，`MidiDevice`对时间的概念总是将时间零放在设备打开时的时间。

现在，有了时间戳的背景解释，让我们回到`Receiver`的`send`方法：

```java
void send(MidiMessage message, long timeStamp)

```

`timeStamp` 参数以微秒为单位表示，根据接收设备对时间的概念。如果设备不支持时间戳，它会简单地忽略 `timeStamp` 参数。您不需要为发送给接收器的消息加时间戳。您可以使用 `-1` 作为 `timeStamp` 参数，表示您不关心调整确切的时间；您只是让接收设备尽快处理消息。然而，不建议在发送给同一接收器的某些消息中使用 `-1`，而在其他消息中使用显式时间戳。这样做可能会导致结果时间的不规则性。

## 连接发射器到接收器

我们已经看到您可以直接向接收器发送 MIDI 消息，而不使用发射器。现在让我们看看更常见的情况，即您不是从头开始创建 MIDI 消息，而是简单地连接设备在一起，以便其中一个可以向另一个发送 MIDI 消息。

### 连接到单个设备

我们将以连接序列器到合成器作为我们的第一个示例。一旦建立了这种连接，启动序列器将导致合成器从序列器当前序列中的事件生成音频。现在，我们将忽略将序列从 MIDI 文件加载到序列器中的过程。此外，我们不会涉及播放序列的机制。加载和播放序列在播放、录制和编辑 MIDI 序列中有详细讨论。加载乐器到合成器在合成声音中有讨论。现在，我们只关心如何连接序列器和合成器。这将作为连接一个设备的发射器到另一个设备的接收器的更一般过程的示例。

为简单起见，我们将使用默认的序列器和默认的合成器。

```java
    Sequencer           seq;
    Transmitter         seqTrans;
    Synthesizer         synth;
    Receiver         synthRcvr;
    try {
          seq     = MidiSystem.getSequencer();
          seqTrans = seq.getTransmitter();
          synth   = MidiSystem.getSynthesizer();
          synthRcvr = synth.getReceiver(); 
          seqTrans.setReceiver(synthRcvr);      
    } catch (MidiUnavailableException e) {
          // handle or throw exception
    }

```

一个实现实际上可能有一个既是默认序列器又是默认合成器的单个对象。换句话说，实现可能使用一个同时实现 `Sequencer` 接口和 `Synthesizer` 接口的类。在这种情况下，可能不需要像上面的代码中所做的显式连接。但出于可移植性考虑，最好不要假设这样的配置。如果需要，当然可以测试这种情况：

```java
if (seq instanceof Synthesizer)

```

尽管上面的显式连接应该在任何情况下都能工作。

### 连接到多个设备

前面的代码示例说明了发射器和接收器之间的一对一连接。但是，如果您需要将相同的 MIDI 消息发送到多个接收器怎么办？例如，假设您希望从外部设备捕获 MIDI 数据以驱动内部合成器，同时将数据录制到序列中。这种连接形式有时被称为“分流”或“分配器”，很简单。以下语句显示了如何创建一个分流连接，通过该连接，到达 MIDI 输入端口的 MIDI 消息被发送到一个`Synthesizer`对象和一个`Sequencer`对象。我们假设您已经获取并打开了三个设备：输入端口、序列器和合成器。（要获取输入端口，您需要遍历`MidiSystem.getMidiDeviceInfo`返回的所有项目。）

```java
    Synthesizer  synth;
    Sequencer    seq;
    MidiDevice   inputPort;
    // [obtain and open the three devices...]
    Transmitter   inPortTrans1, inPortTrans2;
    Receiver            synthRcvr;
    Receiver            seqRcvr;
    try {
          inPortTrans1 = inputPort.getTransmitter();
          synthRcvr = synth.getReceiver(); 
          inPortTrans1.setReceiver(synthRcvr);
          inPortTrans2 = inputPort.getTransmitter();
          seqRcvr = seq.getReceiver(); 
          inPortTrans2.setReceiver(seqRcvr);
    } catch (MidiUnavailableException e) {
          // handle or throw exception
    }

```

此代码介绍了`MidiDevice.getTransmitter`方法的双重调用，将结果分配给`inPortTrans1`和`inPortTrans2`。如前所述，设备可以拥有多个发射器和接收器。每次为给定设备调用`MidiDevice.getTransmitter()`时，都会返回另一个发射器，直到没有更多可用为止，此时将抛出异常。

要了解设备支持多少个发射器和接收器，您可以使用以下`MidiDevice`方法：

```java
    int getMaxTransmitters()
    int getMaxReceivers()

```

这些方法返回设备拥有的总数，而不是当前可用的数量。

发射器一次只能向一个接收器传输 MIDI 消息。（每次调用`Transmitter`的`setReceiver`方法时，如果存在现有的`Receiver`，则会被新指定的替换。您可以通过调用`Transmitter.getReceiver`来判断发射器当前是否有接收器。）但是，如果设备有多个发射器，它可以同时向多个设备发送数据，通过将每个发射器连接到不同的接收器，就像我们在上面的输入端口的情况中看到的那样。

同样，设备可以使用其多个接收器同时从多个设备接收。所需的多接收器代码很简单，直接类似于上面的多发射器代码。一个单一接收器也可以同时从多个发射器接收消息。

### 关闭连接

一旦完成连接，您可以通过调用每个已获取的发射器和接收器的`close`方法来释放其资源。`Transmitter`和`Receiver`接口各自都有一个`close`方法。请注意，调用`Transmitter.setReceiver`不会关闭发射器当前的接收器。接收器保持打开状态，仍然可以接收来自任何连接到它的其他发射器的消息。

如果您也完成了设备的使用，可以通过调用`MidiDevice.close()`将它们提供给其他应用程序。关闭设备会自动关闭其所有发射器和接收器。
