# System 中的其他方法

> 原文：[`docs.oracle.com/javase/tutorial/essential/environment/sysmisc.html`](https://docs.oracle.com/javase/tutorial/essential/environment/sysmisc.html)

本节描述了一些在前几节中未涵盖的`System`中的方法。

`arrayCopy` 方法有效地在数组之间复制数据。有关更多信息，请参考 Arrays 中的 Language Basics 课程。

[`currentTimeMillis`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#currentTimeMillis--) 和 [`nanoTime`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#nanoTime--) 方法在应用程序执行期间测量时间间隔很有用。要测量毫秒级时间间隔，需要在间隔开始和结束时分别调用`currentTimeMillis`，并将第一个返回值从第二个返回值中减去。类似地，连续调用`nanoTime`可以测量纳秒级时间间隔。

* * *

**注意：** `currentTimeMillis` 和 `nanoTime` 的准确性受操作系统提供的时间服务限制。不要假设`currentTimeMillis`精确到最近的毫秒，也不要假设`nanoTime`精确到最近的纳秒。此外，既不应该使用`currentTimeMillis`也不应该使用`nanoTime`来确定当前时间。应使用高级方法，如[`java.util.Calendar.getInstance`](https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html#getInstance--)。

* * *

[`exit`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#exit-int-) 方法导致 Java 虚拟机关闭，退出状态由参数指定。退出状态可供启动应用程序的进程使用。按照惯例，退出状态为`0`表示应用程序正常终止，而其他任何值都是错误代码。
