# 如何编写表模型监听器

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/events/tablemodellistener.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/tablemodellistener.html)

在实例化时，每个`JTable`对象都传递一个管理其显示数据的表模型对象。如果没有指定自定义的`TableModel`对象，默认情况下，`JTable`对象继承一个`DefaultTable`对象，但默认情况下，此模型仅管理字符串。要处理对象、执行计算或从数据库或其他程序检索数据，必须设计自己的自定义`TableModel`对象，该对象实现`TableModel`接口。详细信息请参见创建表模型。

要检测表模型对象管理的数据的更改，`JTable`类需要实现`TableModelListener`接口，调用`addTableModelListener()`来捕获事件，然后重写`tableChanged()`来响应监听器事件。详细信息请参见监听数据更改。

## 表模型监听器 API

TableModelListener 接口

*因为`TableModelListener`只有一个方法，所以没有相应的适配器类。*

| 方法 | 目的 |
| --- | --- |
| [tableChanged(TableModelEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableModelListener.html#tableChanged-javax.swing.event.TableModelEvent-) | 当表的结构或数据发生更改时调用。 |

TableModelEvent API

| 方法 | 目的 |
| --- | --- |
| [Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)（*在`java.util.EventObject`中*） | 返回触发事件的对象。 |
| [int getFirstRow()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableModelEvent.html#getFirstRow--) | 返回更改的第一行的索引。`TableModelEvent.HEADER_ROW`指定表头。 |
| [int getLastRow()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableModelEvent.html#getLastRow--) | 更改的最后一行。同样，`HEADER_ROW`是可能的值。 |
| [int getColumn()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableModelEvent.html#getColumn--) | 返回更改的列的索引。常量`TableModelEvent.ALL_COLUMNS`指定所有列可能已更改。 |
| [int getType()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableModelEvent.html#getType--) | 更改的单元格发生了什么。返回值是以下之一：`TableModelEvent.INSERT`、`TableModelEvent.DELETE`或`TableModelEvent.UPDATE`。 |
