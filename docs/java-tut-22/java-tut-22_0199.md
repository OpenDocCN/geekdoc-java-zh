# 定义和启动线程

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/runthread.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/runthread.html)

创建 `Thread` 实例的应用程序必须提供将在该线程中运行的代码。 有两种方法可以做到这一点：

+   *提供一个 `Runnable` 对象.* [`Runnable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Runnable.html) 接口定义了一个名为 `run` 的方法，用于包含在线程中执行的代码。 `Runnable` 对象被传递给 `Thread` 构造函数，就像 ``HelloRunnable`` 示例中那样：

    ```java

    public class HelloRunnable implements Runnable {

        public void run() {
            System.out.println("Hello from a thread!");
        }

        public static void main(String args[]) {
            (new Thread(new HelloRunnable())).start();
        }

    }

    ```

+   *子类 `Thread`.* `Thread` 类本身实现了 `Runnable`，尽管它的 `run` 方法什么也不做。 应用程序可以子类化 `Thread`，提供自己的 `run` 实现，就像 ``HelloThread`` 示例中那样：

    ```java

    public class HelloThread extends Thread {

        public void run() {
            System.out.println("Hello from a thread!");
        }

        public static void main(String args[]) {
            (new HelloThread()).start();
        }

    }

    ```

请注意，这两个示例都调用了 `Thread.start` 来启动新线程。

你应该使用哪种习语？第一个习语使用了一个 `Runnable` 对象，更通用，因为 `Runnable` 对象可以是 `Thread` 以外的类的子类。 第二个习语在简单应用程序中更容易使用，但受到任务类必须是 `Thread` 的后代的限制。 本课程重点介绍第一种方法，它将 `Runnable` 任务与执行任务的 `Thread` 对象分开。 这种方法不仅更灵活，而且适用于后面介绍的高级线程管理 API。

`Thread` 类定义了一些对线程管理有用的方法。 这些方法包括 `static` 方法，提供有关调用方法的线程的信息或影响其状态。 其他方法是从参与管理线程和 `Thread` 对象的其他线程调用的。 我们将在以下部分中检查其中一些方法。
