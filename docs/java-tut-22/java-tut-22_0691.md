# 服务提供者接口简介

> 原文：[`docs.oracle.com/javase/tutorial/sound/SPI-intro.html`](https://docs.oracle.com/javase/tutorial/sound/SPI-intro.html)

## 什么是服务？

服务是声音处理功能单元，当应用程序使用 Java Sound API 的实现时自动可用。它们由执行读取、写入、混合、处理和转换音频和 MIDI 数据工作的对象组成。Java Sound API 的实现通常提供一组基本服务，但 API 中也包含机制，支持第三方开发人员（或实现供应商自身）开发新声音服务。这些新服务可以“插入”到现有安装的实现中，扩展其功能而不需要发布新版本。在 Java Sound API 架构中，第三方服务被集成到系统中，以便应用程序的接口与“内置”服务的接口相同。在某些情况下，使用 `javax.sound.sampled` 和 `javax.sound.midi` 包的应用程序开发人员甚至可能不知道他们正在使用第三方服务。

潜在的第三方采样音频服务示例包括：

+   声音文件读取器和写入器

+   在不同音频数据格式之间转换的转换器

+   新的音频混音器和输入/输出设备，无论是纯粹在软件中实现，还是在硬件中具有软件接口

第三方 MIDI 服务可能包括：

+   MIDI 文件读取器和写入器

+   用于各种类型声音库文件的读取器（通常特定于特定合成器）

+   受 MIDI 控制的声音合成器、音序器和 I/O 端口，无论是纯粹在软件中实现，还是在硬件中具有软件接口

## 服务如何工作

`javax.sound.sampled` 和 `javax.sound.midi` 包为希望在其应用程序中包含声音服务的应用程序开发人员提供功能。这些包是声音服务的*消费者*，提供接口以获取有关音频和 MIDI 服务的信息、控制和访问。此外，Java Sound API 还提供了两个定义抽象类的包，供声音服务的*提供者*使用：`javax.sound.sampled.spi` 和 `javax.sound.midi.spi` 包。

新声音服务的开发人员实现 SPI 包中适当类的具体子类。这些子类以及支持新服务所需的任何其他类都放在一个包含所包含服务描述的 Java 存档（JAR）存档文件中。当此 JAR 文件安装在用户的 `CLASSPATH` 中时，运行时系统会自动使新服务可用，扩展 Java 平台运行时系统的功能。

一旦安装了新服务，它就可以像以前安装的任何服务一样访问。服务的消费者可以通过调用`javax.sound.sampled`和`javax.sound.midi`包中的`AudioSystem`和`MidiSystem`类的方法来获取有关新服务的信息，或获取新服务类的实例，以返回有关新服务的信息，或返回新的或现有服务类的实例。应用程序无需直接引用 SPI 包（及其子类）中的类来使用已安装的服务。

例如，假设一个名为 Acme Software, Inc.的假想服务提供商有兴趣提供一个允许应用程序读取新格式声音文件的包（但其音频数据是标准数据格式的）。SPI 类`AudioFileReader`可以被子类化为一个名为`AcmeAudioFileReader`的类。在新的子类中，Acme 将提供`AudioFileReader`中定义的所有方法的实现；在这种情况下，只有两个方法（带参数变体），`getAudioFileFormat`和`getAudioInputStream`。然后，当应用程序尝试读取一个恰好是 Acme 文件格式的声音文件时，它会调用`javax.sound.sampled`中的`AudioSystem`类的方法来访问文件和有关文件的信息。方法`AudioSystem.getAudioInputStream`和`AudioSystem.getAudioFileFormat`提供了一个标准的 API 来读取音频流；安装了`AcmeAudioFileReader`类后，此接口会被扩展以透明地支持新文件类型。应用程序开发人员不需要直接访问新注册的 SPI 类：`AudioSystem`对象方法会将查询传递给已安装的`AcmeAudioFileReader`类。

为什么要有这些“工厂”类？为什么不允许应用程序开发人员直接访问新提供的服务？这是一种可能的方法，但通过门卫系统对象管理和实例化所有服务可以使应用程序开发人员免于了解已安装服务的身份。应用程序开发人员只需使用对他们有价值的服务，甚至可能都没有意识到。同时，这种架构允许服务提供者有效地管理其包中的可用资源。

通常，新声音服务的使用对应用程序是透明的。例如，想象一种情况，应用程序开发人员想要从文件中读取音频流。假设`thePathName`标识了一个音频输入文件，程序会这样做：

```java
    File theInFile = new File(thePathName);
    AudioInputStream theInStream = AudioSystem.getAudioInputStream(theInFile); 

```

在幕后，`AudioSystem`确定了哪个已安装的服务可以读取文件，并要求其提供音频数据作为`AudioInputStream`对象。开发人员可能不知道或甚至不关心输入音频文件是某种新文件格式（例如 Acme 的格式），这些格式由已安装的第三方服务支持。程序与流的第一次接触是通过`AudioSystem`对象，其后所有对流及其属性的访问都是通过`AudioInputStream`的方法。这两者都是`javax.sound.sampled` API 中的标准对象；新文件格式可能需要的特殊处理完全被隐藏起来。

## 服务提供者如何准备新服务

服务提供者以特殊格式的 JAR 文件提供其新服务，这些文件将被安装在用户系统中 Java 运行时将找到的目录中。JAR 文件是存档文件，每个文件包含一组文件，这些文件可能在存档中的分层目录结构中组织。关于放入这些存档的类文件的准备细节将在接下来的几页中讨论，这些页面描述了音频和 MIDI SPI 包的具体内容；在这里，我们只是概述 JAR 文件创建的过程。

新服务或服务的 JAR 文件应包含 JAR 文件中支持的每个服务的类文件。遵循 Java 平台的约定，每个类文件都具有新定义类的名称，这是一个抽象服务提供者类的具体子类。JAR 文件还必须包含新服务实现所需的任何支持类。为了使运行时系统的服务提供者机制能够定位新服务，JAR 文件还必须包含特殊文件（下文描述），将 SPI 类名称映射到正在定义的新子类。

继续我们上面的例子，假设 Acme Software, Inc.正在分发一套新的采样音频服务包。假设这个包包含两个新服务：

+   `AcmeAudioFileReader`类，如上所述，是`AudioFileReader`的子类

+   一个名为`AcmeAudioFileWriter`的`AudioFileWriter`子类，将以 Acme 的新格式编写声音文件

从一个任意目录开始——我们称之为`/devel`——我们创建子目录并将新的类文件放入其中，以一种组织方式来给出新类将被引用的期望路径名：

```java
    com/acme/AcmeAudioFileReader.class
    com/acme/AcmeAudioFileWriter.class

```

此外，对于每个新的 SPI 类的子类，我们在一个名为`META-INF/services`的特殊命名目录中创建一个映射文件。文件的名称是被子类化的 SPI 类的名称，文件包含该 SPI 抽象类的新子类的名称。

我们创建文件

```java
  META-INF/services/javax.sound.sampled.spi.AudioFileReader

```

包括

```java
    # Providers of sound file-reading services 
    # (a comment line begins with a pound sign)
    com.acme.AcmeAudioFileReader

```

以及文件

```java
  META-INF/services/javax.sound.sampled.spi.AudioFileWriter

```

包括

```java
    # Providers of sound file-writing services 
    com.acme.AcmeAudioFileWriter

```

现在我们在任何目录中运行`jar`命令行：

```java
jar cvf acme.jar -C /devel .

```

`-C`选项会导致`jar`切换到`/devel`目录，而不是使用执行命令的目录。最后的句点参数指示`jar`归档该目录的所有内容（即`/devel`），但不包括目录本身。

这次运行将创建文件`acme.jar`，其中包含以下内容：

```java
com/acme/AcmeAudioFileReader.class
com/acme/AcmeAudioFileWriter.class
META-INF/services/javax.sound.sampled.spi.AudioFileReader
META-INF/services/javax.sound.sampled.spi.AudioFileWriter
META-INF/Manifest.mf

```

文件`Manifest.mf`是由`jar`工具本身生成的，其中列出了存档中包含的所有文件。

## 用户如何安装新服务

对于希望通过他们的应用程序获得新服务访问权限的最终用户（或系统管理员），安装是简单的。他们将提供的 JAR 文件放在他们的`CLASSPATH`中的一个目录中。在执行时，Java 运行时会在需要时找到引用的类。

安装同一服务的多个提供者并不是错误。例如，两个不同的服务提供者可能提供支持读取相同类型的声音文件。在这种情况下，系统会任意选择一个提供者。在意识到哪个提供者被选择的用户应该只安装所需的那个。
