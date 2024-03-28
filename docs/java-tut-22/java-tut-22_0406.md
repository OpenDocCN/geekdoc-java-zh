# 初始线程

> 译文：[`docs.oracle.com/javase/tutorial/uiswing/concurrency/initial.html`](https://docs.oracle.com/javase/tutorial/uiswing/concurrency/initial.html)

每个程序都有一组应用程序逻辑开始的线程。在标准程序中，只有一个这样的线程：调用程序类的`main`方法的线程。在 applet 中，初始线程是构造 applet 对象并调用其`init`和`start`方法的线程；这些操作可能发生在单个线程上，也可能发生在两个或三个不同的线程上，这取决于 Java 平台的实现。在本课程中，我们称这些线程为*初始线程*。

在 Swing 程序中，初始线程没有太多事情要做。它们最重要的工作是创建一个初始化 GUI 的`Runnable`对象，并将该对象调度到事件分发线程上执行。一旦 GUI 创建完成，程序主要由 GUI 事件驱动，每个事件都会导致在事件分发线程上执行一个短任务。应用程序代码可以在事件分发线程（如果它们快速完成，以免干扰事件处理）或工作线程（用于长时间运行的任务）上调度额外的任务。

初始线程通过调用[`javax.swing.SwingUtilities.invokeLater`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingUtilities.html#invokeLater-java.lang.Runnable-)或[`javax.swing.SwingUtilities.invokeAndWait`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingUtilities.html#invokeAndWait-java.lang.Runnable-)来调度 GUI 创建任务。这两种方法都接受一个参数：定义新任务的`Runnable`。它们的唯一区别由它们的名称表示：`invokeLater`仅调度任务并返回；`invokeAndWait`在任务完成之前等待返回。

您可以在 Swing 教程中看到这种情况的示例：

```java
SwingUtilities.invokeLater(new Runnable() {
    public void run() {
        createAndShowGUI();
    }
});

```

在 applet 中，GUI 创建任务必须从`init`方法中使用`invokeAndWait`启动；否则，`init`可能在 GUI 创建之前返回，这可能会导致 Web 浏览器启动 applet 时出现问题。在任何其他类型的程序中，调度 GUI 创建任务通常是初始线程做的最后一件事情，因此无论是使用`invokeLater`还是`invokeAndWait`都无所谓。

为什么初始线程不直接创建 GUI 呢？因为几乎所有创建或与 Swing 组件交互的代码都必须在事件分发线程上运行。这个限制在下一节中进一步讨论。
