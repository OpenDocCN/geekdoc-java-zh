# 介绍 Sequencers

> 原文：[`docs.oracle.com/javase/tutorial/sound/MIDI-seq-intro.html`](https://docs.oracle.com/javase/tutorial/sound/MIDI-seq-intro.html)

在 MIDI 世界中，*sequencer*是任何能精确播放或记录一系列时间戳 MIDI 消息的硬件或软件设备。同样，在 Java Sound API 中，[`Sequencer`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Sequencer.html)抽象接口定义了可以播放和记录[`MidiEvent`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiEvent.html)对象序列的对象的属性。`Sequencer`通常从标准 MIDI 文件加载这些`MidiEvent`序列或将它们保存到这样的文件中。序列也可以进行编辑。以下页面解释了如何使用`Sequencer`对象以及相关的类和接口来完成这些任务。

要对`Sequencer`是什么有直观的理解，可以将其类比为磁带录音机，在许多方面`Sequencer`与磁带录音机相似。磁带录音机播放音频，而`Sequencer`播放 MIDI 数据。一个序列是多轨、线性、按时间顺序记录的 MIDI 音乐数据，`Sequencer`可以以不同速度播放，倒带，定位到特定点，录制或复制到文件进行存储。

传输和接收 MIDI 消息解释了设备通常具有`Receiver`对象、`Transmitter`对象或两者。为了*播放*音乐，设备通常通过`Receiver`接收`MidiMessages`，而`Receiver`通常是从属于`Sequencer`的`Transmitter`接收这些消息。拥有这个`Receiver`的设备可能是一个`Synthesizer`，它将直接生成音频，或者可能是一个 MIDI 输出端口，通过物理电缆将 MIDI 数据传输到外部设备。类似地，为了*录制*音乐，一系列带有时间戳的`MidiMessages`通常被发送到`Sequencer`拥有的`Receiver`中，然后将它们放入`Sequence`对象中。通常发送消息的对象是与硬件输入端口相关联的`Transmitter`，端口通过物理电缆中继从外部设备获取的 MIDI 数据。然而，负责发送消息的设备可能是其他`Sequencer`，或者任何具有`Transmitter`的设备。此外，如前所述，程序可以在完全不使用任何`Transmitter`的情况下发送消息。

一个`Sequencer`本身既有`Receivers`又有`Transmitters`。在录制时，它实际上通过其`Receivers`获取`MidiMessages`。在播放时，它使用其`Transmitters`发送存储在其记录的`Sequence`中的`MidiMessages`（或从文件中加载）。

在 Java Sound API 中，将`Sequencer`的角色视为`MidiMessages`的聚合器和“解聚器”的一种方式。一系列独立的`MidiMessages`被发送到`Sequencer`，每个`MidiMessages`都有自己的时间戳，标记了音乐事件的时间。这些`MidiMessages`被封装在`MidiEvent`对象中，并通过`Sequencer.record`方法在`Sequence`对象中收集。`Sequence`是一个包含`MidiEvents`聚合的数据结构，通常代表一系列音符，通常是整首歌曲或作品。在播放时，`Sequencer`再次从`Sequence`中的`MidiEvent`对象中提取`MidiMessages`，然后将它们传输到一个或多个设备，这些设备将把它们渲染成声音，保存它们，修改它们，或将它们传递给其他设备。

一些音序器可能既没有发射器也没有接收器。例如，它们可能会根据键盘或鼠标事件从头开始创建`MidiEvents`，而不是通过`Receivers`接收`MidiMessages`。同样，它们可能通过直接与内部合成器（实际上可能是与音序器相同的对象）通信来演奏音乐，而不是将`MidiMessages`发送到与另一个对象关联的`Receiver`。

## 何时使用音序器

应用程序可以直接向设备发送 MIDI 消息，而不使用音序器，就像在传输和接收 MIDI 消息中描述的那样。程序只需在想要发送消息时调用`Receiver.send`方法。这是一种直接的方法，当程序本身实时创建消息时非常有用。例如，考虑一个程序，让用户通过点击屏幕上的钢琴键盘来演奏音符。当程序接收到鼠标按下事件时，立即向合成器发送适当的 Note On 消息。

如前所述，程序可以在发送到设备的接收器的每个 MIDI 消息中包含时间戳。然而，这些时间戳仅用于微调时间，以纠正处理延迟。调用者通常不能设置任意时间戳；传递给`Receiver.send`的时间值必须接近当前时间，否则接收设备可能无法正确安排消息。这意味着，如果应用程序想要提前为整首音乐创建一个 MIDI 消息队列（而不是对实时事件做出响应时创建每个消息），它必须非常小心地安排每次调用`Receiver.send`几乎正确的时间。

幸运的是，大多数应用程序不必担心这种调度问题。程序可以使用`Sequencer`对象来管理 MIDI 消息队列，而不是自己调用`Receiver.send`。`Sequencer`负责调度和发送消息，换句话说，以正确的时间播放音乐。通常，在需要将非实时 MIDI 消息序列转换为实时序列（如播放）或反之（如录制）时，使用`Sequencer`是有利的。`Sequencer`最常用于播放来自 MIDI 文件的数据和从 MIDI 输入端口录制数据。

## 理解序列数据

在查看`Sequencer`API 之前，了解存储在序列中的数据类型是有帮助的。

### 序列和轨道

在 Java Sound API 中，`Sequencer`在组织记录的 MIDI 数据方面紧密遵循标准 MIDI 文件规范。如上所述，`Sequence`是按时间组织的`MidiEvents`的聚合。但`Sequence`比仅仅是线性`MidiEvents`序列更具结构：`Sequence`实际上包含全局时间信息以及一组`Tracks`，而`Tracks`本身保存`MidiEvent`数据。因此，由`Sequencer`播放的数据由三级对象层次结构组成：`Sequencer`，`Track`和`MidiEvent`。

在这些对象的常规使用中，`Sequence`代表完整的音乐作品或作品的一部分，每个`Track`对应于合奏中的一个声音或演奏者。在这个模型中，特定`Track`上的所有数据也因此被编码到为该声音或演奏者保留的特定 MIDI 通道中。

这种数据组织方式便于编辑序列，但请注意，这只是一种使用`Tracks`的常规方式。`Track`类的定义中没有任何限制它只能包含不同 MIDI 通道上的`MidiEvents`的内容。例如，整个多通道 MIDI 作品可以混合录制到一个`Track`上。此外，Type 0 标准 MIDI 文件（与 Type 1 和 Type 2 相对）根据定义只包含一个轨道；因此，从这样的文件中读取的`Sequence`必然只有一个`Track`对象。

### MidiEvents 和 Ticks

如 Java Sound API 概述中所讨论的，Java Sound API 包括与组成大多数标准 MIDI 消息的原始两个或三字节序列对应的`MidiMessage`对象。`MidiEvent`只是一个`MidiMessage`的打包，以及指定事件发生时间的伴随时间值。（我们可能会说序列实际上包含四或五级数据层次，而不是三级，因为表面上最低级别的`MidiEvent`实际上包含一个更低级别的`MidiMessage`，同样`MidiMessage`对象包含一个组成标准 MIDI 消息的字节数组。）

在 Java Sound API 中，`MidiMessages`可以与定时值关联的另一种方式有两种。 其中一种是上面提到的“何时使用 Sequencer”。 这种技术在不使用 Transmitter 向接收器发送消息和理解时间戳下有详细描述。 在那里，我们看到`Receiver`的`send`方法接受一个`MidiMessage`参数和一个时间戳参数。 那种时间戳只能用微秒表示。

`MidiMessage`可以指定其定时的另一种方式是通过封装在`MidiEvent`中。 在这种情况下，定时以稍微更抽象的单位称为*ticks*来表示。

一个 tick 的持续时间是多少？ 它可以在序列之间变化（但不会在序列内部变化），其值存储在标准 MIDI 文件的头部中。 一个 tick 的大小以两种类型的单位给出：

+   每四分音符的脉冲（ticks），缩写为 PPQ

+   每帧的 ticks，也称为 SMPTE 时间码（由电影和电视工程师协会采用的标准）

如果单位是 PPQ，一个 tick 的大小被表示为四分音符的一部分，这是一个相对的，而不是绝对的时间值。 四分音符是一个音乐持续时间值，通常对应于音乐中的一个节拍（4/4 拍子中的四分之一）。 四分音符的持续时间取决于速度，如果序列包含速度变化事件，则音乐中的四分音符的持续时间可能会在音乐过程中变化。 因此，如果序列的定时增量（ticks）发生，比如每四分音符发生 96 次，那么每个事件的定时值都以音乐术语来衡量该事件的位置，而不是绝对时间值。

另一方面，在 SMPTE 的情况下，单位度量绝对时间，而速度的概念不适用。 实际上有四种不同的 SMPTE 约定可用，它们指的是每秒的电影帧数。 每秒的帧数可以是 24、25、29.97 或 30。 使用 SMPTE 时间码，一个 tick 的大小被表示为帧的一部分。

在 Java Sound API 中，您可以调用`Sequence.getDivisionType`来了解特定序列中使用的单位类型，即 PPQ 或 SMPTE 单位之一。 然后在调用`Sequence.getResolution`之后可以计算一个 tick 的大小。 如果分割类型是 PPQ，则后一种方法返回每四分音符的 ticks 数，或者如果分割类型是 SMPTE 约定之一，则返回每个 SMPTE 帧的 ticks 数。 在 PPQ 的情况下，可以使用以下公式来获取一个 tick 的大小：

```java
ticksPerSecond =  
    resolution * (currentTempoInBeatsPerMinute / 60.0);
tickSize = 1.0 / ticksPerSecond;

```

以及在 SMPTE 的情况下的这个公式：

```java
framesPerSecond = 
  (divisionType == Sequence.SMPTE_24 ? 24
    : (divisionType == Sequence.SMPTE_25 ? 25
      : (divisionType == Sequence.SMPTE_30 ? 30
        : (divisionType == Sequence.SMPTE_30DROP ?

            29.97))));
ticksPerSecond = resolution * framesPerSecond;
tickSize = 1.0 / ticksPerSecond;

```

Java Sound API 中对序列中时间的定义与标准 MIDI 文件规范相似。然而，有一个重要的区别。`MidiEvents`中包含的 tick 值衡量的是*累积*时间，而不是*增量*时间。在标准 MIDI 文件中，每个事件的时间信息衡量的是自上一个事件开始以来经过的时间。这被称为增量时间。但在 Java Sound API 中，ticks 不是增量值；它们是前一个事件的时间值*加上*增量值。换句话说，在 Java Sound API 中，每个事件的时间值始终大于序列中前一个事件的时间值（或者相等，如果事件应该同时发生）。每个事件的时间值衡量的是自序列开始以来经过的时间。

总结一下，Java Sound API 以 MIDI ticks 或微秒表示时间信息。`MidiEvents`以 MIDI ticks 形式存储时间信息。一个 tick 的持续时间可以从`Sequence`的全局时间信息以及（如果序列使用基于速度的时间）当前的音乐速度计算出来。另一方面，发送给`Receiver`的`MidiMessage`的时间戳总是以微秒表示。

这种设计的一个目标是避免时间概念的冲突。`Sequencer`的工作是解释其`MidiEvents`中的时间单位，这些单位可能是 PPQ 单位，并将其转换为以微秒为单位的绝对时间，考虑到当前的速度。序列器还必须相对于接收消息的设备打开时的时间表达微秒。请注意，一个序列器可以有多个发射器，每个发射器将消息传递给一个可能与完全不同设备关联的不同接收器。因此，您可以看到，序列器必须能够同时执行多个转换，确保每个设备接收适合其时间概念的时间戳。

更复杂的是，不同设备可能基于不同来源（如操作系统的时钟或声卡维护的时钟）更新其时间概念。这意味着它们的时间可能相对于序列器的时间会有偏移。为了与序列器保持同步，一些设备允许自己成为序列器时间概念的“从属”。设置主从关系将在稍后的[`MidiEvent`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiEvent.html)中讨论。
