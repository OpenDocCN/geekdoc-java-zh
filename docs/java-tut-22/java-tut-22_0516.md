# 概要

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/painting/summary.html`](https://docs.oracle.com/javase/tutorial/uiswing/painting/summary.html)

+   在 Swing 中，绘制始于`paint`方法，然后调用`paintComponent`、`paintBorder`和`paintChildren`。当组件首次绘制、调整大小或在被另一个窗口隐藏后再次暴露时，系统将自动调用这些方法。

+   通过调用组件的`repaint`方法来实现程序化重绘；*不要*直接调用其`paintComponent`。调用`repaint`会导致绘制子系统采取必要步骤，以确保您的`paintComponent`方法在适当的时间被调用。

+   `repaint`的多参数版本允许您缩小组件的*剪辑矩形*（受绘制操作影响的屏幕部分），以使绘制更有效。我们在`moveSquare`方法中利用了这种技术，以避免重新绘制未发生变化的屏幕部分。还有一个无参数版本的方法，它将重新绘制组件的整个表面区域。

+   因为我们已经缩小了剪辑矩形，我们的`moveSquare`方法不止一次地调用`repaint`。第一次调用重新绘制了组件中方块*先前*所在的区域（继承的行为是用当前背景颜色填充区域）。第二次调用绘制了组件中方块*当前*所在的区域。

+   您可以在同一事件处理程序中多次调用`repaint`，但 Swing 将获取这些信息并在一次操作中重新绘制组件。

+   对于具有 UI Delegate 的组件，您应该将`Graphics`参数与`super.paintComponent(g)`这一行代码作为您的`paintComponent`覆盖中的第一行代码传递。如果不这样做，那么您的组件将负责手动绘制其背景。您可以通过注释掉该行并重新编译来进行实验，看看背景不再被绘制。

+   通过将新代码提取到一个单独的`RedSquare`类中，应用程序保持了面向对象的设计，这使得`MyPanel`类的`paintComponent`方法保持清晰。绘制仍然有效，因为我们通过调用其`paintSquare(Graphics g)`方法将`Graphics`对象传递给了红色方块。请记住，这个方法的名称是我们从头开始创建的；我们没有从 Swing API 的更高层次覆盖`paintSquare`。
