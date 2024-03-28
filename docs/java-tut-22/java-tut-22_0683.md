# 使用文件和格式转换器

> 原文：[`docs.oracle.com/javase/tutorial/sound/converters.html`](https://docs.oracle.com/javase/tutorial/sound/converters.html)

大多数处理声音的应用程序需要读取声音文件或音频流。这是常见的功能，无论程序随后对读取的数据做什么（如播放、混合或处理）。同样，许多程序需要写入声音文件（或流）。在某些情况下，已读取的数据（或将要写入的数据）需要转换为不同的格式。

正如在访问音频系统资源中简要提到的，Java Sound API 为应用程序开发人员提供了各种设施，用于文件输入/输出和格式转换。应用程序可以读取、写入和在各种声音文件格式和音频数据格式之间进行转换。

采样包概述介绍了与声音文件和音频数据格式相关的主要类。作为回顾：

+   从文件中读取或写入的音频数据流由`AudioInputStream`对象表示。（`AudioInputStream`继承自`java.io.InputStream`。）

+   此音频数据的格式由`AudioFormat`对象表示。

    此格式指定了音频样本本身的排列方式，但不指定它们可能存储在的文件的结构。换句话说，`AudioFormat`描述了“原始”音频数据，例如系统在从麦克风输入捕获或从声音文件解析后可能传递给程序的数据。`AudioFormat`包括编码、字节顺序、通道数、采样率和每个样本的位数等信息。

+   有几种众所周知的标准声音文件格式，如 WAV、AIFF 或 AU。不同类型的声音文件具有不同的存储音频数据以及存储有关音频数据的描述信息的结构。Java Sound API 中通过`AudioFileFormat`对象表示声音文件格式。`AudioFileFormat`包括一个`AudioFormat`对象来描述文件中存储的音频数据的格式，还包括有关文件类型和文件中数据长度的信息。

+   `AudioSystem`类提供了方法，用于将来自`AudioInputStream`的音频数据流存储到特定类型的音频文件中（换句话说，写入文件），从音频文件中提取音频字节流（`AudioInputStream`）（换句话说，读取文件），以及将音频数据从一种数据格式转换为另一种数据格式。本页分为三个部分，解释了这三种活动。

* * *

**注意：**

Java Sound API 的实现不一定提供全面的设施来读取、写入和转换不同数据和文件格式的音频。它可能仅支持最常见的数据和文件格式。然而，服务提供者可以开发和分发扩展这一集合的转换服务，正如你稍后将在提供采样音频服务中看到的那样。`AudioSystem`类提供了允许应用程序了解可用转换的方法，稍后在转换文件和数据格式下描述。

* * *

## 读取声音文件

`AudioSystem`类提供了两种类型的文件读取服务：

+   存储在声音文件中的音频数据格式的信息

+   可从声音文件中读取的格式化音频数据流

第一个是`getAudioFileFormat`方法的三个变体：

```java
    static AudioFileFormat getAudioFileFormat (java.io.File file)
    static AudioFileFormat getAudioFileFormat(java.io.InputStream stream)
    static AudioFileFormat getAudioFileFormat (java.net.URL url)

```

如上所述，返回的`AudioFileFormat`对象告诉你文件类型、文件中数据的长度、编码、字节顺序、通道数、采样率和每个样本的位数。

第二种文件读取功能由这些`AudioSystem`方法提供。

```java
    static AudioInputStream getAudioInputStream (java.io.File file)
    static AudioInputStream getAudioInputStream (java.net.URL url)
    static AudioInputStream getAudioInputStream (java.io.InputStream stream)

```

这些方法给你一个对象（一个`AudioInputStream`），让你使用`AudioInputStream`的读取方法读取文件的音频数据。我们马上会看到一个例子。

假设你正在编写一个声音编辑应用程序，允许用户从文件中加载声音数据，显示相应的波形图或频谱图，编辑声音，播放编辑后的数据，并将结果保存在新文件中。或者你的程序将读取文件中存储的数据，应用某种信号处理（例如减慢声音而不改变音调的算法），然后播放处理后的音频。在任何情况下，你需要访问音频文件中包含的数据。假设你的程序提供了一些方式让用户选择或指定输入声音文件，读取该文件的音频数据涉及三个步骤：

1.  从文件中获取一个`AudioInputStream`对象。

1.  创建一个字节数组，用于存储文件中连续的数据块。

1.  重复从音频输入流中读取字节到数组中。在每次迭代中，对数组中的字节执行一些有用的操作（例如，你可以播放它们、过滤它们、分析它们、显示它们或将它们写入另一个文件）。

以下代码片段概述了这些步骤：

```java
int totalFramesRead = 0;
File fileIn = new File(somePathName);
// somePathName is a pre-existing string whose value was
// based on a user selection.
try {
  AudioInputStream audioInputStream = 
    AudioSystem.getAudioInputStream(fileIn);
  int bytesPerFrame = 
    audioInputStream.getFormat().getFrameSize();
    if (bytesPerFrame == AudioSystem.NOT_SPECIFIED) {
    // some audio formats may have unspecified frame size
    // in that case we may read any amount of bytes
    bytesPerFrame = 1;
  } 
  // Set an arbitrary buffer size of 1024 frames.
  int numBytes = 1024 * bytesPerFrame; 
  byte[] audioBytes = new byte[numBytes];
  try {
    int numBytesRead = 0;
    int numFramesRead = 0;
    // Try to read numBytes bytes from the file.
    while ((numBytesRead = 
      audioInputStream.read(audioBytes)) != -1) {
      // Calculate the number of frames actually read.
      numFramesRead = numBytesRead / bytesPerFrame;
      totalFramesRead += numFramesRead;
      // Here, do something useful with the audio data that's 
      // now in the audioBytes array...
    }
  } catch (Exception ex) { 
    // Handle the error...
  }
} catch (Exception e) {
  // Handle the error...
}

```

让我们看看上面代码示例中发生了什么。首先，外部的`try`子句通过调用`AudioSystem.getAudioInputStream(File)`方法实例化了一个`AudioInputStream`对象。此方法透明地执行了所有必要的测试，以确定指定的文件实际上是 Java Sound API 支持的声音文件类型。如果正在检查的文件（例如此示例中的`fileIn`）不是声音文件，或者是某种不受支持的声音文件类型，将抛出`UnsupportedAudioFileException`异常。这种行为很方便，因为应用程序员不需要测试文件属性，也不需要遵守任何文件命名约定。相反，`getAudioInputStream`方法负责处理验证输入文件所需的所有底层解析和验证。然后，外部的`try`子句创建了一个名为`audioBytes`的字节数组，长度是任意固定的。我们确保其字节长度等于整数帧数，这样我们就不会最终只读取部分帧或更糟糕的是只读取部分样本。这个字节数组将作为一个缓冲区，临时保存从流中读取的一块音频数据。如果我们知道我们将只读取非常短的声音文件，我们可以使此数组与文件中的数据长度相同，通过从`AudioInputStream`的`getFrameLength`方法返回的帧数长度来推导其字节长度。（实际上，我们可能会使用`Clip`对象。）但为了避免在一般情况下耗尽内存，我们会一次读取一个缓冲区的文件。

内部的`try`子句包含一个`while`循环，在这里我们将音频数据从`AudioInputStream`读入字节数组中。您应该在此循环中添加代码，以适当处理此数组中的音频数据，以满足程序的需求。如果您对数据应用某种信号处理，您可能需要进一步查询`AudioInputStream`的`AudioFormat`，以了解每个样本的位数等信息。

注意，方法`AudioInputStream.read(byte[])`返回读取的*字节数*，而不是样本数或帧数。当没有更多数据可读取时，此方法返回-1。一旦检测到这种情况，我们就会跳出`while`循环。

## 写入声音文件

前一节描述了读取声音文件的基础知识，使用了`AudioSystem`和`AudioInputStream`类的特定方法。本节描述了如何将音频数据写入新文件。

以下`AudioSystem`方法创建指定文件类型的磁盘文件。该文件将包含指定`AudioInputStream`中的音频数据：

```java
static int write(AudioInputStream in, 
  AudioFileFormat.Type fileType, File out)

```

请注意，第二个参数必须是系统支持的文件类型之一（例如，AU、AIFF 或 WAV），否则`write`方法将抛出`IllegalArgumentException`。为了避免这种情况，您可以通过调用此`AudioSystem`方法来测试特定`AudioInputStream`是否可以写入特定类型的文件：

```java
static boolean isFileTypeSupported
  (AudioFileFormat.Type fileType, AudioInputStream stream)

```

只有在支持特定组合时才会返回`true`。

更一般地，您可以通过调用这些`AudioSystem`方法来了解系统可以写入哪些类型的文件：

```java
static AudioFileFormat.Type[] getAudioFileTypes() 
static AudioFileFormat.Type[] getAudioFileTypes(AudioInputStream stream) 

```

其中第一个返回系统可以写入的所有文件类型，第二个仅返回系统可以从给定音频输入流写入的文件类型。

以下摘录演示了使用上述提到的`write`方法从`AudioInputStream`创建输出文件的一种技术。

```java
File fileOut = new File(someNewPathName);
AudioFileFormat.Type fileType = fileFormat.getType();
if (AudioSystem.isFileTypeSupported(fileType, 
    audioInputStream)) {
  AudioSystem.write(audioInputStream, fileType, fileOut);
}

```

上面的第一个语句创建了一个新的`File`对象`fileOut`，带有用户或程序指定的路径名。第二个语句从名为`fileFormat`的预先存在的`AudioFileFormat`对象中获取文件类型，该对象可能已从其他声音文件（例如，在上面的读取声音文件中读取的文件）中获取。 （您可以选择提供您想要的任何支持的文件类型，而不是从其他地方获取文件类型。例如，您可以删除第二个语句，并将上面代码中的其他两个`fileType`出现替换为`AudioFileFormat.Type.WAVE`。）

第三个语句测试指定类型的文件是否可以从所需的`AudioInputStream`写入。与文件格式一样，此流可能是先前读取的声音文件派生而来的。（如果是这样，那么您可能以某种方式处理或更改了其数据，否则有更简单的方法可以简单地复制文件。）或者也许该流包含从麦克风输入中新捕获的字节。

最后，流、文件类型和输出文件被传递给`AudioSystem`.`write`方法，以实现写入文件的目标。

## 转换文件和数据格式

请回想一下什么是格式化音频数据？，Java Sound API 区分音频*文件*格式和音频*数据*格式。这两者或多或少是独立的。粗略地说，数据格式指的是计算机表示每个原始数据点（样本）的方式，而文件格式指的是存储在磁盘上的声音文件的组织方式。每种声音文件格式都有一个定义文件头中存储信息的特定结构。在某些情况下，文件格式还包括包含某种形式元数据的结构，除了实际的“原始”音频样本。本页的其余部分将探讨 Java Sound API 的方法，这些方法使得可以进行各种文件格式和数据格式的转换。

## 从一种文件格式转换为另一种

本节介绍了在 Java Sound API 中转换音频文件类型的基础知识。再次，我们提出一个假设的程序，其目的是从任意输入文件中读取音频数据并将其写入类型为 AIFF 的文件中。当然，输入文件必须是系统能够读取的类型，输出文件必须是系统能够写入的类型。（在此示例中，我们假设系统能够写入 AIFF 文件。）示例程序不执行任何数据格式转换。如果输入文件的数据格式无法表示为 AIFF 文件，则程序简单地通知用户存在问题。另一方面，如果输入音频文件已经是 AIFF 文件，则程序会通知用户无需转换。

以下函数实现了刚才描述的逻辑：

```java
public void ConvertFileToAIFF(String inputPath, 
  String outputPath) {
  AudioFileFormat inFileFormat;
  File inFile;
  File outFile;
  try {
    inFile = new File(inputPath);
    outFile = new File(outputPath);     
  } catch (NullPointerException ex) {
    System.out.println("Error: one of the 
      ConvertFileToAIFF" +" parameters is null!");
    return;
  }
  try {
    // query file type
    inFileFormat = AudioSystem.getAudioFileFormat(inFile);
    if (inFileFormat.getType() != AudioFileFormat.Type.AIFF) 
    {
      // inFile is not AIFF, so let's try to convert it.
      AudioInputStream inFileAIS = 
        AudioSystem.getAudioInputStream(inFile);
      inFileAIS.reset(); // rewind
      if (AudioSystem.isFileTypeSupported(
             AudioFileFormat.Type.AIFF, inFileAIS)) {
         // inFileAIS can be converted to AIFF. 
         // so write the AudioInputStream to the
         // output file.
         AudioSystem.write(inFileAIS,
           AudioFileFormat.Type.AIFF, outFile);
         System.out.println("Successfully made AIFF file, "
           + outFile.getPath() + ", from "
           + inFileFormat.getType() + " file, " +
           inFile.getPath() + ".");
         inFileAIS.close();
         return; // All done now
       } else
         System.out.println("Warning: AIFF conversion of " 
           + inFile.getPath()
           + " is not currently supported by AudioSystem.");
    } else
      System.out.println("Input file " + inFile.getPath() +
          " is AIFF." + " Conversion is unnecessary.");
  } catch (UnsupportedAudioFileException e) {
    System.out.println("Error: " + inFile.getPath()
        + " is not a supported audio file type!");
    return;
  } catch (IOException e) {
    System.out.println("Error: failure attempting to read " 
      + inFile.getPath() + "!");
    return;
  }
}

```

如前所述，此示例函数`ConvertFileToAIFF`的目的是查询输入文件，以确定它是否是 AIFF 音频文件，如果不是，则尝试将其转换为 AIFF 格式，生成一个新的副本，其路径名由第二个参数指定。（作为练习，您可以尝试使此函数更通用，以便不总是转换为 AIFF，而是根据新的函数参数指定的文件类型进行转换。）请注意，副本的音频数据格式——即新文件模仿原始输入文件的音频数据格式。

大部分此函数是不言自明的，并且与 Java Sound API 无关。然而，例程中使用了一些 Java Sound API 方法，这些方法对于音频文件类型转换至关重要。这些方法调用都在上面的第二个`try`子句中找到，包括以下内容：

+   `AudioSystem.getAudioFileFormat`：在此处用于确定输入文件是否已经是 AIFF 类型。如果是，则函数会快速返回；否则，转换尝试继续进行。

+   `AudioSystem.isFileTypeSupported`：指示系统是否可以写入包含来自指定`AudioInputStream`的音频数据的指定类型文件。在我们的示例中，如果指定的音频输入文件可以转换为 AIFF 音频文件格式，则此方法返回`true`。如果不支持`AudioFileFormat.Type.AIFF`，`ConvertFileToAIFF`会发出警告，指出无法转换输入文件，然后返回。

+   `AudioSystem.write`：在此处用于将`inFileAIS`的音频数据从`AudioInputStream`写入到输出文件`outFile`中。

这些方法中的第二个`isFileTypeSupported`方法有助于在写入之前确定特定输入音频文件是否可以转换为特定输出音频文件类型。在下一节中，我们将看到如何通过对`ConvertFileToAIFF`示例例程进行一些修改，可以转换音频数据格式以及音频文件类型。

## 在不同数据格式之间转换音频

前一节展示了如何使用 Java Sound API 将文件从一种*文件*格式（即一种声音文件类型）转换为另一种。本节探讨了一些方法，这些方法使音频*数据*格式转换成为可能。

在前一节中，我们从一个任意类型的文件中读取数据，并将其保存在一个 AIFF 文件中。请注意，尽管我们改变了用于存储数据的文件类型，但我们并没有改变音频数据本身的格式。（大多数常见的音频文件类型，包括 AIFF，可以包含各种格式的音频数据。）因此，如果原始文件包含 CD 音质音频数据（16 位样本大小、44.1kHz 采样率和两个声道），那么我们的输出 AIFF 文件也会包含相同的数据。

现在假设我们想要指定输出文件的*数据*格式，以及文件类型。例如，也许我们正在保存许多长文件以供在互联网上使用，并且担心我们的文件所需的磁盘空间和下载时间。我们可能选择创建包含低分辨率数据的较小的 AIFF 文件-例如，具有 8 位样本大小、8kHz 采样率和单声道的数据。

不像之前那样详细地进行编码，让我们探讨一些用于数据格式转换的方法，并考虑我们需要对`ConvertFileToAIFF`函数进行的修改以实现新目标。

音频数据转换的主要方法再次在`AudioSystem`类中找到。这个方法是`getAudioInputStream`的一个变体：

```java
AudioInputStream getAudioInputStream(AudioFormat
    format, AudioInputStream stream)

```

此函数返回一个`AudioInputStream`，该流是使用指定的`AudioFormat``format`转换`stream`的结果。如果`AudioSystem`不支持转换，此函数会抛出`IllegalArgumentException`。

为了避免这种情况，我们可以首先通过调用这个`AudioSystem`方法来检查系统是否可以执行所需的转换：

```java
boolean isConversionSupported(AudioFormat targetFormat,
    AudioFormat sourceFormat)

```

在这种情况下，我们将`stream.getFormat()`作为第二个参数传递。

要创建一个特定的`AudioFormat`对象，我们使用下面显示的两个`AudioFormat`构造函数之一，要么：

```java
AudioFormat(float sampleRate, int sampleSizeInBits,
    int channels, boolean signed, boolean bigEndian)

```

使用线性 PCM 编码和给定参数构造一个`AudioFormat`，或者：

```java
AudioFormat(AudioFormat.Encoding encoding, 
    float sampleRate, int sampleSizeInBits, int channels,
    int frameSize, float frameRate, boolean bigEndian) 

```

还构造了一个`AudioFormat`，但允许您指定编码、帧大小和帧速率，除了其他参数。

现在，有了上面的方法，让我们看看如何扩展我们的`ConvertFileToAIFF`函数以执行所需的“低分辨率”音频数据格式转换。首先，我们将构造一个描述所需输出音频数据格式的`AudioFormat`对象。以下语句就足够了，并且可以插入到函数的顶部附近：

```java
AudioFormat outDataFormat = new AudioFormat((float) 8000.0,
(int) 8, (int) 1, true, false);

```

由于上面的`AudioFormat`构造函数描述了一个具有 8 位样本的格式，因此构造函数的最后一个参数，指定样本是大端还是小端，是无关紧要的。（大端与小端只有在样本大小大于一个字节时才是一个问题。）

以下示例展示了我们如何使用这个新的`AudioFormat`来转换我们从输入文件创建的`AudioInputStream`，`inFileAIS`：

```java
AudioInputStream lowResAIS;         
  if (AudioSystem.isConversionSupported(outDataFormat,   
    inFileAIS.getFormat())) {
    lowResAIS = AudioSystem.getAudioInputStream
      (outDataFormat, inFileAIS);
  }

```

不管我们在何处插入这段代码，只要它在构建`inFileAIS`之后即可。如果没有`isConversionSupported`测试，如果请求的特定转换不受支持，调用将失败并抛出`IllegalArgumentException`。（在这种情况下，控制将转移到我们函数中适当的`catch`子句。）

因此，在这个过程中，我们将产生一个新的`AudioInputStream`，这是通过将原始输入文件（以其`AudioInputStream`形式）转换为由`outDataFormat`定义的所需低分辨率音频数据格式而产生的。

生成所需的低分辨率 AIFF 声音文件的最后一步是将`AudioSystem.write`调用中的`AudioInputStream`参数（即第一个参数）替换为我们转换后的流`lowResAIS`，如下所示：

```java
AudioSystem.write(lowResAIS, AudioFileFormat.Type.AIFF, 
  outFile);

```

对我们之前的函数进行这几处修改，可以产生一个能够转换任何指定输入文件的音频数据和文件格式的东西，前提是系统支持转换。

## 学习可用的转换方式

几个`AudioSystem`方法会测试它们的参数，以确定系统是否支持特定的数据格式转换或文件写入操作。（通常，每个方法都与另一个方法配对，执行数据转换或写入文件。）其中一个查询方法`AudioSystem.isFileTypeSupported`在我们的示例函数`ConvertFileToAIFF`中被使用，以确定系统是否能够将音频数据写入 AIFF 文件。相关的`AudioSystem`方法`getAudioFileTypes(AudioInputStream)`返回给定流支持的所有文件类型的完整列表，作为`AudioFileFormat.Type`实例的数组。该方法：BEGINCODE boolean isConversionSupported(AudioFormat.Encoding encoding，

AudioFormat 格式）

用于确定是否可以从具有指定音频格式的音频输入流中获取具有指定编码的音频输入流。类似地，该方法：

```java
boolean isConversionSupported(AudioFormat newFormat,
                              AudioFormat oldFormat) 

```

告诉我们是否可以通过将具有音频格式`oldFormat`的`AudioInputStream`转换为具有指定音频格式`newFormat`的`AudioInputStream`来获得`AudioInputStream`。（这个方法在前一节代码片段中被调用，用于创建低分辨率音频输入流`lowResAIS`。）

这些与格式相关的查询有助于在尝试使用 Java Sound API 执行格式转换时防止错误。
