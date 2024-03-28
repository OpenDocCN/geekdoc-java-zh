# 布局管理的工作原理

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/layout/howLayoutWorks.html`](https://docs.oracle.com/javase/tutorial/uiswing/layout/howLayoutWorks.html)

* * *

**注意：** 本课程涵盖手动编写布局代码，这可能具有挑战性。如果您不想学习布局管理的所有细节，您可能更喜欢使用`GroupLayout`布局管理器结合构建工具来布局您的 GUI。其中一个构建工具是 NetBeans IDE。否则，如果您想手动编码而不想使用`GroupLayout`，那么建议使用`GridBagLayout`作为下一个最灵活和强大的布局管理器。

* * *

如果您有兴趣使用 JavaFX 创建 GUI，请参阅[JavaFX 中的布局](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html)。

这里是一个使用[`LayoutManager2`](https://docs.oracle.com/javase/8/docs/api/java/awt/LayoutManager2.html) 对容器进行布局管理的示例。

1.  布局管理器基本上执行两项任务：

    +   计算容器的最小/首选/最大大小。

    +   布局容器的子元素。

    布局管理器根据提供的约束、容器的属性（如插入）以及子元素的最小/首选/最大大小来执行此操作。如果子元素本身是一个容器，则使用其自身的布局管理器来获取其最小/首选/最大大小并进行布局。

1.  容器可以是*有效*的（即，`isValid()` 返回 true），也可以是*无效*的。要使容器有效，所有容器的子元素必须已经布局，并且也必须全部有效。[`Container.validate`](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#validate--) 方法可用于验证无效容器。此方法触发容器及其子容器沿组件层次结构向下的布局，并将此容器标记为有效。

1.  组件创建后，默认处于无效状态。[`Window.pack`](https://docs.oracle.com/javase/8/docs/api/java/awt/Window.html) 方法验证窗口，并首次布局窗口的组件层次结构。

最终结果是为了确定容器的最佳大小，系统确定了包含层次结构底部容器的大小。然后这些大小向上层次结构传播，最终确定容器的总大小。

如果组件的大小发生变化，例如更改字体后，必须调用该组件的`revalidate`和`repaint`方法来重新调整大小并重绘。`revalidate`和`repaint`都是线程安全的  您无需从事件分派线程调用它们。

当你在组件上调用`revalidate`时，请求会沿着包含层次结构传递，直到遇到一个容器，比如滚动窗格或顶层容器，这些容器不应受到组件调整大小的影响。（这是通过调用容器的`isValidateRoot`方法来确定的。）然后对容器进行布局，这会调整重新验证的组件的大小以及所有受影响组件的大小。
