# 使用高级音序器功能

> 译文：[`docs.oracle.com/javase/tutorial/sound/MIDI-seq-adv.html`](https://docs.oracle.com/javase/tutorial/sound/MIDI-seq-adv.html)

到目前为止，我们专注于简单的 MIDI 数据播放和录制。本节将简要描述通过`Sequencer`接口和`Sequence`类的方法可用的一些更高级功能。

## 移动到序列中的任意位置

有两个`Sequencer`方法可以获取序列中音序器的当前位置。其中的第一个：

```java
long getTickPosition()

```

返回从序列开始测量的 MIDI 滴答位置。第二种方法：

```java
long getMicrosecondPosition()

```

返回当前位置的微秒数。此方法假定序列以存储在 MIDI 文件或`Sequence`中的默认速率播放。如果您按照下面描述的方式更改了播放速度，则它不会返回不同的值。

您也可以根据一个单位或另一个单位设置音序器的当前位置：

```java
void setTickPosition(long tick)

```

或

```java
void setMicrosecondPosition(long microsecond)

```

## 更改播放速度

如前所述，序列的速度由其速度确定，可以在序列的过程中变化。序列可以包含封装标准 MIDI 速度变化消息的事件。当音序器处理此类事件时，它会根据指定的速度更改播放速度。此外，您可以通过调用任何这些`Sequencer`方法来以编程方式更改速度：

```java
    public void setTempoInBPM(float bpm)
    public void setTempoInMPQ(float mpq)
    public void setTempoFactor(float factor)

```

这些方法中的前两种分别设置每分钟的节拍数或每个四分音符的微秒数的速度。速度将保持在指定值，直到再次调用其中一种方法，或者在序列中遇到速度变化事件，此时当前速度将被新指定的速度覆盖。

第三种方法，`setTempoFactor`，性质不同。它会按比例缩放音序器设置的任何速度（无论是通过速度变化事件还是通过上述前两种方法之一）。默认比例为 1.0（无变化）。尽管此方法会导致播放或录制速度比标称速度快或慢（除非因子为 1.0），但不会改变标称速度。换句话说，`getTempoInBPM`和`getTempoInMPQ`返回的速度值不受速度因子影响，尽管速度因子会影响实际播放或录制速率。此外，如果速度通过速度变化事件或前两种方法之一更改，它仍会按上次设置的速度因子进行缩放。但是，如果加载新序列，则速度因子将重置为 1.0。

请注意，当序列的分割类型为 SMPTE 类型之一而不是 PPQ 时，所有这些速度变化指令都无效。

## 静音或独奏序列中的单独轨道

对于音序器的用户来说，通常可以方便地关闭某些轨道，以更清楚地听到音乐中发生的情况。一个功能齐全的音序器程序允许用户在播放过程中选择哪些轨道应该发声。（更准确地说，由于音序器实际上不会自己发声，用户选择哪些轨道将为音序器产生的 MIDI 消息流做出贡献。）通常，每个轨道上都有两种类型的图形控件：一个*静音*按钮和一个*独奏*按钮。如果激活了静音按钮，那个轨道在任何情况下都不会发声，直到静音按钮被停用。独奏是一个不太为人知的功能。它大致相当于静音的反义词。如果任何轨道上的独奏按钮被激活，只有那些独奏按钮被激活的轨道才会发声。这个功能让用户可以快速试听少量轨道，而无需将所有其他轨道静音。静音按钮通常优先于独奏按钮：如果两者都被激活，那个轨道不会发声。

使用`Sequencer`方法，静音或独奏轨道（以及查询轨道当前的静音或独奏状态）是很容易实现的。假设我们已经获得了默认的`Sequencer`并且已经将序列数据加载到其中。要静音序列中的第五个轨道，可以按照以下步骤进行：

```java
    sequencer.setTrackMute(4, true);
    boolean muted = sequencer.getTrackMute(4);
    if (!muted) { 
        return;         // muting failed
    }

```

有几点需要注意上面的代码片段。首先，序列的轨道编号从 0 开始，以总轨道数减 1 结束。此外，`setTrackMute`的第二个参数是一个布尔值。如果为 true，则请求是将轨道静音；否则请求是取消静音指定的轨道。最后，为了测试静音是否生效，我们调用`Sequencer getTrackMute`方法，将要查询的轨道号传递给它。如果它返回`true`，正如我们在这种情况下所期望的那样，那么静音请求成功。如果返回`false`，则请求失败。

静音请求可能因各种原因而失败。例如，`setTrackMute`调用中指定的轨道号可能超过了总轨道数，或者音序器可能不支持静音。通过调用`getTrackMute`，我们可以确定我们的请求是成功还是失败。

顺便说一句，`getTrackMute`返回的布尔值确实可以告诉我们是否发生了失败，但它无法告诉我们失败的原因。我们可以测试看看失败是否是由于将无效的轨道号传递给`setTrackMute`方法引起的。为此，我们可以调用`Sequence`的`getTracks`方法，该方法返回一个包含序列中所有轨道的数组。如果在`setTrackMute`调用中指定的轨道号超过了此数组的长度，则我们知道我们指定了一个无效的轨道号。

如果静音请求成功，那么在我们的示例中，当序列播放时第五个轨道将不会发声，当前静音的任何其他轨道也不会发声。

独奏轨道的方法和技巧与静音非常相似。要独奏一个轨道，调用`Sequence`的`setTrackSolo`方法：

```java
void setTrackSolo(int track, boolean bSolo)

```

与`setTrackMute`相同，第一个参数指定基于零的轨道编号，第二个参数如果为`true`，则指定该轨道应处于独奏模式；否则该轨道不应处于独奏状态。

默认情况下，轨道既不静音也不独奏。

## 与其他 MIDI 设备同步

`Sequencer`有一个名为`Sequencer.SyncMode`的内部类。`SyncMode`对象代表 MIDI 序列器的时间概念如何与主设备或从设备同步的一种方式。如果序列器正在与主设备同步，序列器会根据来自主设备的某些 MIDI 消息调整其当前时间。如果序列器有一个从设备，序列器同样会发送 MIDI 消息来控制从设备的定时。

有三种预定义模式指定了序列器可能的主设备：`INTERNAL_CLOCK`、`MIDI_SYNC`和`MIDI_TIME_CODE`。后两种模式在序列器接收来自另一设备的 MIDI 消息时起作用。在这两种模式下，序列器的时间会根据系统实时定时时钟消息或 MIDI 时间码（MTC）消息进行重置。 （有关这些消息类型的更多信息，请参阅 MIDI 规范。）这两种模式也可以用作从模式，此时序列器会向其接收器发送相应类型的 MIDI 消息。第四种模式`NO_SYNC`用于指示序列器不应向其接收器发送定时信息。

通过调用带有支持的`SyncMode`对象作为参数的`setMasterSyncMode`方法，您可以指定序列器的定时如何受控。同样，`setSlaveSyncMode`方法确定序列器将向其接收器发送哪些定时信息。这些信息控制使用序列器作为主定时源的设备的定时。

## 指定特殊事件监听器

序列的每个轨道可以包含许多不同类型的`MidiEvents`。这些事件包括音符开和音符关消息、程序更改、控制更改和元事件。Java Sound API 为最后两种事件类型（控制更改事件和元事件）指定了“监听器”接口。您可以使用这些接口在播放序列时接收这些事件发生时的通知。

支持`ControllerEventListener`接口的对象可以在`Sequencer`处理特定控制变化消息时接收通知。控制变化消息是一种标准的 MIDI 消息类型，代表了 MIDI 控制器值的变化，比如音高弯曲轮或数据滑块。 （请参阅 MIDI 规范获取控制变化消息的完整列表。）当在序列播放过程中处理这样的消息时，消息指示任何设备（可能是合成器）从序列器接收数据以更新某些参数的值。该参数通常控制声音合成的某些方面，比如如果控制器是音高弯曲轮，则控制当前发声音符的音高。当录制序列时，控制变化消息意味着在创建消息的外部物理设备上移动了一个控制器，或者在软件中模拟了这样的移动。

这里是`ControllerEventListener`接口的使用方法。假设你已经开发了一个实现`ControllerEventListener`接口的类，意味着你的类包含以下方法：

```java
    void controlChange(ShortMessage msg)

```

假设你已经创建了一个类的实例，并将其赋给一个名为`myListener`的变量。如果你在程序的某个地方包含以下语句：

```java
    int[] controllersOfInterest = { 1, 2, 4 };
    sequencer.addControllerEventListener(myListener,
        controllersOfInterest);

```

那么你的类的`controlChange`方法将在`Sequencer`处理 MIDI 控制器编号为 1、2 或 4 的控制变化消息时被调用。换句话说，当`Sequencer`处理设置任何已注册控制器的值的请求时，`Sequencer`将调用你的类的`controlChange`方法。（请注意，将 MIDI 控制器编号分配给特定控制设备的详细信息在 MIDI 1.0 规范中有详细说明。）

`controlChange`方法接收一个包含受影响的控制器编号和控制器设置的新值的`ShortMessage`。你可以使用`ShortMessage.getData1`方法获取控制器编号，并使用`ShortMessage.getData2`方法获取控制器值的新设置。

另一种特殊事件监听器的类型由`MetaEventListener`接口定义。根据标准 MIDI 文件 1.0 规范，元消息是不在 MIDI 线协议中存在但可以嵌入到 MIDI 文件中的消息。它们对合成器没有意义，但可以被序列器解释。元消息包括指令（如变速命令）、歌词或其他文本以及其他指示（如音轨结束）。

`MetaEventListener`机制类似于`ControllerEventListener`。在任何需要在`Sequencer`处理`MetaMessage`时收到通知的类中实现`MetaEventListener`接口。这涉及向类中添加以下方法：

```java
void meta(MetaMessage msg)

```

通过将此类的实例作为参数传递给`Sequencer addMetaEventListener`方法来注册该类的实例：

```java
boolean b = sequencer.addMetaEventListener
        (myMetaListener);

```

这与`ControllerEventListener`接口所采取的方法略有不同，因为您必须注册以接收所有`MetaMessages`，而不仅仅是感兴趣的部分。如果顺序器在其序列中遇到`MetaMessage`，它将调用`myMetaListener.meta`，并将遇到的`MetaMessage`传递给它。`meta`方法可以调用其`MetaMessage`参数上的`getType`，以获取一个从 0 到 127 的整数，该整数表示消息类型，如标准 MIDI 文件 1.0 规范所定义。
