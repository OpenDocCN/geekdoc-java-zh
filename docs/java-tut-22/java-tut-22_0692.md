# 提供采样音频服务

> 原文：[`docs.oracle.com/javase/tutorial/sound/SPI-providing-sampled.html`](https://docs.oracle.com/javase/tutorial/sound/SPI-providing-sampled.html)

正如你所知，Java Sound API 包括两个包，`javax.sound.sampled.spi` 和 `javax.sound.midi.spi`，它们定义了抽象类，供声音服务的开发者使用。通过实现并安装这些抽象类的子类，服务提供者可以注册新服务，扩展运行时系统的功能。本页面告诉你如何使用 `javax.sound.sampled.spi` 包来提供处理采样音频的新服务。

`javax.sound.sampled.spi` 包中有四个抽象类，代表着你可以为采样音频系统提供的四种不同类型的服务：

+   [`AudioFileWriter`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/spi/AudioFileWriter.html) 提供音频文件写入服务。这些服务使应用程序能够将音频数据流写入特定类型的文件。

+   [`AudioFileReader`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/spi/AudioFileReader.html) 提供文件读取服务。这些服务使应用程序能够确定音频文件的特性，并获取一个流，从中可以读取文件的音频数据。

+   [`FormatConversionProvider`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/spi/FormatConversionProvider.html) 提供音频数据格式转换服务。这些服务允许应用程序将音频流从一种数据格式转换为另一种。

+   [`MixerProvider`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/spi/MixerProvider.html) 提供特定类型混音器的管理。这种机制允许应用程序获取关于给定类型混音器的信息，并访问实例。

总结之前的讨论，服务提供者可以扩展运行时系统的功能。典型的 SPI 类有两种类型的方法：一种是响应关于特定提供者提供的服务类型的查询，另一种是直接执行新服务，或返回实际提供服务的对象实例。运行时环境的服务提供者机制提供了已安装服务与音频系统的*注册*，以及新服务提供者类的*管理*。

本质上，服务实例与应用程序开发人员之间存在双重隔离。应用程序从不直接创建服务对象的实例，例如混音器或格式转换器，以用于其音频处理任务。程序甚至不会直接从管理它们的 SPI 类中请求这些对象。应用程序向`javax.sound.sampled`包中的`AudioSystem`对象发出请求，`AudioSystem`反过来使用 SPI 对象来处理这些查询和服务请求。

新音频服务的存在对用户和应用程序员可能是完全透明的。所有应用程序引用都通过`javax.sound.sampled`包的标准对象，主要是`AudioSystem`，新服务可能提供的特殊处理通常是完全隐藏的。

在本讨论中，我们将继续使用类似`AcmeMixer`和`AcmeMixerProvider`的名称来指代新的 SPI 子类。

## 提供音频文件写入服务

让我们从`AudioFileWriter`开始，这是较简单的 SPI 类之一。

实现`AudioFileWriter`方法的子类必须提供一组方法的实现，以处理关于类支持的文件格式和文件类型的查询，以及提供实际将提供的音频数据流写入`File`或`OutputStream`的方法。

`AudioFileWriter`包括两个在基类中具有具体实现的方法：

```java
boolean isFileTypeSupported(AudioFileFormat.Type fileType) 
boolean isFileTypeSupported(AudioFileFormat.Type fileType, AudioInputStream stream) 

```

这些方法中的第一个方法通知调用者，此文件写入器是否可以写入指定类型的声音文件。这个方法是一个一般性的查询，如果文件写入器可以写入那种类型的文件，它将返回`true`，假设文件写入器被提供适当的音频数据。然而，写入文件的能力可能取决于传递给文件写入器的特定音频数据的格式。文件写入器可能不支持每种音频数据格式，或者约束可能由文件格式本身施加。（并非所有类型的音频数据都可以写入所有类型的声音文件。）因此，第二个方法更具体，询问特定的`AudioInputStream`是否可以写入特定类型的文件。

通常情况下，您不需要覆盖这两个具体方法。每个方法只是调用两个其他查询方法之一并遍历返回的结果的包装器。这两个其他查询方法是抽象的，因此需要在子类中实现：

```java
abstract AudioFileFormat.Type[] getAudioFileTypes() 
abstract AudioFileFormat.Type[] getAudioFileTypes(AudioInputStream stream) 

```

这些方法直接对应于前两个方法。每个方法返回所有支持的文件类型的数组-在第一个方法的情况下，通常是所有一般支持的，在第二个方法的情况下，是特定音频流支持的所有文件类型。第一个方法的典型实现可能简单地返回一个由文件写入器构造函数初始化的数组。第二个方法的实现可能测试流的`AudioFormat`对象，以查看请求的文件类型是否支持该数据格式。

`AudioFileWriter`的最后两个方法执行实际的文件写入工作：

```java
abstract int write(AudioInputStream stream, 
     AudioFileFormat.Type fileType, java.io.File out) 
abstract int write(AudioInputStream stream, 
     AudioFileFormat.Type fileType, java.io.OutputStream out) 

```

这些方法将代表音频数据的字节流写入到第三个参数指定的流或文件中。如何完成这项工作的细节取决于指定类型文件的结构。`write`方法必须按照该格式声音文件的规定方式写入文件的头部和音频数据（无论是标准类型的声音文件还是新的、可能是专有的类型）。

## 提供音频文件读取服务

`AudioFileReader`类由六个抽象方法组成，您的子类需要实现这些方法-实际上，两个不同的重载方法，每个方法都可以接受`File`、`URL`或`InputStream`参数。这两个重载方法中的第一个接受有关指定文件格式的查询：

```java
abstract AudioFileFormat getAudioFileFormat(java.io.File file) 
abstract AudioFileFormat getAudioFileFormat(java.io.InputStream stream) 
abstract AudioFileFormat getAudioFileFormat(java.net.URL url) 

```

`getAudioFileFormat`方法的典型实现读取并解析声音文件的头部，以确定其文件格式。查看`AudioFileFormat`类的描述，了解需要从头部读取哪些字段，并参考特定文件类型的规范，以了解如何解析头部。

因为调用者将流作为参数提供给此方法，希望该方法不改变流，文件读取器通常应该从标记流开始。在读取到头部结束后，应该将流重置到其原始位置。

另一个重载的`AudioFileReader`方法提供文件读取服务，通过返回一个`AudioInputStream`，从中可以读取文件的音频数据：

```java
abstract AudioInputStream getAudioInputStream(java.io.File file) 
abstract AudioInputStream getAudioInputStream(java.io.InputStream stream) 
abstract AudioInputStream getAudioInputStream(java.net.URL url) 

```

通常，`getAudioInputStream`的实现返回一个绕到文件数据块（在头部之后）开头的`AudioInputStream`，准备好进行读取。然而，可以想象，文件读取器返回的`AudioInputStream`可能表示一种从文件中包含的内容解码出来的数据流。重要的是，该方法返回一个格式化的流，从中可以读取文件中包含的音频数据。返回的`AudioInputStream`对象中封装的`AudioFormat`将告知调用者有关流的数据格式，通常情况下，但不一定是文件本身的数据格式。

通常，返回的流是`AudioInputStream`的一个实例；您不太可能需要对`AudioInputStream`进行子类化。

## 提供格式转换服务

`FormatConversionProvider`子类将具有一个音频数据格式的`AudioInputStream`转换为具有另一种格式的`AudioInputStream`。前者（输入）流被称为*源*流，后者（输出）流被称为*目标*流。回想一下，`AudioInputStream`包含一个`AudioFormat`，而`AudioFormat`又包含一种特定类型的数据编码，由`AudioFormat.Encoding`对象表示。源流中的格式和编码称为源格式和源编码，目标流中的格式和编码同样被称为目标格式和目标编码。

转换工作是在`FormatConversionProvider`的重载抽象方法`getAudioInputStream`中执行的。该类还具有用于了解所有支持的目标和源格式和编码的抽象查询方法。有具体的包装方法用于查询特定的转换。

`getAudioInputStream`的两个变体是：

```java
abstract AudioInputStream getAudioInputStream(AudioFormat.Encoding targetEncoding, 
     AudioInputStream sourceStream) 

```

和

```java
abstract AudioInputStream getAudioInputStream(AudioFormat targetFormat, 
     AudioInputStream sourceStream) 

```

这些根据调用者是指定完整目标格式还是只是格式的编码而有所不同的第一个参数。

`getAudioInputStream`的典型实现通过返回一个围绕原始（源）`AudioInputStream`的新的`AudioInputStream`子类来工作，并在调用`read`方法时对其数据应用数据格式转换。例如，考虑一个名为`AcmeCodec`的新`FormatConversionProvider`子类的情况，它与一个名为`AcmeCodecStream`的新`AudioInputStream`子类一起工作。

`AcmeCodec`的第二个`getAudioInputStream`方法的实现可能是：

```java
public AudioInputStream getAudioInputStream
      (AudioFormat outputFormat, AudioInputStream stream) {
        AudioInputStream cs = null;
        AudioFormat inputFormat = stream.getFormat();
        if (inputFormat.matches(outputFormat) ) {
            cs = stream;
        } else {
            cs = (AudioInputStream)
                (new AcmeCodecStream(stream, outputFormat));
            tempBuffer = new byte[tempBufferSize];
        }
        return cs;
    }

```

实际的格式转换发生在返回的`AcmeCodecStream`的新`read`方法中，它是`AudioInputStream`的子类。同样，访问这个返回的`AcmeCodecStream`的应用程序只需将其视为`AudioInputStream`进行操作，而不需要了解其实现的细节。

`FormatConversionProvider`的其他方法都允许查询对象支持的输入和输出编码和格式。以下四个方法是抽象的，需要被实现：

```java
abstract AudioFormat.Encoding[] getSourceEncodings() 
abstract AudioFormat.Encoding[] getTargetEncodings() 
abstract AudioFormat.Encoding[] getTargetEncodings(
    AudioFormat sourceFormat) 
abstract  AudioFormat[] getTargetFormats(
    AudioFormat.Encoding targetEncoding, 
    AudioFormat sourceFormat) 

```

与上面讨论的`AudioFileReader`类的查询方法一样，这些查询通常通过检查对象的私有数据，并且对于后两种方法，将它们与参数进行比较来处理。

剩下的四个`FormatConversionProvider`方法是具体的，通常不需要被重写：

```java
boolean isConversionSupported(
    AudioFormat.Encoding targetEncoding,
    AudioFormat sourceFormat) 
boolean isConversionSupported(AudioFormat targetFormat, 
    AudioFormat sourceFormat) 
boolean isSourceEncodingSupported(
    AudioFormat.Encoding sourceEncoding) 
boolean isTargetEncodingSupported(
    AudioFormat.Encoding targetEncoding) 

```

与`AudioFileWriter.isFileTypeSupported()`类似，这些方法的默认实现本质上是调用其他查询方法之一并遍历返回的结果的包装器。

## 提供新类型的混音器

正如其名称所示，`MixerProvider`提供混音器的实例。每个具体的`MixerProvider`子类都充当应用程序使用的`Mixer`对象的工厂。当然，只有在定义一个或多个新的`Mixer`接口的实现时，定义新的`MixerProvider`才有意义。就像上面的`FormatConversionProvider`示例中，我们的`getAudioInputStream`方法返回了一个执行转换的`AudioInputStream`子类一样，我们的新类`AcmeMixerProvider`有一个`getMixer`方法，返回实现`Mixer`接口的另一个新类的实例。我们将后者称为`AcmeMixer`。特别是如果混音器是硬件实现的，提供者可能仅支持所请求设备的一个静态实例。如果是这样，它应该在每次调用`getMixer`时返回这个静态实例。

由于`AcmeMixer`支持`Mixer`接口，应用程序不需要任何额外的信息来访问其基本功能。然而，如果`AcmeMixer`支持`Mixer`接口中未定义的功能，并且供应商希望使这些扩展功能对应用程序可访问，那么混音器当然应该被定义为一个公共类，具有额外的、有文档记录的公共方法，以便希望利用这些扩展功能的程序可以导入`AcmeMixer`并将`getMixer`返回的对象转换为这种类型。

另外两种`MixerProvider`的方法是：

```java
abstract Mixer.Info[] getMixerInfo() 

```

和

```java
boolean isMixerSupported(Mixer.Info info) 

```

这些方法允许音频系统确定这个特定的提供者类是否可以提供应用程序需要的设备。换句话说，`AudioSystem`对象可以迭代所有已安装的`MixerProviders`，看看哪些，如果有的话，可以提供应用程序请求的`AudioSystem`的设备。`getMixerInfo`方法返回一个包含有关此提供程序对象提供的混音器类型信息的对象数组。系统可以将这些信息对象与其他提供程序的信息一起传递给应用程序。

一个`MixerProvider`可以提供多种类型的混音器。当系统调用`MixerProvider`的`getMixerInfo`方法时，它会得到一个信息对象列表，标识此提供程序支持的不同类型的混音器。然后系统可以调用`MixerProvider.getMixer(Mixer.Info)`来获取每个感兴趣的混音器。

你的子类需要实现`getMixerInfo`，因为它是抽象的。`isMixerSupported`方法是具体的，通常不需要被覆盖。默认实现只是将提供的`Mixer.Info`与`getMixerInfo`返回的数组中的每一个进行比较。
