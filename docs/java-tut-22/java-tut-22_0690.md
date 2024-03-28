# 合成声音

> 原文：[`docs.oracle.com/javase/tutorial/sound/MIDI-synth.html`](https://docs.oracle.com/javase/tutorial/sound/MIDI-synth.html)

大多数使用 Java Sound API 的 MIDI 包的程序都是用来合成声音。之前讨论过的整个 MIDI 文件、事件、序列和序列器的装置几乎总是最终将音乐数据发送到合成器以转换为音频。（可能的例外包括将 MIDI 转换为音乐符号的程序，可以被音乐家阅读，以及向外部 MIDI 控制设备发送消息的程序，如混音台。）

因此，`Synthesizer`接口对于 MIDI 包至关重要。本页展示了如何操作合成器播放声音。许多程序将简单地使用序列器将 MIDI 文件数据发送到合成器，并且不需要直接调用许多`Synthesizer`方法。然而，也可以直接控制合成器，而不使用序列器甚至`MidiMessage`对象，如本页末尾所述。

对于不熟悉 MIDI 的读者来说，合成架构可能看起来很复杂。其 API 包括三个接口：

+   [`合成器`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Synthesizer.html)

+   [`Midi 通道`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiChannel.html)

+   [`声音库`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Soundbank.html)

和四个类：

+   [`乐器`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Instrument.html)

+   [`补丁`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Patch.html)

+   [`声音库资源`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/SoundbankResource.html)

+   [`VoiceStatus`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/VoiceStatus.html)

作为对所有这些 API 的定位，下一节解释了一些 MIDI 合成的基础知识以及它们如何在 Java Sound API 中反映。随后的部分将更详细地查看 API。

## 理解 MIDI 合成

合成器是如何产生声音的？根据其实现方式，它可能使用一种或多种声音合成技术。例如，许多合成器使用波表合成。波表合成器从内存中读取存储的音频片段，以不同的采样率播放它们，并循环播放它们以创建不同音高和持续时间的音符。例如，要合成萨克斯风演奏 C#4 音符（MIDI 音符号 61）的声音，合成器可能会访问从萨克斯风演奏中音符中央 C（MIDI 音符号 60）的录音中提取的一个非常短的片段，然后以比录制时略快的采样率不断循环播放这个片段，从而创建一个略高音高的长音符。其他合成器使用诸如频率调制（FM）、加法合成或物理建模等技术，这些技术不使用存储的音频，而是使用不同的算法从头开始生成音频。

### 乐器

所有合成技术共同之处在于能够创造许多种声音。不同的算法，或者同一算法内不同参数的设置，会产生不同的声音结果。一个*乐器*是合成某种类型声音的规范。该声音可能模拟传统乐器，如钢琴或小提琴；也可能模拟其他类型的声源，例如电话或直升机；或者根本不模拟任何“现实世界”的声音。一个名为通用 MIDI 的规范定义了一个标准的 128 种乐器列表，但大多数合成器也允许使用其他乐器。许多合成器提供一系列内置乐器，始终可供使用；一些合成器还支持加载额外乐器的机制。

一个乐器可能是特定供应商的——换句话说，仅适用于一个合成器或同一供应商的几个型号。当两个不同的合成器使用不同的声音合成技术，或者即使基本技术相同，但使用不同的内部算法和参数时，就会出现不兼容性。由于合成技术的细节通常是专有的，因此不兼容性是常见的。Java Sound API 包括检测给定合成器是否支持给定乐器的方法。

一个乐器通常可以被视为一个预设；你不必了解产生其声音的合成技术的细节。然而，你仍然可以改变其声音的各个方面。每个 Note On 消息指定一个单独音符的音高和音量。你还可以通过其他 MIDI 命令如控制器消息或系统专用消息来改变声音。

### 通道

许多合成器是*多音轨*（有时称为*多音色*），意味着它们可以同时演奏不同乐器的音符。 (*音色*是使听众能够区分一种乐器与其他乐器的特征音质。) 多音轨合成器可以模拟整个真实乐器的合奏，而不仅仅是一次一个乐器。 MIDI 合成器通常通过利用 MIDI 规范允许数据传输的不同 MIDI 通道来实现此功能。在这种情况下，合成器实际上是一组发声单元，每个单元模拟不同的乐器，并独立响应在不同 MIDI 通道上接收到的消息。由于 MIDI 规范仅提供 16 个通道，典型的 MIDI 合成器可以同时演奏多达 16 种不同的乐器。合成器接收一系列 MIDI 命令，其中许多是通道命令。 (通道命令针对特定的 MIDI 通道；有关更多信息，请参阅 MIDI 规范。) 如果合成器是多音轨的，它会根据命令中指示的通道号将每个通道命令路由到正确的发声单元。

在 Java Sound API 中，这些发声单元是实现`MidiChannel`接口的类的实例。一个`synthesizer`对象至少有一个`MidiChannel`对象。如果合成器是多音轨的，通常有多个，通常是 16 个。每个`MidiChannel`代表一个独立的发声单元。

因为合成器的`MidiChannel`对象更多或更少是独立的，将乐器分配给通道不必是唯一的。例如，所有 16 个通道都可以演奏钢琴音色，就好像有一个由 16 台钢琴组成的合奏团。任何分组都是可能的—例如，通道 1、5 和 8 可以演奏吉他声音，而通道 2 和 3 演奏打击乐，通道 12 有低音音色。在给定的 MIDI 通道上演奏的乐器可以动态更改；这被称为*程序更改*。

尽管大多数合成器一次只能激活 16 个或更少的乐器，但这些乐器通常可以从更大的选择中选择，并根据需要分配到特定的通道。

### 声音库和音色

在合成器中，乐器按照银行号和程序号进行层次化组织。银行和程序可以被视为乐器的二维表中的行和列。一个银行是一个程序的集合。 MIDI 规范允许一个银行中最多有 128 个程序，最多有 128 个银行。然而，特定的合成器可能仅支持一个银行，或几个银行，并且可能支持每个银行少于 128 个程序。

在 Java Sound API 中，层次结构中有一个更高级别的部分：声音库。声音库可以包含多达 128 个银行，每个银行包含多达 128 个乐器。一些合成器可以将整个声音库加载到内存中。

要从当前声音库中选择一个乐器，您需要指定一个银行号和一个程序号。MIDI 规范通过两个 MIDI 命令实现了这一点：银行选择和程序更改。在 Java Sound API 中，银行号和程序号的组合封装在一个`Patch`对象中。通过指定一个新的 patch，您可以更改 MIDI 通道的当前乐器。该 patch 可以被视为当前声音库中乐器的二维索引。

您可能想知道声音库是否也是按数字索引的。答案是否定的；MIDI 规范没有提供这一点。在 Java Sound API 中，可以通过读取声音库文件来获取`Soundbank`对象。如果合成器支持声音库，它的乐器可以根据需要单独加载到合成器中，或者一次性全部加载。许多合成器都有一个内置或默认的声音库；该声音库中包含的乐器始终对合成器可用。

### 声音

区分合成器可以同时播放的*音色*数量和*音符*数量是很重要的。前者在“通道”下面已经描述过。同时播放多个音符的能力被称为*复音*。即使一个合成器不是多音色的，通常也可以同时播放多个音符（所有音符具有相同的音色，但不同的音高）。例如，播放任何和弦，比如 G 大三和弦或 B 小七和弦，都需要复音。任何实时生成声音的合成器都有一个限制，即它可以同时合成的音符数量。在 Java Sound API 中，合成器通过`getMaxPolyphony`方法报告这个限制。

*声音*是一系列单音符，比如一个人可以唱的旋律。复音包括多个声音，比如合唱团唱的部分。例如，一个 32 声音的合成器可以同时播放 32 个音符。（然而，一些 MIDI 文献使用“声音”一词的含义不同，类似于“乐器”或“音色”的含义。）

将传入的 MIDI 音符分配给特定声音的过程称为*声音分配*。合成器维护一个声音列表，跟踪哪些声音是活动的（意味着它们当前有音符在响）。当一个音符停止响时，声音变为非活动状态，意味着它现在可以接受合成器接收到的下一个音符请求。一个传入的 MIDI 命令流很容易请求比合成器能够生成的更多同时音符。当所有合成器的声音都是活动的时，下一个 Note On 请求应该如何处理？合成器可以实现不同的策略：最近请求的音符可以被忽略；或者通过停止另一个音符，比如最近启动的音符，来播放它。

尽管 MIDI 规范并不要求这样做，合成器可以公开每个声音的内容。Java Sound API 包括一个`VoiceStatus`类来实现这一目的。

一个`VoiceStatus`报告了声音当前的活动或非活动状态，MIDI 通道，银行和程序号，MIDI 音符号，以及 MIDI 音量。

有了这个背景，让我们来看一下 Java Sound API 合成的具体细节。

## 管理乐器和音色库

在许多情况下，一个程序可以使用`Synthesizer`对象而几乎不需要显式调用任何合成 API。例如，假设你正在播放一个标准的 MIDI 文件。你将其加载到一个`Sequence`对象中，通过让一个序列器将数据发送到默认的合成器来播放。序列中的数据按照预期控制合成器，按时播放所有正确的音符。

然而，有些情况下这种简单的情景是不够的。序列包含正确的音乐，但乐器听起来全错了！这种不幸的情况可能是因为 MIDI 文件的创建者心目中的乐器与当前加载到合成器中的乐器不同。

MIDI 1.0 规范提供了银行选择和程序更改命令，这些命令影响每个 MIDI 通道上当前播放的乐器。然而，该规范并未定义每个补丁位置（银行和程序号）应该放置什么乐器。较新的 General MIDI 规范通过定义一个包含 128 个与特定乐器声音对应的程序的银行来解决这个问题。General MIDI 合成器使用 128 个与指定集合匹配的乐器。即使播放应该是相同乐器的不同 General MIDI 合成器听起来可能会有很大不同。然而，一个 MIDI 文件在大多数情况下应该听起来相似（即使不完全相同），无论哪个 General MIDI 合成器在播放它。

尽管如此，并非所有的 MIDI 文件创建者都希望受限于 General MIDI 定义的 128 种音色。本节展示如何更改合成器附带的默认乐器集合。（如果没有默认设置，意味着在访问合成器时没有加载任何乐器，那么无论如何你都必须使用这个 API 开始。）

### 了解加载的乐器

要了解当前加载到合成器中的乐器是否符合你的要求，可以调用这个`Synthesizer`方法：

```java
Instrument[] getLoadedInstruments() 

```

并遍历返回的数组，查看当前加载的确切乐器。很可能，你会在用户界面中显示乐器的名称（使用`Instrument`的`getName`方法），让用户决定是否使用这些乐器或加载其他乐器。`Instrument` API 包括一个报告乐器属于哪个声音库的方法。声音库的名称可能帮助你的程序或用户确定乐器的确切信息。

这个`Synthesizer`方法：

```java
Soundbank getDefaultSoundbank() 

```

给出默认的声音库。`Soundbank` API 包括检索声音库名称、供应商和版本号的方法，通过这些信息，程序或用户可以验证库的身份。然而，当你第一次获得一个合成器时，不能假设默认声音库中的乐器已经被加载到合成器中。例如，一个合成器可能有大量内置乐器可供使用，但由于其有限的内存，它可能不会自动加载它们。

### 加载不同的乐器

用户可能决定加载与当前乐器不同的乐器（或者你可能以编程方式做出这个决定）。以下方法告诉你合成器附带哪些乐器（而不必从声音库文件加载）：

```java
Instrument[] getAvailableInstruments()

```

你可以通过调用以下方法加载任何这些乐器：

```java
boolean loadInstrument(Instrument instrument) 

```

乐器被加载到合成器中，位置由乐器的`Patch`对象指定（可以使用`Instrument`的`getPatch`方法检索）。

要从其他声音库加载乐器，首先调用`Synthesizer`的`isSupportedSoundbank`方法，确保声音库与此合成器兼容（如果不兼容，可以遍历系统的合成器尝试找到支持声音库的合成器）。然后可以调用这些方法之一从声音库加载乐器：

```java
boolean loadAllInstruments(Soundbank soundbank) 
boolean loadInstruments(Soundbank soundbank, 
  Patch[] patchList) 

```

正如名称所示，第一个加载给定声音库中的所有乐器，第二个加载声音库中选择的乐器。你也可以使用`Soundbank`的`getInstruments`方法访问所有乐器，然后遍历它们，并使用`loadInstrument`逐个加载选择的乐器。

不需要加载的所有乐器来自同一个声音库。您可以使用`loadInstrument`或`loadInstruments`从一个声音库加载某些乐器，从另一个声音库加载另一组乐器，依此类推。

每个乐器都有自己的`Patch`对象，指定了乐器应加载到合成器的位置。该位置由银行号和程序号定义。没有 API 可以通过更改补丁的银行或程序号来更改位置。

然而，可以使用`Synthesizer`的以下方法将乐器加载到除其补丁指定位置之外的位置：

```java
boolean remapInstrument(Instrument from, Instrument to) 

```

此方法从合成器中卸载其第一个参数，并将其第二个参数放置在第一个参数占用的合成器补丁位置。

### 卸载乐器

将乐器加载到程序位置会自动卸载该位置已经加载的任何乐器，如果有的话。您还可以显式卸载乐器，而不一定要用新的替换它们。`Synthesizer`包括三个与三个加载方法对应的卸载方法。如果合成器接收到选择当前未加载任何乐器的程序位置的程序更改消息，则在发送程序更改消息的 MIDI 通道上不会有任何声音。

### 访问声音库资源

一些合成器在其声音库中存储除乐器之外的其他信息。例如，波表合成器存储一个或多个乐器可以访问的音频样本。因为样本可能被多个乐器共享，它们独立于任何乐器存储在声音库中。`Soundbank`接口和`Instrument`类都提供一个名为`getSoundbankResources`的方法调用，返回一个`SoundbankResource`对象列表。这些对象的细节特定于为其设计声音库的合成器。在波表合成的情况下，资源可能是一个封装自音频录音片段的一系列音频样本的对象。使用其他合成技术的合成器可能在合成器的`SoundbankResources`数组中存储其他类型的对象。

## 查询合成器的功能和当前状态

`Synthesizer`接口包括返回有关合成器功能的信息的方法：

```java
    public long getLatency()
    public int getMaxPolyphony()

```

延迟度量了传递 MIDI 消息到合成器并合成器实际产生相应结果之间的最坏情况延迟。例如，合成器在接收到音符开启事件后可能需要几毫秒才开始生成音频。

`getMaxPolyphony` 方法指示合成器可以同时发出多少音符，如前面在 Voices 下讨论的那样。如同在同一讨论中提到的，合成器可以提供关于其音色的信息。这是通过以下方法实现的：

```java
public VoiceStatus[] getVoiceStatus()

```

返回的数组中的每个 `VoiceStatus` 报告了音色的当前活动或非活动状态、MIDI 通道、银行和程序号、MIDI 音符号码和 MIDI 音量。数组的长度通常应该与 `getMaxPolyphony` 返回的相同数量一样。如果合成器没有播放，所有其 `VoiceStatus` 对象的 active 字段都设置为 `false`。

您可以通过检索其 `MidiChannel` 对象并查询其状态来了解有关合成器当前状态的其他信息。这将在下一节中更详细地讨论。

## 使用通道

有时访问合成器的 `MidiChannel` 对象直接是有用或必要的。本节讨论了这种情况。

### 在不使用序列器的情况下控制合成器

当使用序列器时，比如从 MIDI 文件中读取的序列，您不需要自己向合成器发送 MIDI 命令。相反，您只需将序列加载到序列器中，将序列器连接到合成器，并让其运行。序列器负责安排事件的时间表，结果是可预测的音乐表现。当所需音乐事先已知时，这种情况是有效的，这在从文件中读取时是正确的。

然而，在某些情况下，音乐是在播放时即时生成的。例如，用户界面可能会显示一个音乐键盘或吉他指板，并允许用户通过鼠标点击随意弹奏音符。另一个例子，一个应用程序可能使用合成器不是为了演奏音乐本身，而是为了根据用户的操作生成音效。这种情况在游戏中很典型。最后一个例子，应用程序可能确实正在播放从文件中读取的音乐，但用户界面允许用户与音乐互动，动态地改变它。在所有这些情况下，应用程序直接向合成器发送命令，因为 MIDI 消息需要立即传递，而不是被安排在将来的某个确定时间点。

有至少两种方法可以将 MIDI 消息发送到合成器而不使用序列器。第一种方法是构造一个 `MidiMessage` 并通过 `Receiver` 的 send 方法将其传递给合成器。例如，要在 MIDI 通道 5（从 1 开始计数）上立即产生中央 C（MIDI 音符号码 60），可以执行以下操作：

```java
    ShortMessage myMsg = new ShortMessage();
    // Play the note Middle C (60) moderately loud
    // (velocity = 93)on channel 4 (zero-based).
    myMsg.setMessage(ShortMessage.NOTE_ON, 4, 60, 93); 
    Synthesizer synth = MidiSystem.getSynthesizer();
    Receiver synthRcvr = synth.getReceiver();
    synthRcvr.send(myMsg, -1); // -1 means no time stamp

```

第二种方法是完全绕过消息传递层（即 `MidiMessage` 和 `Receiver` API），直接与合成器的 `MidiChannel` 对象交互。您首先需要检索合成器的 `MidiChannel` 对象，使用以下 `Synthesizer` 方法：

```java
public MidiChannel[] getChannels()

```

之后，您可以直接调用所需的`MidiChannel`方法。这比将相应的`MidiMessages`发送到合成器的`Receiver`并让合成器处理与其自己的`MidiChannels`的通信更直接。例如，前面示例对应的代码将是：

```java
    Synthesizer synth = MidiSystem.getSynthesizer();
    MidiChannel chan[] = synth.getChannels(); 
    // Check for null; maybe not all 16 channels exist.
    if (chan[4] != null) {
         chan[4].noteOn(60, 93); 
    }

```

### 获取通道的当前状态

`MidiChannel`接口提供了与 MIDI 规范中定义的每个“通道音频”或“通道模式”消息一一对应的方法。我们在前面的示例中看到了使用 noteOn 方法的情况。但是，除了这些经典方法之外，Java Sound API 的`MidiChannel`接口还添加了一些“get”方法，用于检索最近由相应的音频或模式“set”方法设置的值：

```java
    int       getChannelPressure()
    int       getController(int controller)
    boolean   getMono()
    boolean   getOmni() 
    int       getPitchBend() 
    int       getPolyPressure(int noteNumber)
    int       getProgram()

```

这些方法可能对向用户显示通道状态或决定随后发送给通道的值很有用。

### 静音和独奏通道

Java Sound API 添加了每个通道独奏和静音的概念，这不是 MIDI 规范所要求的。这类似于 MIDI 序列轨道上的独奏和静音。

如果静音打开，该通道将不会发声，但其他通道不受影响。如果独奏打开，该通道和任何其他独奏的通道将会发声（如果它没有被静音），但其他通道不会发声。同时被独奏和静音的通道将不会发声。`MidiChannel` API 包括四种方法：

```java
    boolean      getMute() 
    boolean      getSolo()
    void         setMute(boolean muteState) 
    void         setSolo(boolean soloState)

```

## 允许播放合成声音

任何已安装的 MIDI 合成器产生的音频通常会通过采样音频系统路由。如果您的程序没有权限播放音频，则合成器的声音将听不到，并且会抛出安全异常。有关音频权限的更多信息，请参阅之前关于使用音频资源的权限使用音频资源的权限的讨论。
