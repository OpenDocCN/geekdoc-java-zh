# 将诊断信息写入标准输出和错误流

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/stdout.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/stdout.html)

一个 Java 小程序可以将消息写入标准输出和标准错误流。在调试 Java 小程序时，将诊断信息写入标准输出可以是一个非常有价值的工具。

以下代码片段将消息写入标准输出流和标准错误流。

```java
*// Where instance variables are declared:*
boolean DEBUG = true;
// ...
*// Later, when we want to print some status:*
if (DEBUG) {
    try {
        // ...
        //some code that throws an exception
        System.out.
            println("Called someMethod(" + x + "," + y + ")");
    } catch (Exception e) {
        e.printStackTrace()
    }
}

```

检查 Java 控制台日志，查看写入标准输出流或标准错误流的消息。要将消息存储在日志文件中，请在 Java 控制面板中启用日志记录。消息将被写入用户主目录中的日志文件（例如，在 Windows 上，日志文件可能位于`C:\Documents and Settings\someuser\Application Data\Sun\Java\Deployment\log`）。

* * *

**注意：** 在发布小程序之前，请务必禁用所有调试输出。

* * *
