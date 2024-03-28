# 提供 MIDI 服务

> 原文：[`docs.oracle.com/javase/tutorial/sound/SPI-providing-MIDI.html`](https://docs.oracle.com/javase/tutorial/sound/SPI-providing-MIDI.html)

服务提供者接口简介 解释了`javax.sound.sampled.spi`和`javax.sound.midi.spi`包定义了供声音服务开发人员使用的抽象类。通过实现这些抽象类的子类，服务提供者可以创建一个扩展运行时系统功能的新服务。前一节介绍了如何使用`javax.sound.sampled.spi`包。本节讨论如何使用`javax.sound.midi.spi`包为处理 MIDI 设备和文件提供新服务。

`javax.sound.midi.spi` 包中有四个抽象类，代表着你可以为 MIDI 系统提供的四种不同类型的服务：

+   [`MidiFileWriter`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/spi/MidiFileWriter.html) 提供了 MIDI 文件写入服务。这些服务使应用程序能够将其生成或处理的 MIDI `Sequence` 保存到 MIDI 文件中。

+   [`MidiFileReader`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/spi/MidiFileReader.html) 提供了从 MIDI 文件中返回 MIDI `Sequence` 供应用程序使用的文件读取服务。

+   [`MidiDeviceProvider`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/spi/MidiDeviceProvider.html) 提供了一个或多个特定类型的 MIDI 设备实例，可能包括硬件设备。

+   [`SoundbankReader`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/spi/SoundbankReader.html) 提供了声音库文件读取服务。`SoundbankReader`的具体子类解析给定的声音库文件，生成一个可以加载到`Synthesizer`中的`Soundbank`对象。

应用程序不会直接创建服务对象的实例，无论是提供者对象，比如`MidiDeviceProvider`，还是由提供者对象提供的对象，比如`Synthesizer`。程序也不会直接引用 SPI 类。相反，应用程序会向`javax.sound.midi`包中的`MidiSystem`对象发出请求，而`MidiSystem`又会使用`javax.sound.midi.spi`类的具体子类来处理这些请求。

## 提供 MIDI 文件写入服务

有三种标准的 MIDI 文件格式，Java Sound API 的实现都可以支持：Type 0、Type 1 和 Type 2。这些文件格式在文件中的 MIDI 序列数据的内部表示上有所不同，并且适用于不同类型的序列。如果一个实现本身不支持所有三种类型，服务提供者可以为未实现的类型提供支持。还有标准 MIDI 文件格式的变体，其中一些是专有的，同样可以由第三方供应商支持。

写入 MIDI 文件的能力由`MidiFileWriter`的具体子类提供。这个抽象类直接类比于`javax.sampled.spi.AudioFileWriter`。同样，方法被分组为查询方法，用于了解可以写入哪些类型的文件，以及用于实际写入文件的方法。与`AudioFileWriter`一样，其中两个查询方法是具体的：

```java
boolean isFileTypeSupported(int fileType)
boolean isFileTypeSupported(int fileType, Sequence sequence) 

```

其中第一个提供关于文件写入器是否可以写入指定类型的 MIDI 文件类型的一般信息。第二个方法更具体：它询问特定 Sequence 是否可以写入指定类型的 MIDI 文件。通常情况下，您不需要覆盖这两个具体方法中的任何一个。在默认实现中，每个方法调用另外两个相应的查询方法之一，并遍历返回的结果。作为抽象方法，这另外两个查询方法需要在子类中实现：

```java
abstract int[] getMidiFileTypes() 
abstract int[] getMidiFileTypes(Sequence sequence) 

```

其中第一个返回一个数组，其中包含一般支持的所有文件类型。一个典型的实现可能会在文件写入器的构造函数中初始化数组，并从这个方法返回数组。从文件类型集中，第二个方法找到文件写入器可以写入给定 Sequence 的子集。根据 MIDI 规范，不是所有类型的序列都可以写入所有类型的 MIDI 文件。

`MidiFileWriter`子类的`write`方法执行将给定`Sequence`中的数据编码为请求的 MIDI 文件类型的正确数据格式，将编码流写入文件或输出流的操作：

```java
abstract int write(Sequence in, int fileType, 
                   java.io.File out) 
abstract int write(Sequence in, int fileType, 
                   java.io.OutputStream out) 

```

为了实现这一点，`write`方法必须通过迭代`Sequence`的轨道来解析`Sequence`，构建适当的文件头，并将头部和轨道写入输出。当然，MIDI 文件的头部格式由 MIDI 规范定义。它包括诸如标识这是 MIDI 文件的“魔数”，头部长度，轨道数以及序列的定时信息（分频类型和分辨率）等信息。MIDI 文件的其余部分由轨道数据组成，格式由 MIDI 规范定义。

让我们简要地看一下应用程序、MIDI 系统和服务提供者如何合作编写 MIDI 文件。在典型情况下，一个应用程序有一个特定的 MIDI `Sequence`要保存到文件中。程序查询`MidiSystem`对象，看看对于手头的特定`Sequence`支持哪些 MIDI 文件格式（如果有的话），然后尝试写入文件。`MidiSystem.getMidiFileTypes(Sequence)`方法返回系统可以写入特定序列的所有 MIDI 文件类型的数组。它通过调用每个已安装的`MidiFileWriter`服务的相应`getMidiFileTypes`方法来实现这一点，并将结果收集并以整数数组的形式返回，这可以被视为与给定`Sequence`兼容的所有文件类型的主列表。在写入`Sequence`到文件时，调用`MidiSystem.write`传递一个表示文件类型的整数，以及要写入的`Sequence`和输出文件；`MidiSystem`使用提供的类型来决定哪个已安装的`MidiFileWriter`应处理写入请求，并将相应的写入分派给适当的`MidiFileWriter`。

## 提供 MIDI 文件读取服务

`MidiFileReader`抽象类直接类似于`javax.sampled.spi.AudioFileReader`类。两者都包括两个重载的方法，每个方法都可以接受`File`、`URL`或`InputStream`参数。重载方法中的第一个返回指定文件的文件格式。对于`MidiFileReader`，API 为：

```java
abstract MidiFileFormat getMidiFileFormat(java.io.File file) 
abstract MidiFileFormat getMidiFileFormat(
    java.io.InputStream stream) 
abstract MidiFileFormat getMidiFileFormat(java.net.URL url) 

```

具体的子类必须实现这些方法来返回一个填充的`MidiFileFormat`对象，描述指定 MIDI 文件（或流或 URL）的格式，假设该文件是文件读取器支持的类型，并且包含有效的头信息。否则，应抛出`InvalidMidiDataException`。

另一个重载的方法从给定的文件、流或 URL 返回一个 MIDI `Sequence`：

```java
abstract Sequence getSequence(java.io.File file) 
abstract Sequence getSequence(java.io.InputStream stream) 
abstract Sequence getSequence(java.net.URL url) 

```

`getSequence`方法执行解析 MIDI 输入文件中的字节并构造相应`Sequence`对象的实际工作。这基本上是`MidiFileWriter.write`使用的过程的反向。由于 MIDI 文件的内容（由 MIDI 规范定义）与 Java Sound API 定义的`Sequence`对象之间存在一对一的对应关系，因此解析的细节是直接的。如果传递给`getSequence`的文件包含文件读取器无法解析的数据（例如，因为文件已损坏或不符合 MIDI 规范），则应抛出`InvalidMidiDataException`。

## 提供特定的 MIDI 设备

一个`MidiDeviceProvider`可以被视为提供一种或多种特定类型的 MIDI 设备的工厂。该类包括一个返回 MIDI 设备实例的方法，以及查询方法来了解该提供者可以提供哪些类型的设备。

与其他 `javax.sound.midi.spi` 服务一样，应用程序开发人员通过调用 `MidiSystem` 方法间接访问 `MidiDeviceProvider` 服务，本例中是 `MidiSystem.getMidiDevice` 和 `MidiSystem.getMidiDeviceInfo`。子类化 `MidiDeviceProvider` 的目的是提供一种新类型的设备，因此服务开发人员还必须为返回的设备创建一个相应的类，就像我们在 `javax.sound.sampled.spi` 包中看到的 `MixerProvider` 一样。在那里，返回的设备类实现了 `javax.sound.sampled.Mixer` 接口；这里实现了 `javax.sound.midi.MidiDevice` 接口。它还可以实现 `MidiDevice` 的子接口，如 `Synthesizer` 或 `Sequencer`。

因为 `MidiDeviceProvider` 的单个子类可以提供多种类型的 `MidiDevice`，所以该类的 `getDeviceInfo` 方法返回一个枚举不同可用 `MidiDevices` 的 `MidiDevice.Info` 对象数组：

```java
abstract MidiDevice.Info[] getDeviceInfo() 

```

返回的数组当然可以只包含一个元素。提供者的典型实现可能会在其构造函数中初始化一个数组，并在此处返回它。这使得 `MidiSystem` 可以遍历所有已安装的 `MidiDeviceProviders` 来构建所有已安装设备的列表。然后，`MidiSystem` 可以将这个列表（`MidiDevice.Info[]` 数组）返回给应用程序。

`MidiDeviceProvider` 还包括一个具体的查询方法：

```java
boolean isDeviceSupported(MidiDevice.Info info) 

```

这个方法允许系统查询提供者关于特定类型设备的信息。通常情况下，你不需要重写这个便利方法。默认实现会遍历由 `getDeviceInfo` 返回的数组，并将参数与每个元素进行比较。

第三个也是最后一个 `MidiDeviceProvider` 方法返回请求的设备：

```java
abstract MidiDevice getDevice(MidiDevice.Info info) 

```

这个方法应该首先测试参数，确保它描述的是该提供者可以提供的设备。如果不是，它应该抛出一个 `IllegalArgumentException`。否则，它会返回该设备。

## 提供声音库文件读取服务

`SoundBank` 是一组可以加载到 `Synthesizer` 中的 `Instruments`。`Instrument` 是一个实现声音合成算法的对象，产生特定类型的声音，并包含相关的名称和信息字符串。`SoundBank` 大致对应于 MIDI 规范中的一个银行，但它是一个更广泛和可寻址的集合；可以更好地将其视为 MIDI 银行的集合。

`SoundbankReader` 由一个重载的方法组成，系统调用该方法从声音库文件中读取一个 `Soundbank` 对象：

```java
abstract Soundbank getSoundbank(java.io.File file) 
abstract Soundbank getSoundbank(java.io.InputStream stream) 
abstract Soundbank getSoundbank(java.net.URL url) 

```

`SoundbankReader` 的具体子类将与特定提供者定义的 `SoundBank`、`Instrument` 和 `Synthesizer` 实现配合工作，以允许系统从文件中加载 `SoundBank` 到特定 `Synthesizer` 类的实例中。合成技术可能在一个 `Synthesizer` 到另一个 `Synthesizer` 之间有很大差异，因此，存储在 `Instrument` 或 `SoundBank` 中为 `Synthesizer` 合成过程提供控制或规范数据的数据可以采用各种形式。一种合成技术可能只需要少量字节的参数数据；另一种可能基于广泛的声音样本。`SoundBank` 中存在的资源将取决于它们加载到的 `Synthesizer` 的性质，因此 `SoundbankReader` 子类的 `getSoundbank` 方法的实现可以访问特定类型 `SoundBank` 的知识。此外，`SoundbankReader` 的特定子类了解用于存储 `SoundBank` 数据的特定文件格式。该文件格式可能是供应商特定和专有的。

`SoundBank` 只是一个接口，对 `SoundBank` 对象的内容只有弱约束。要实现这个接口，对象必须支持的方法（`getResources`、`getInstruments`、`getVendor`、`getName` 等）对对象包含的数据有宽松的要求。例如，`getResources` 和 `getInstruments` 可以返回空数组。子类化的 `SoundBank` 对象的实际内容，特别是它的乐器和非乐器资源，由服务提供者定义。因此，解析声音库文件的机制完全取决于该特定类型声音库文件的规范。

声音库文件是在 Java Sound API 之外创建的，通常由可以加载该类型声音库的合成器的供应商创建。一些供应商可能会提供用于创建此类文件的最终用户工具。
