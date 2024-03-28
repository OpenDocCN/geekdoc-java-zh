# 具有中间结果的任务

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/concurrency/interim.html`](https://docs.oracle.com/javase/tutorial/uiswing/concurrency/interim.html)

在后台任务仍在运行时提供中间结果通常是有用的。任务可以通过调用[`SwingWorker.publish`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#publish-V...-)来实现这一点。该方法接受可变数量的参数。每个参数必须是由`SwingWorker`的第二个类型参数指定的类型。

要收集`publish`提供的结果，重写[`SwingWorker.process`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#process-java.util.List-)。此方法将从事件分发线程中调用。通常会将多次调用`publish`的结果累积到单次调用`process`中。

让我们看看``Flipper.java``示例如何使用`publish`提供中间结果。单击启动按钮以使用[Java™ Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html)运行`Flipper`（[下载 JDK 7 或更高版本](http://www.oracle.com/technetwork/java/javase/downloads/index.html)）。或者，要自行编译和运行示例，请参考示例索引。

![启动 Flipper 示例](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/FlipperProject/Flipper.jnlp)

该程序通过在后台任务中生成一系列随机的`boolean`值来测试[`java.util.Random`](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html)的公平性。这相当于抛硬币；因此命名为`Flipper`。为了报告其结果，后台任务使用了类型为`FlipPair`的对象。

```java
private static class FlipPair {
    private final long heads, total;
    FlipPair(long heads, long total) {
        this.heads = heads;
        this.total = total;
    }
}

```

`heads`字段是随机值为`true`的次数；`total`字段是随机值的总数。

后台任务由`FlipTask`的一个实例表示：

```java
private class FlipTask extends SwingWorker<Void, FlipPair> {

```

由于任务不返回最终结果，第一个类型参数是什么并不重要；`Void`被用作占位符。任务在每次“抛硬币”后调用`publish`：

```java
@Override
protected Void doInBackground() {
    long heads = 0;
    long total = 0;
    Random random = new Random();
    while (!isCancelled()) {
        total++;
        if (random.nextBoolean()) {
            heads++;
        }
        publish(new FlipPair(heads, total));
    }
    return null;
}

```

（`isCancelled`方法将在下一节中讨论。）由于`publish`被非常频繁地调用，很可能在事件分发线程中调用`process`之前会积累很多`FlipPair`值；`process`只对每次报告的最后一个值感兴趣，用它来更新 GUI：

```java
protected void process(List<FlipPair> pairs) {
    FlipPair pair = pairs.get(pairs.size() - 1);
    headsText.setText(String.format("%d", pair.heads));
    totalText.setText(String.format("%d", pair.total));
    devText.setText(String.format("%.10g", 
            ((double) pair.heads)/((double) pair.total) - 0.5));
}

```

如果`Random`是公平的，那么`devText`中显示的值应该在`Flipper`运行时越来越接近 0。

* * *

**注意：** 在`Flipper`中使用的`setText`方法实际上符合其[规范](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setText-java.lang.String-)中定义的“线程安全”。这意味着我们可以在工作线程中直接设置文本字段，而不需要使用`publish`和`process`。为了提供`SwingWorker`中间结果的简单演示，我们选择忽略这一事实。

* * *
