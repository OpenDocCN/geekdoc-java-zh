# 解决常见布局问题

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/layout/problems.html`](https://docs.oracle.com/javase/tutorial/uiswing/layout/problems.html)

* * *

**注意：** 本课程涵盖了手动编写布局代码，这可能具有挑战性。如果你不想学习布局管理的所有细节，可以选择使用`GroupLayout`布局管理器结合构建工具来布局你的 GUI。其中一种构建工具是 NetBeans IDE。否则，如果你想手动编码而不想使用`GroupLayout`，那么推荐使用`GridBagLayout`作为下一个最灵活和强大的布局管理器。

* * *

如果你有兴趣使用 JavaFX 来创建 GUI，请查看[使用 JavaFX 中的布局](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html)。

**问题：** 如何指定组件的确切尺寸？

+   一些较现代的布局管理器提供了覆盖组件设置的尺寸的方法。检查你正在使用的布局管理器是否允许你指定组件大小。

+   确保你真的需要设置组件的确切尺寸。每个 Swing 组件的首选尺寸都不同，取决于其使用的字体和外观。因此，通常没有必要指定 Swing 组件的确切尺寸。

+   如果组件不受布局管理器控制，可以通过调用`setSize`或`setBounds`方法来设置其大小。否则，你需要提供尺寸提示，然后确保你使用的布局管理器尊重尺寸提示。

+   如果你扩展了 Swing 组件类，可以通过重写组件的`getMinimumSize`、`getPreferredSize`和`getMaximumSize`方法来给出尺寸提示。这种方法的好处在于，每个`get*Xxxx*Size`方法可以通过调用`super.get*Xxxx*Size()`来获取组件的默认尺寸提示。然后在返回之前可以调整尺寸，如果有必要的话。这对于文本组件特别方便，其中你可能希望固定宽度，但高度由内容确定。然而，有时会在`GridBagLayout`和文本字段中遇到问题，如果容器的尺寸小于首选尺寸，则使用最小尺寸，这可能会导致文本字段大幅缩小。

+   另一种给出尺寸提示的方法是调用组件的`setMinimumSize`、`setPreferredSize`和`setMaximumSize`方法。

+   如果为已经可见的组件指定了新的尺寸提示，则需要在其上调用`revalidate`方法，以确保其包含层次结构再次布局。然后调用`repaint`方法。

* * *

**注意：** 无论您如何指定组件的大小，请确保组件的容器使用一个能够尊重组件请求大小的布局管理器。`FlowLayout` 和 `GridBagLayout` 布局管理器使用组件的首选大小（后者取决于您设置的约束条件），但 `BorderLayout` 和 `GridLayout` 通常不会。`BoxLayout` 布局管理器通常使用组件的首选大小（尽管组件可以更大），并且是少数几个尊重组件最大大小的布局管理器之一。

* * *

**问题：** 我添加了组件到容器后，组件没有显示出来。

+   在添加组件后，您需要调用 `revalidate` 和 `repaint` 才能使组件显示在容器中。

**问题：** 我的自定义组件尺寸太小了。

+   组件是否实现了 `getPreferredSize` 和 `getMinimumSize` 方法？如果是，它们是否返回了正确的值？

+   您是否使用了一个可以利用所有可用空间的布局管理器？请参阅选择布局管理器的提示以获取一些关于选择布局管理器并指定其使用特定组件的最大可用空间的提示。

如果您在此列表中找不到您的问题，请参阅解决常见组件问题。
