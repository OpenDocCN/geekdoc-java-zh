# 如何编写插入符监听器

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/events/caretlistener.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/caretlistener.html)

插入符事件发生在文本组件中的插入符（指示插入点的光标）移动或文本组件中的选择更改时。例如，当文本组件插入或删除文本时，文本组件的文档可以启动插入符事件。您可以使用`addCaretListener`方法将插入符监听器附加到任何`JTextComponent`子类的实例上。

* * *

**注意：**检测插入符更改的另一种方法是直接将监听器附加到插入符对象本身，而不是附加到管理插入符的文本组件。插入符会触发更改事件（*而不是*插入符事件），因此您需要编写一个更改监听器而不是插入符监听器。

这是一个名为`TextComponentDemo`的应用程序中的插入符事件处理代码：

```java
...
*//where initialization occurs*
CaretListenerLabel caretListenerLabel =
    new CaretListenerLabel("Caret Status");
...
textPane.addCaretListener(caretListenerLabel);
...
protected class CaretListenerLabel extends JLabel
                                   implements CaretListener
{
    ...
    //Might not be invoked from the event dispatching thread.
    public void caretUpdate(CaretEvent e) {
        displaySelectionInfo(e.getDot(), e.getMark());
    }

    //This method can be invoked from any thread.  It 
    //invokes the setText and modelToView methods, which 
    //must run in the event dispatching thread. We use
    //invokeLater to schedule the code for execution
    //in the event dispatching thread.
    protected void displaySelectionInfo(final int dot,
                                        final int mark) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                if (dot == mark) {  // no selection
                    ...
                }
            });
        }
    }
}

```

* * *

**注意：**`caretUpdate`方法不能保证在事件分派线程中调用。要在`caretUpdate`中使用更新 GUI 的任何方法，需要特殊处理以确保它们在事件分派线程上执行。您可以通过将代码包装在`Runnable`中并在该`Runnable`上调用`SwingUtilities.invokeLater`来实现这一点。

* * *

您可以在使用 Swing 组件示例索引中找到`TextComponentDemo`的源文件链接。有关程序的插入符监听器方面的讨论，请参阅监听插入符和选择更改中的文本组件功能。

## 插入符监听器 API

插入符监听器接口

*因为`CaretListener`只有一个方法，所以它没有相应的适配器类。*

| 方法 | 目的 |
| --- | --- |
| [caretUpdate(CaretEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/CaretListener.html#caretUpdate-javax.swing.event.CaretEvent-) | 当监听组件中的插入符移动或监听组件中的选择更改时调用。 |

插入符事件类

| 方法 | 目的 |
| --- | --- |
| [int getDot()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/CaretEvent.html#getDot--) | 返回插入符的当前位置。如果选择了文本，则插入符标记选择的一端。 |
| [int getMark()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/CaretEvent.html#getMark--) | 返回选择的另一端。如果没有选择任何内容，则此方法返回的值等于`getDot`返回的值。请注意，插入符不一定小于标记。 |
| [Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)（*在`java.util.EventObject`中*） | 返回触发事件的对象。 |

## 使用插入符监听器的示例

下表列出了使用插入符监听器的示例。

| 示例 | 描述位置 | 备注 |
| --- | --- | --- |
| `TextComponentDemo` | 监听插入符和选择更改 | 使用监听器标签显示插入符和选择状态。 |
