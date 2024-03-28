# 分叉/合并

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html)

分叉/合并框架是`ExecutorService`接口的一种实现，可以帮助你充分利用多个处理器。它专为可以递归地分解为较小片段的工作而设计。目标是利用所有可用的处理能力来提高应用程序的性能。

与任何`ExecutorService`实现一样，分叉/合并框架将任务分配给线程池中的工作线程。分叉/合并框架的独特之处在于它使用*工作窃取*算法。工作线程如果没有任务可执行，可以从其他仍在忙碌的线程中窃取任务。

分叉/合并框架的核心是[`ForkJoinPool`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinPool.html)类，它是`AbstractExecutorService`类的扩展。`ForkJoinPool`实现了核心的工作窃取算法，并可以执行[`ForkJoinTask`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinTask.html)进程。

## 基本用法

使用分叉/合并框架的第一步是编写执行一部分工作的代码。你的代码应该类似于以下伪代码：

```java
if (my portion of the work is small enough)
  do the work directly
else
  split my work into two pieces
  invoke the two pieces and wait for the results

```

将这段代码封装在一个`ForkJoinTask`子类中，通常使用其中的一个更专门的类型，要么是[`RecursiveTask`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/RecursiveTask.html)（可以返回结果），要么是[`RecursiveAction`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/RecursiveAction.html)。

当你的`ForkJoinTask`子类准备就绪后，创建代表所有要完成工作的对象，并将其传递给`ForkJoinPool`实例的`invoke()`方法。

## 清晰的模糊

为了帮助你理解分叉/合并框架的工作原理，请考虑以下示例。假设你想要对图像进行模糊处理。原始*源*图像由一个整数数组表示，其中每个整数包含单个像素的颜色值。模糊后的*目标*图像也由一个与源图像大小相同的整数数组表示。

执行模糊操作是通过逐个像素地处理源数组来完成的。每个像素与其周围像素（红色、绿色和蓝色分量取平均值）进行平均，结果放入目标数组中。由于图像是一个大数组，这个过程可能需要很长时间。你可以利用多处理器系统上的并发处理，使用分叉/合并框架来实现算法。以下是一个可能的实现：

```java
public class ForkBlur extends RecursiveAction {
    private int[] mSource;
    private int mStart;
    private int mLength;
    private int[] mDestination;

    // Processing window size; should be odd.
    private int mBlurWidth = 15;

    public ForkBlur(int[] src, int start, int length, int[] dst) {
        mSource = src;
        mStart = start;
        mLength = length;
        mDestination = dst;
    }

    protected void computeDirectly() {
        int sidePixels = (mBlurWidth - 1) / 2;
        for (int index = mStart; index < mStart + mLength; index++) {
            // Calculate average.
            float rt = 0, gt = 0, bt = 0;
            for (int mi = -sidePixels; mi <= sidePixels; mi++) {
                int mindex = Math.min(Math.max(mi + index, 0),
                                    mSource.length - 1);
                int pixel = mSource[mindex];
                rt += (float)((pixel & 0x00ff0000) >> 16)
                      / mBlurWidth;
                gt += (float)((pixel & 0x0000ff00) >>  8)
                      / mBlurWidth;
                bt += (float)((pixel & 0x000000ff) >>  0)
                      / mBlurWidth;
            }

            // Reassemble destination pixel.
            int dpixel = (0xff000000     ) |
                   (((int)rt) << 16) |
                   (((int)gt) <<  8) |
                   (((int)bt) <<  0);
            mDestination[index] = dpixel;
        }
    }

  ...

```

现在你要实现抽象的`compute()`方法，该方法可以直接执行模糊操作，也可以将其拆分为两个较小的任务。一个简单的数组长度阈值有助于确定是执行工作还是拆分任务。

```java
protected static int sThreshold = 100000;

protected void compute() {
    if (mLength < sThreshold) {
        computeDirectly();
        return;
    }

    int split = mLength / 2;

    invokeAll(new ForkBlur(mSource, mStart, split, mDestination),
              new ForkBlur(mSource, mStart + split, mLength - split,
                           mDestination));
}

```

如果前面的方法在`RecursiveAction`类的子类中，那么设置任务在`ForkJoinPool`中运行就很简单，包括以下步骤：

1.  创建一个代表所有要完成工作的任务。

    ```java
    // source image pixels are in src
    // destination image pixels are in dst
    ForkBlur fb = new ForkBlur(src, 0, src.length, dst);

    ```

1.  创建将运行任务的`ForkJoinPool`。

    ```java
    ForkJoinPool pool = new ForkJoinPool();

    ```

1.  运行任务。

    ```java
    pool.invoke(fb);

    ```

要查看完整的源代码，包括一些额外的代码来创建目标图像文件，请参见``ForkBlur``示例。

## 标准实现

除了在多处理器系统上并行执行任务的自定义算法（例如前一节中的`ForkBlur.java`示例）中使用分支/合并框架之外，Java SE 中还有一些通用功能已经使用分支/合并框架实现。其中一种实现是在 Java SE 8 中引入的，被[`java.util.Arrays`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html)类用于其`parallelSort()`方法。这些方法类似于`sort()`，但通过分支/合并框架利用并发性能。在多处理器系统上运行时，大型数组的并行排序比顺序排序更快。然而，这些方法如何利用分支/合并框架超出了 Java 教程的范围。有关此信息，请参阅 Java API 文档。

另一个实现分支/合并框架的方法是使用`java.util.streams`包中的方法，该包是[Project Lambda](http://openjdk.java.net/projects/lambda/)的一部分，计划在 Java SE 8 发布中使用。更多信息，请参阅 Lambda 表达式部分。
