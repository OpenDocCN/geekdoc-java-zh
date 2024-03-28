# MIDI 包概述

> 原文：[`docs.oracle.com/javase/tutorial/sound/overview-MIDI.html`](https://docs.oracle.com/javase/tutorial/sound/overview-MIDI.html)

介绍简要介绍了 Java Sound API 的 MIDI 功能。接下来的讨论更详细地介绍了通过`javax.sound.midi`包访问的 Java Sound API 的 MIDI 架构。解释了一些 MIDI 本身的基本特性，作为一个复习或介绍，以便将 Java Sound API 的 MIDI 功能放入上下文中。然后继续讨论 Java Sound API 对 MIDI 的处理，为后续部分中解释的编程任务做准备。对 MIDI API 的讨论分为两个主要领域：数据和设备。

## MIDI 复习：线缆和文件

音乐器数字接口（MIDI）标准定义了一种用于电子音乐设备（如电子键盘乐器和个人计算机）的通信协议。MIDI 数据可以在现场表演期间通过特殊电缆传输，并且还可以存储在标准类型的文件中以供以后播放或编辑。

本节回顾了一些 MIDI 基础知识，不涉及 Java Sound API。这个讨论旨在为熟悉 MIDI 的读者提供一个复习，为不熟悉的读者提供一个简要介绍，为随后讨论 Java Sound API 的 MIDI 包提供背景。如果您对 MIDI 有深入了解，可以安全地跳过本节。在编写大量 MIDI 应用程序之前，不熟悉 MIDI 的程序员可能需要比本教程中包含的更详细的 MIDI 描述。请参阅仅在硬拷贝中提供的完整 MIDI 1.0 详细规范，可从[`www.midi.org`](http://www.midi.org)获取（尽管您可能会在网上找到改写或摘要版本）。

MIDI 既是硬件规范，也是软件规范。要理解 MIDI 的设计，了解其历史是有帮助的。MIDI 最初是为了在电子键盘乐器（如合成器）之间传递音乐事件（如按键）而设计的。被称为音序器的硬件设备存储了可以控制合成器的音符序列，允许音乐表演被录制并随后播放。后来，开发了连接 MIDI 乐器与计算机串口的硬件接口，允许音序器在软件中实现。最近，计算机声卡已经整合了用于 MIDI I/O 和合成音乐声音的硬件。如今，许多 MIDI 用户只使用声卡，从不连接外部 MIDI 设备。CPU 已经足够快，以至于合成器也可以在软件中实现。只有在音频 I/O 和在某些应用中与外部 MIDI 设备通信时才需要声卡。

MIDI 规范的简要硬件部分规定了 MIDI 电缆的引脚排列方式以及这些电缆插入的插孔。这部分内容不需要我们关心。因为最初需要硬件的设备，如音序器和合成器，现在可以在软件中实现，也许大多数程序员需要了解 MIDI 硬件设备的唯一原因只是为了理解 MIDI 中的隐喻。然而，外部 MIDI 硬件设备对于一些重要的音乐应用仍然至关重要，因此 Java Sound API 支持 MIDI 数据的输入和输出。

MIDI 规范的软件部分非常广泛。这部分内容涉及 MIDI 数据的结构以及合成器等设备应如何响应该数据。重要的是要理解 MIDI 数据可以*流式传输*或*序列化*。这种二元性反映了 Complete MIDI 1.0 详细规范的两个不同部分：

+   MIDI 1.0

+   标准 MIDI 文件

通过检查 MIDI 规范的这两个部分的目的，我们将解释流式传输和序列化的含义。

## MIDI 线协议中的流数据

MIDI 规范的这两个部分中的第一个描述了非正式称为“MIDI 线协议”的内容。 MIDI 线协议，即原始 MIDI 协议，基于这样一个假设，即 MIDI 数据正在通过 MIDI 电缆（“线”）发送。电缆将数字数据从一个 MIDI 设备传输到另一个 MIDI 设备。每个 MIDI 设备可能是乐器或类似设备，也可能是配备有 MIDI 功能的声卡或 MIDI 到串口接口的通用计算机。

MIDI 数据，根据 MIDI 线协议定义，被组织成消息。不同类型的消息由消息中的第一个字节区分，称为*状态字节*。（状态字节是唯一一个最高位设置为 1 的字节。）在消息中跟随状态字节的字节称为*数据字节*。某些 MIDI 消息，称为*通道*消息，具有包含四位用于指定通道消息类型和另外四位用于指定通道号的状态字节。因此有 16 个 MIDI 通道；接收 MIDI 消息的设备可以设置为响应所有或仅一个这些虚拟通道上的通道消息。通常，每个 MIDI 通道（不应与音频通道混淆）用于发送不同乐器的音符。例如，两个常见的通道消息是 Note On 和 Note Off，分别启动音符发声并停止它。这两个消息各自需要两个数据字节：第一个指定音符的音高，第二个指定其“速度”（假设键盘乐器正在演奏音符时按下或释放键的速度）。

MIDI 传输协议为 MIDI 数据定义了一个流模型。该协议的一个核心特点是 MIDI 数据的字节是实时传递的，换句话说，它们是流式传输的。数据本身不包含时间信息；每个事件在接收时被处理，并假定它在正确的时间到达。如果音符是由现场音乐家生成的，那么这种模型是可以接受的，但如果您想要存储音符以便以后播放，或者想要在实时之外进行组合，那么这是不够的。当您意识到 MIDI 最初是为音乐表演而设计的，作为键盘音乐家在许多音乐家使用计算机之前控制多个合成器的一种方式时，这种限制是可以理解的。（规范的第一个版本于 1984 年发布。）

## 标准 MIDI 文件中的序列化数据

MIDI 规范的标准 MIDI 文件部分解决了 MIDI 传输协议中的时间限制。标准 MIDI 文件是一个包含 MIDI *事件* 的数字文件。一个事件简单地是一个 MIDI 消息，如 MIDI 传输协议中定义的，但附加了一个指定事件时间的额外信息。（还有一些事件不对应 MIDI 传输协议消息，我们将在下一节中看到。）额外的时间信息是一系列字节，指示何时执行消息描述的操作。换句话说，标准 MIDI 文件不仅指定要播放哪些音符，而且确切地指定何时播放每一个音符。这有点像一个乐谱。

标准 MIDI 文件中的信息被称为*序列*。标准 MIDI 文件包含一个或多个*轨道*。每个轨道通常包含一个乐器会演奏的音符，如果音乐由现场音乐家演奏。一个序列器是一个可以读取序列并在正确时间传递其中包含的 MIDI 消息的软件或硬件设备。一个序列器有点像一个管弦乐队指挥：它拥有所有音符的信息，包括它们的时间，然后告诉其他实体何时演奏这些音符。

## Java Sound API 对 MIDI 数据的表示

现在我们已经勾勒出 MIDI 规范对流式和序列化音乐数据的处理方式，让我们来看看 Java Sound API 如何表示这些数据。

### MIDI 消息

[`MidiMessage`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiMessage.html) 是表示“原始” MIDI 消息的抽象类。一个“原始” MIDI 消息通常是由 MIDI 传输协议定义的消息。它也可以是标准 MIDI 文件规范中定义的事件之一，但没有事件的时间信息。在 Java Sound API 中，有三类原始 MIDI 消息，分别由这三个相应的 `MidiMessage` 子类表示：

+   [`ShortMessages`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/ShortMessage.html)是最常见的消息，状态字节后最多有两个数据字节。通道消息，如 Note On 和 Note Off，都是短消息，还有一些其他消息也是短消息。

+   [`SysexMessages`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/SysexMessage.html)包含*系统专用*的 MIDI 消息。它们可能有许多字节，并且通常包含制造商特定的指令。

+   [`MetaMessages`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MetaMessage.html)出现在 MIDI 文件中，但不出现在 MIDI 线协议中。元消息包含数据，例如歌词或速度设置，这对于音序器可能很有用，但通常对合成器没有意义。

### MIDI 事件

正如我们所见，标准 MIDI 文件包含用于包装“原始”MIDI 消息以及时间信息的事件。Java Sound API 的[`MidiEvent`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiEvent.html)类的实例代表了一个类似于标准 MIDI 文件中存储的事件。

`MidiEvent`的 API 包括设置和获取事件的时间值的方法。还有一个方法用于检索其嵌入的原始 MIDI 消息，这是`MidiMessage`子类的实例，接下来会讨论。（嵌入的原始 MIDI 消息只能在构造`MidiEvent`时设置。）

### 序列和轨道

如前所述，标准 MIDI 文件存储被安排到轨道中的事件。通常文件代表一个音乐作品，通常每个轨道代表一个部分，例如可能由单个乐器演奏。乐器演奏的每个音符至少由两个事件表示：开始音符的 Note On 和结束音符的 Note Off。轨道还可能包含不对应音符的事件，例如元事件（如上所述）。

Java Sound API 将 MIDI 数据组织成三部分层次结构：

+   [`Sequence`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Sequence.html)

+   [`Track`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Track.html)

+   [`MidiEvent`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiEvent.html)

`Track`是`MidiEvents`的集合，`Sequence`是`Tracks`的集合。这种层次结构反映了标准 MIDI 文件规范中的文件、轨道和事件。（注意：这是一种包含和拥有的层次结构；这不是一种继承的类层次结构。这三个类直接继承自`java.lang.Object`。）

`Sequences`可以从 MIDI 文件中读取，也可以从头开始创建并通过向`Sequence`添加`Tracks`（或删除它们）进行编辑。同样，`MidiEvents`可以添加到序列中的轨道中，也可以从中删除。

## Java Sound API 对 MIDI 设备的表示

前一节解释了 MIDI 消息在 Java Sound API 中的表示方式。然而，MIDI 消息并不是独立存在的。它们通常从一个设备发送到另一个设备。使用 Java Sound API 的程序可以从头开始生成 MIDI 消息，但更常见的情况是这些消息是由软件设备（如序列器）创建的，或者通过 MIDI 输入端口从计算机外部接收。这样的设备通常会将这些消息发送到另一个设备，比如合成器或 MIDI 输出端口。

### MidiDevice 接口

在外部 MIDI 硬件设备的世界中，许多设备可以将 MIDI 消息传输到其他设备，并从其他设备接收消息。同样，在 Java Sound API 中，实现[`MidiDevice`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiDevice.html)接口的软件对象可以传输和接收消息。这样的对象可以纯粹在软件中实现，也可以作为硬件的接口，比如声卡的 MIDI 功能。基本的`MidiDevice`接口提供了 MIDI 输入或输出端口通常所需的所有功能。然而，合成器和序列器进一步实现了`MidiDevice`的子接口之一：[`Synthesizer`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Synthesizer.html)或[`Sequencer`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Sequencer.html)。

`MidiDevice`接口包括用于打开和关闭设备的 API。它还包括一个名为`MidiDevice.Info`的内部类，提供设备的文本描述，包括其名称、供应商和版本。如果您已经阅读了本教程的采样音频部分，那么这个 API 可能会听起来很熟悉，因为其设计类似于`javax.sampled.Mixer`接口，代表音频设备，并且具有类似的内部类`Mixer.Info`。

### 发送器和接收器

大多数 MIDI 设备都能够发送`MidiMessages`、接收它们，或两者兼而有之。设备发送数据的方式是通过它“拥有”的一个或多个发送器对象。同样，设备接收数据的方式是通过一个或多个接收器对象。发送器对象实现了[`Transmitter`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Transmitter.html)接口，而接收器实现了[`Receiver`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Receiver.html)接口。

每个发送器一次只能连接到一个接收器，反之亦然。一个设备如果同时向多个其他设备发送其 MIDI 消息，则通过拥有多个发送器，每个发送器连接到不同设备的接收器来实现。同样，一个设备如果要同时从多个来源接收 MIDI 消息，则必须通过多个接收器来实现。

### 序列器

一个序列器是一种捕获和播放 MIDI 事件序列的设备。它具有发射器，因为它通常将存储在序列中的 MIDI 消息发送到另一个设备，例如合成器或 MIDI 输出端口。它还具有接收器，因为它可以捕获 MIDI 消息并将其存储在序列中。在其超接口`MidiDevice`中，`Sequencer`添加了用于基本 MIDI 序列操作的方法。序列器可以从 MIDI 文件加载序列，查询和设置序列的速度，并将其他设备与其同步。应用程序可以注册一个对象，以便在序列器处理某些类型的事件时收到通知。

### 合成器

`Synthesizer`是一种产生声音的设备。它是`javax.sound.midi`包中唯一产生音频数据的对象。合成器设备控制一组 MIDI 通道对象 - 通常是 16 个，因为 MIDI 规范要求有 16 个 MIDI 通道。这些 MIDI 通道对象是实现[`MidiChannel`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiChannel.html)接口的类的实例，其方法代表 MIDI 规范的“通道音频消息”和“通道模式消息”。

应用程序可以通过直接调用合成器的 MIDI 通道对象的方法来生成声音。然而，更常见的情况是，合成器响应发送到其一个或多个接收器的消息而生成声音。例如，这些消息可能是由序列器或 MIDI 输入端口发送的。合成器解析其接收器接收到的每条消息，并通常根据事件中指定的 MIDI 通道号将相应的命令（如`noteOn`或`controlChange`）发送到其一个`MidiChannel`对象。

`MidiChannel`使用这些消息中的音符信息来合成音乐。例如，`noteOn`消息指定了音符的音高和“速度”（音量）。然而，音符信息是不够的；合成器还需要关于如何为每个音符创建音频信号的精确指令。这些指令由一个[`Instrument`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Instrument.html)表示。每个`Instrument`通常模拟不同的真实乐器或音效。`Instruments`可能作为合成器的预设提供，也可能从声音库文件中加载。在合成器中，`Instruments`按照银行号（可以将其视为行）和程序号（列）进行排列。

本节为理解 MIDI 数据提供了背景，并介绍了与 Java Sound API 中的 MIDI 相关的一些重要接口和类。后续章节将展示如何在应用程序中访问和使用这些对象。
