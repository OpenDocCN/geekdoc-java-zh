# 如何编写可撤销编辑监听器

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/events/undoableeditlistener.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/undoableeditlistener.html)

当组件上发生可撤销操作时，会触发可撤销编辑事件。目前，只有文本组件间接触发可撤销编辑事件。文本组件的文档会触发这些事件。对于文本组件，可撤销操作包括插入字符、删除字符和修改文本样式。程序通常监听可撤销编辑事件以帮助实现撤销和重做命令。

这是来自名为`TextComponentDemo`的应用程序的可撤销编辑事件处理代码。

```java
...
*//where initialization occurs*
*document*.addUndoableEditListener(new MyUndoableEditListener());
...
protected class MyUndoableEditListener implements UndoableEditListener {
    public void undoableEditHappened(UndoableEditEvent e) {
        //Remember the edit and update the menus
        undo.addEdit(e.getEdit());
        undoAction.updateUndoState();
        redoAction.updateRedoState();
    }
}  

```

您可以在使用 Swing 组件示例索引中找到`TextComponentDemo`的源文件链接。有关程序中可撤销编辑监听器方面的讨论，请参阅实现撤销和重做

## 可撤销编辑监听器 API

可撤销编辑监听器接口

*因为`UndoableEditListener`只有一个方法，所以没有相应的适配器类。*

| 方法 | 目的 |
| --- | --- |
| [undoableEditHappened(UndoableEditEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/UndoableEditListener.html#undoableEditHappened-javax.swing.event.UndoableEditEvent-) | 当监听的组件上发生可撤销事件时调用。 |

可撤销编辑事件类

| 方法 | 目的 |
| --- | --- |
| [Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--) (*在 `java.util.EventObject` 中*) | 返回触发事件的对象。 |
| [UndoableEdit getEdit()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/UndoableEditEvent.html#getEdit--) | 返回一个表示发生的编辑并包含有关撤消或重做编辑的信息和命令的[`UndoableEdit`](https://docs.oracle.com/javase/8/docs/api/javax/swing/undo/UndoableEdit.html)对象。 |

## 使用可撤销编辑监听器的示例

以下表格列出了使用可撤销编辑监听器的示例。

| 示例 | 描述位置 | 备注 |
| --- | --- | --- |
| `TextComponentDemo` | 实现撤销和重做 | 使用可撤销编辑监听器在文本窗格上实现撤销和重做。 |
