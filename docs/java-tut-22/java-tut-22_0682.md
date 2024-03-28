# 使用控件处理音频

> 原文：[`docs.oracle.com/javase/tutorial/sound/controls.html`](https://docs.oracle.com/javase/tutorial/sound/controls.html)

先前的部分已经讨论了如何播放或捕获音频样本。隐含的目标是尽可能忠实地传递样本，而不进行修改（除了可能将样本与其他音频线路的样本混合）。然而，有时您可能希望能够修改信号。用户可能希望声音听起来更响亮、更安静、更丰满、更具混响、音高更高或更低等。本页讨论了提供这些类型信号处理的 Java Sound API 功能。

有两种应用信号处理的方式：

+   您可以通过查询[`Control`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Control.html)对象并根据用户的需求设置控件来使用混音器或其组件线路支持的任何处理。混音器和线路通常支持的控件包括增益、声像和混响控件。

+   如果混音器或其线路提供的处理方式不符合您的需求，您的程序可以直接操作音频字节，根据需要进行操作。

本页更详细地讨论了第一种技术，因为第二种技术没有专门的 API。

## 控件介绍

混音器的某些或所有线路上可以有各种信号处理控件。例如，用于音频捕获的混音器可能具有带增益控制的输入端口，以及带增益和声像控制的目标数据线。用于音频播放的混音器可能在其源数据线上具有采样率控制。在每种情况下，所有控件都通过`Line`接口的方法访问。

因为`Mixer`接口扩展了`Line`，混音器本身可以具有自己的一组控件。这些控件可能作为主控件，影响所有混音器的源或目标线路。例如，混音器可能具有主增益控制，其分贝值会添加到其目标线路上各个增益控制的值中。

混音器的其他控件可能会影响一个特殊的线路，既不是源也不是目标，混音器在内部用于其处理。例如，全局混响控制可能选择应用于输入信号混合的混响类型，这个“湿”（混响）信号会在传递到混音器的目标线路之前混合回“干”信号中。

如果混音器或其任何线路具有控件，则您可能希望通过程序用户界面中的图形对象公开控件，以便用户可以根据需要调整音频特性。这些控件本身并不是图形化的；它们只是允许您检索和更改其设置。由您决定在程序中使用何种图形表示（滑块、按钮等）。

所有控件都作为抽象类`Control`的具体子类实现。许多典型的音频处理控件可以通过基于数据类型（如布尔值、枚举值或浮点数）的`Control`的抽象子类来描述。例如，布尔控件代表二进制状态控件，如静音或混响的开/关控件。另一方面，浮点控件非常适合表示连续可变控件，如声道、平衡或音量。

Java 音频 API 指定了以下`Control`的抽象子类：

+   [`BooleanControl`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/BooleanControl.html) — 代表二进制状态（真或假）控件。例如，静音、独奏和开/关开关都是`BooleanControls`的良好候选。

+   [`FloatControl`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/FloatControl.html) — 提供对一系列浮点值的控制的数据模型。例如，音量和声道是可以通过旋钮或滑块操作的`FloatControls`。

+   [`EnumControl`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/EnumControl.html) — 提供从一组对象中进行选择的选项。例如，您可以将用户界面中的一组按钮与`EnumControl`关联起来，以选择几个预设混响设置中的一个。

+   [`CompoundControl`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/CompoundControl.html) — 提供对一组相关项目的访问，其中每个项目本身都是`Control`子类的实例。`CompoundControls`代表多控件模块，如图形均衡器。（图形均衡器通常由一组滑块表示，每个滑块影响一个`FloatControl`。）

上述每个`Control`的子类都有适用于其基础数据类型的方法。大多数类包括设置和获取控件当前值的方法，获取控件标签等。

当然，每个类都有特定于它和类所代表的数据模型的方法。例如，`EnumControl`有一个方法让您获取其可能值的集合，而`FloatControl`允许您获取其最小和最大值，以及控件的精度（增量或步长）。

每个`Control`的子类都有一个对应的`Control.Type`子类，其中包括标识特定控件的静态实例。

以下表格显示了每个`Control`子类、其对应的`Control.Type`子类以及指示特定控件类型的静态实例：

| `Control` | `Control.Type` | `Control.Type` **实例** |
| --- | --- | --- |
| `BooleanControl` | `BooleanControl.Type` | `MUTE` – 线路静音状态 `APPLY_REVERB` – 混响开/关 |
| `CompoundControl` | `CompoundControl.Type` | (无) |
| `EnumControl` | `EnumControl.Type` | `REVERB` – 访问混响设置（每个都是 ReverbType 的实例） |

| `FloatControl` | `FloatControl.Type` | `AUX_RETURN` – 线路上的辅助返回增益 `AUX_SEND` – 线路上的辅助发送增益

`BALANCE` – 左右音量平衡

`MASTER_GAIN` – 线路上的总增益

`PAN` – 左右位置

`REVERB_RETURN` – 线路上的后混响增益

`REVERB_SEND` – 线路上的前混响增益

`SAMPLE_RATE` – 播放采样率

`VOLUME` – 线路上的音量 |

Java Sound API 的实现可以在其混音器和线路上提供任何或所有这些控件类型。它还可以提供 Java Sound API 中未定义的其他控件类型。这些控件类型可以通过这四个抽象子类的具体子类或不继承这四个抽象子类的其他 `Control` 子类来实现。应用程序可以查询每条线路以查找它支持的控件。

### 获取具有所需控件的线路

在许多情况下，应用程序将简单地显示该线路支持的任何控件。如果线路没有任何控件，那就算了。但是如果重要的是找到具有特定控件的线路呢？在这种情况下，您可以使用 `Line.Info` 来获取具有正确特征的线路，如之前在 获取所需类型的线路 中描述的。

例如，假设您希望有一个输入端口，让用户设置声音输入的音量。以下代码摘录显示了如何查询默认混音器以确定它是否具有所需的端口和控件：

```java
Port lineIn;
FloatControl volCtrl;
try {
  mixer = AudioSystem.getMixer(null);
  lineIn = (Port)mixer.getLine(Port.Info.LINE_IN);
  lineIn.open();
  volCtrl = (FloatControl) lineIn.getControl(

      FloatControl.Type.VOLUME);

  // Assuming getControl call succeeds, 
  // we now have our LINE_IN VOLUME control.
} catch (Exception e) {
  System.out.println("Failed trying to find LINE_IN"
    + " VOLUME control: exception = " + e);
}
if (volCtrl != null)
  // ...

```

### 从线路获取控件

一个需要在其用户界面中公开控件的应用程序可能只需查询可用的线路和控件，然后为感兴趣的每条线路上的每个控件显示适当的用户界面元素。在这种情况下，程序的唯一任务是为用户提供对控件的“处理”，而不是知道这些控件对音频信号做了什么。只要程序知道如何将线路的控件映射到用户界面元素，Java Sound API 的 `Mixer`、`Line` 和 `Control` 架构通常会处理其余部分。

例如，假设您的程序播放声音。您正在使用一个 `SourceDataLine`，如之前在 获取所需类型的线路 中描述的那样获取。您可以通过调用以下 `Line` 方法来访问线路的控件：

```java
Control[] getControls()

```

然后，对于返回的数组中的每个控件，您可以使用以下 `Control` 方法来获取控件的类型：

```java
Control.Type getType()

```

知道特定的`Control.Type`实例后，您的程序可以显示相应的用户界面元素。当然，为特定的`Control.Type`选择“相应的用户界面元素”取决于您的程序采取的方法。一方面，您可能会使用相同类型的元素来表示同一类别的所有`Control.Type`实例。这将需要您使用例如`Object.getClass`方法查询`Control.Type`实例的*类*。假设结果匹配了`BooleanControl.Type`。在这种情况下，您的程序可能会显示一个通用的复选框或切换按钮，但如果其类匹配了`FloatControl.Type`，那么您可能会显示一个图形滑块。

另一方面，您的程序可能会区分不同类型的控件，甚至是同一类别的控件，并为每个控件使用不同的用户界面元素。这将需要您测试`Control's getType`方法返回的*实例*。然后，例如，如果类型匹配了`BooleanControl.Type.APPLY_REVERB`，您的程序可能会显示一个复选框；而如果类型匹配了`BooleanControl.Type.MUTE`，您可能会显示一个切换按钮。

### 使用控件更改音频信号

现在您知道如何访问控件并确定其类型，本节将描述如何使用`Controls`来更改音频信号的各个方面。本节不涵盖所有可用的控件；相反，它提供了一些示例，以展示如何入门。这些示例包括：

+   控制线的静音状态

+   更改线路的音量

+   在各种混响预设中进行选择

假设您的程序已经访问了所有的混音器、它们的线路以及这些线路上的控件，并且它有一个数据结构来管理控件与其对应的用户界面元素之间的逻辑关联。那么，将用户对这些控件的操作转换为相应的`Control`方法就变得相当简单。

以下各小节描述了必须调用的一些方法，以影响特定控件的更改。

### 控制线的静音状态

控制任何线的静音状态只需调用以下`BooleanControl`方法：

```java
void setValue(boolean value)

```

（可以假设程序通过引用其控件管理数据结构知道，静音是`BooleanControl`的一个实例。）为了使通过线路传递的信号静音，程序调用上述方法，指定值为`true`。要关闭静音，允许信号通过线路流动，程序调用该方法并将参数设置为`false`。

### 更改线路的音量

假设您的程序将特定的图形滑块与特定线路的音量控制相关联。音量控制的值（即`FloatControl.Type.VOLUME`）是使用以下`FloatControl`方法设置的：

```java
void setValue(float newValue)

```

检测到用户移动滑块后，程序会获取滑块的当前值，并将其作为参数`newValue`传递给上述方法。这会改变通过拥有该控件的线路流动的信号的音量。

### 在各种混响预设中进行选择

假设我们的程序有一个带有类型为`EnumControl.Type.REVERB`的控件的混音器。调用`EnumControl`方法：

```java
java.lang.Objects[] getValues()

```

在该控件上会生成一个`ReverbType`对象数组。如果需要，可以使用以下`ReverbType`方法访问每个对象的特定参数设置：

```java
int getDecayTime() 
int getEarlyReflectionDelay() 
float getEarlyReflectionIntensity() 
int getLateReflectionDelay() 
float getLateReflectionIntensity() 

```

例如，如果一个程序只想要一个听起来像洞穴的混响设置，它可以迭代`ReverbType`对象，直到找到一个`getDecayTime`返回大于 2000 的值。有关这些方法的详细解释，包括代表性返回值表，请参阅`javax.sound.sampled.ReverbType`的 API 参考文档。

通常，程序会为`getValues`方法返回的数组中的每个`ReverbType`对象创建一个用户界面元素，例如单选按钮。当用户点击其中一个单选按钮时，程序会调用`EnumControl`方法。

```java
void setValue(java.lang.Object value) 

```

其中`value`设置为与新选择的按钮对应的`ReverbType`。通过拥有此`EnumControl`的线路发送的音频信号将根据构成控件当前`ReverbType`（即`setValue`方法的`value`参数中指定的特定`ReverbType`）的参数设置而产生混响效果。

因此，从我们应用程序的角度来看，让用户从一个混响预设（即 ReverbType）移动到另一个只是将`getValues`返回的数组的每个元素连接到不同的单选按钮。

### 直接操作音频数据

`Control` API 允许 Java Sound API 的实现或混音器的第三方提供商通过控件提供任意类型的信号处理。但是如果没有混音器提供所需类型的信号处理怎么办？这需要更多的工作，但您可能可以在程序中实现信号处理。因为 Java Sound API 允许您将音频数据作为字节数组访问，您可以以任何您选择的方式修改这些字节。

如果您正在处理传入的声音，您可以从`TargetDataLine`中读取字节，然后对其进行操作。一个算法上微不足道但可以产生引人入胜结果的例子是通过将其帧按相反顺序排列来倒放声音。这个微不足道的例子可能对您的程序没有太大用处，但有许多复杂的数字信号处理（DSP）技术可能更合适。一些例子包括均衡、动态范围压缩、峰值限制、时间拉伸或压缩，以及延迟、合唱、混响、失真等特效。

要播放处理过的声音，您可以将处理后的字节数组放入`SourceDataLine`或`Clip`中。当然，字节数组不一定需要来自现有的声音。您可以从头开始合成声音，尽管这需要一些声学知识或者访问声音合成函数。无论是处理还是合成，您可能需要查阅音频 DSP 教科书以获取您感兴趣的算法，或者将第三方信号处理函数库导入到您的程序中。对于合成声音的播放，考虑一下`javax.sound.midi`包中的`Synthesizer` API 是否符合您的需求。稍后在合成声音下您将了解更多关于`javax.sound.midi`的内容。
