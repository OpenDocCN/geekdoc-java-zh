# 保护块

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/guardmeth.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/guardmeth.html)

线程经常需要协调它们的动作。最常见的协调习语是*保护块*。这样的块开始于轮询一个条件，该条件必须在块可以继续之前为真。为了正确执行此操作，需要遵循一些步骤。

假设，例如`guardedJoy`是一个方法，必须在另一个线程设置共享变量`joy`之前才能继续。这样的方法理论上可以简单地循环，直到条件满足，但是该循环是浪费的，因为它在等待时持续执行。

```java
public void guardedJoy() {
    // Simple loop guard. Wastes
    // processor time. Don't do this!
    while(!joy) {}
    System.out.println("Joy has been achieved!");
}

```

一个更有效的保护块调用[`Object.wait`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#wait--)来挂起当前线程。调用`wait`不会返回，直到另一个线程发出通知，表明可能发生了某个特殊事件，尽管不一定是该线程正在等待的事件：

```java
public synchronized void guardedJoy() {
    // This guard only loops once for each special event, which may not
    // be the event we're waiting for.
    while(!joy) {
        try {
            wait();
        } catch (InterruptedException e) {}
    }
    System.out.println("Joy and efficiency have been achieved!");
}

```

* * *

**注意：**始终在测试等待的条件的循环中调用`wait`。不要假设中断是为了您正在等待的特定条件，或者该条件仍然为真。

* * *

像许多暂停执行的方法一样，`wait`可能会抛出`InterruptedException`。在这个例子中，我们可以忽略这个异常，我们只关心`joy`的值。

为什么这个`guardedJoy`的版本是同步的？假设`d`是我们用来调用`wait`的对象。当一个线程调用`d.wait`时，它必须拥有`d`的内在锁，否则会抛出错误。在同步方法中调用`wait`是获取内在锁的简单方法。

当调用`wait`时，线程释放锁并暂停执行。在将来的某个时间，另一个线程将获得相同的锁并调用[`Object.notifyAll`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notifyAll--)，通知所有等待该锁的线程发生了重要事件：

```java
public synchronized notifyJoy() {
    joy = true;
    notifyAll();
}

```

第二个线程释放锁后一段时间，第一个线程重新获取锁，并通过从`wait`调用返回来恢复执行。

* * *

**注意：**还有第二种通知方法，`notify`，它唤醒单个线程。因为`notify`不允许您指定被唤醒的线程，所以它只在大规模并行应用程序中有用，即具有大量线程的程序，所有线程都在做类似的工作。在这种应用程序中，您不关心哪个线程被唤醒。

* * *

让我们使用保护块来创建一个*生产者-消费者*应用程序。这种应用程序在两个线程之间共享数据：*生产者*创建数据，*消费者*对其进行处理。这两个线程使用共享对象进行通信。协调是必不可少的：消费者线程在生产者线程交付数据之前不得尝试检索数据，生产者线程在消费者尚未检索旧数据之前不得尝试交付新数据。

在这个例子中，数据是一系列文本消息，通过一个类型为``Drop``的对象共享：

```java

public class Drop {
    // Message sent from producer
    // to consumer.
    private String message;
    // True if consumer should wait
    // for producer to send message,
    // false if producer should wait for
    // consumer to retrieve message.
    private boolean empty = true;

    public synchronized String take() {
        // Wait until message is
        // available.
        while (empty) {
            try {
                wait();
            } catch (InterruptedException e) {}
        }
        // Toggle status.
        empty = true;
        // Notify producer that
        // status has changed.
        notifyAll();
        return message;
    }

    public synchronized void put(String message) {
        // Wait until message has
        // been retrieved.
        while (!empty) {
            try { 
                wait();
            } catch (InterruptedException e) {}
        }
        // Toggle status.
        empty = false;
        // Store message.
        this.message = message;
        // Notify consumer that status
        // has changed.
        notifyAll();
    }
}

```

生产者线程，在``Producer``中定义，发送一系列熟悉的消息。字符串"DONE"表示所有消息都已发送。为了模拟真实应用程序的不可预测性，生产者线程在消息之间暂停一段随机时间。

```java

import java.util.Random;

public class Producer implements Runnable {
    private Drop drop;

    public Producer(Drop drop) {
        this.drop = drop;
    }

    public void run() {
        String importantInfo[] = {
            "Mares eat oats",
            "Does eat oats",
            "Little lambs eat ivy",
            "A kid will eat ivy too"
        };
        Random random = new Random();

        for (int i = 0;
             i < importantInfo.length;
             i++) {
            drop.put(importantInfo[i]);
            try {
                Thread.sleep(random.nextInt(5000));
            } catch (InterruptedException e) {}
        }
        drop.put("DONE");
    }
}

```

消费者线程，在``Consumer``中定义，简单地检索消息并打印出来，直到检索到"DONE"字符串为止。该线程还会暂停一段随机时间。

```java

import java.util.Random;

public class Consumer implements Runnable {
    private Drop drop;

    public Consumer(Drop drop) {
        this.drop = drop;
    }

    public void run() {
        Random random = new Random();
        for (String message = drop.take();
             ! message.equals("DONE");
             message = drop.take()) {
            System.out.format("MESSAGE RECEIVED: %s%n", message);
            try {
                Thread.sleep(random.nextInt(5000));
            } catch (InterruptedException e) {}
        }
    }
}

```

最后，这是主线程，在``ProducerConsumerExample``中定义，启动生产者和消费者线程。

```java

public class ProducerConsumerExample {
    public static void main(String[] args) {
        Drop drop = new Drop();
        (new Thread(new Producer(drop))).start();
        (new Thread(new Consumer(drop))).start();
    }
}

```

* * *

**注意：** `Drop` 类是为了演示受保护的代码块而编写的。在尝试编写自己的数据共享对象之前，请查看 Java 集合框架中的现有数据结构，以避免重复造轮子。有关更多信息，请参考问题和练习部分。

* * *
