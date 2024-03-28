# 队列接口

> 原文：[`docs.oracle.com/javase/tutorial/collections/interfaces/queue.html`](https://docs.oracle.com/javase/tutorial/collections/interfaces/queue.html)

一个[`Queue`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)是在处理之前保存元素的集合。除了基本的 `Collection` 操作外，队列还提供额外的插入、移除和检查操作。`Queue` 接口如下。

```java
public interface Queue<E> extends Collection<E> {
    E element();
    boolean offer(E e);
    E peek();
    E poll();
    E remove();
}

```

每个 `Queue` 方法都有两种形式：（1）如果操作失败，则抛出异常，（2）如果操作失败，则返回特殊值（根据操作的不同，可能是 `null` 或 `false`）。接口的常规结构如下表所示。

**队列接口结构**

| 操作类型 | 抛出异常 | 返回特殊值 |
| --- | --- | --- |
| 插入 | `add(e)` | `offer(e)` |
| 移除 | `remove()` | `poll()` |
| 检查 | `element()` | `peek()` |

队列通常按照 FIFO（先进先出）的方式排序元素，但不一定如此。其中的例外是优先队列，它根据元素的值排序 —— 详见对象排序部分）。无论使用何种排序方式，队列的头部元素都是通过调用 `remove` 或 `poll` 方法将被移除的元素。在 FIFO 队列中，所有新元素都插入到队列的尾部。其他类型的队列可能使用不同的放置规则。每个 `Queue` 实现必须指定其排序属性。

一个 `Queue` 实现可能限制其持有的元素数量；这样的队列称为*有界*。`java.util.concurrent` 中的一些 `Queue` 实现是有界的，但 `java.util` 中的实现则不是。

`add` 方法是 `Queue` 从 `Collection` 继承的方法，除非违反队列的容量限制，否则会插入一个元素，否则会抛出 `IllegalStateException` 异常。`offer` 方法仅用于有界队列，与 `add` 的区别仅在于插入元素失败时返回 `false`。

`remove` 和 `poll` 方法都会移除并返回队列的头部。具体移除哪个元素取决于队列的排序策略。当队列为空时，`remove` 和 `poll` 方法的行为有所不同。在这种情况下，`remove` 会抛出 `NoSuchElementException` 异常，而 `poll` 则返回 `null`。

`element` 和 `peek` 方法会返回队列的头部元素，但不会将其移除。它们与 `remove` 和 `poll` 的区别与之相同：如果队列为空，`element` 会抛出 `NoSuchElementException` 异常，而 `peek` 则返回 `null`。

`Queue`实现通常不允许插入`null`元素。`LinkedList`实现是一个例外，它被改装为实现`Queue`。出于历史原因，它允许`null`元素，但你应该避免利用这一点，因为`null`被`poll`和`peek`方法用作特殊返回值。

队列实现通常不定义基于元素的`equals`和`hashCode`方法的版本，而是从`Object`继承基于身份的版本。

`Queue`接口不定义阻塞队列方法，这在并发编程中很常见。这些方法，等待元素出现或空间变得可用，被定义在接口[`java.util.concurrent.BlockingQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html)中，它扩展了`Queue`。

在下面的示例程序中，使用队列实现倒计时器。队列预先加载了从命令行指定的数字到零的所有整数值，按降序排列。然后，这些值按一秒的间隔从队列中移除并打印。该程序是人为的，因为在不使用队列的情况下做同样的事情更自然，但它展示了使用队列在后续处理之前存储元素的用法。

```java
import java.util.*;

public class Countdown {
    public static void main(String[] args) throws InterruptedException {
        int time = Integer.parseInt(args[0]);
        Queue<Integer> queue = new LinkedList<Integer>();

        for (int i = time; i >= 0; i--)
            queue.add(i);

        while (!queue.isEmpty()) {
            System.out.println(queue.remove());
            Thread.sleep(1000);
        }
    }
}

```

在下面的示例中，优先队列用于对一组元素进行排序。同样，这个程序是人为的，因为没有理由使用它来代替`Collections`提供的`sort`方法，但它展示了优先队列的行为。

```java
static <E> List<E> heapSort(Collection<E> c) {
    Queue<E> queue = new PriorityQueue<E>(c);
    List<E> result = new ArrayList<E>();

    while (!queue.isEmpty())
        result.add(queue.remove());

    return result;
}

```
