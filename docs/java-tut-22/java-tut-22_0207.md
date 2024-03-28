# 同步方法

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/syncmeth.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/syncmeth.html)

Java 编程语言提供了两种基本的同步习语：*同步方法*和*同步语句*。其中更复杂的同步语句将在下一节中描述。本节讨论的是同步方法。

要使方法同步，只需在其声明中添加`synchronized`关键字：

```java
public class SynchronizedCounter {
    private int c = 0;

    public synchronized void increment() {
        c++;
    }

    public synchronized void decrement() {
        c--;
    }

    public synchronized int value() {
        return c;
    }
}

```

如果`count`是`SynchronizedCounter`的一个实例，则使这些方法同步会产生两个效果：

+   首先，不可能让同一对象上的两次同步方法调用交错。当一个线程正在为对象执行同步方法时，所有调用同一对象的同步方法的其他线程都会被阻塞（暂停执行），直到第一个线程完成对象的操作。

+   其次，当一个同步方法退出时，它会自动与*同一对象的任何后续同步方法的调用*建立 happens-before 关系。这确保了对对象状态的更改对所有线程都是可见的。

请注意，构造函数不能被同步  使用`synchronized`关键字与构造函数是语法错误。同步构造函数没有意义，因为只有创建对象的线程在构造对象时应该访问它。

* * *

**警告：** 在构建一个将在多个线程之间共享的对象时，一定要非常小心，确保对象的引用不会过早“泄漏”。例如，假设你想要维护一个名为`instances`的`List`，其中包含类的每个实例。你可能会诱惑地在构造函数中添加以下行：

```java
instances.add(this);

```

但是其他线程可以使用`instances`来访问对象，而在对象构造完成之前。

* * *

同步方法为防止线程干扰和内存一致性错误提供了一种简单的策略：如果一个对象对多个线程可见，那么对该对象的变量的所有读取或写入都通过`synchronized`方法进行。 （一个重要的例外：`final`字段，在对象构造后无法修改，可以通过非同步方法安全地读取，一旦对象构造完成）这种策略是有效的，但在后面的课程中我们将看到它可能会出现 liveness 问题。
