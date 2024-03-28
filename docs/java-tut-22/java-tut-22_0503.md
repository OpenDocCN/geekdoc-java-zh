# 如何编写树选择监听器

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/events/treeselectionlistener.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/treeselectionlistener.html)

要检测用户何时在 树 中选择节点，需要注册树选择监听器。以下是一个示例，取自 响应节点选择 中讨论的 `TreeDemo` 示例，用于检测一次最多可以选择一个节点的树中的节点选择：

```java
tree.addTreeSelectionListener(new TreeSelectionListener() {
    public void valueChanged(TreeSelectionEvent e) {
        DefaultMutableTreeNode node = (DefaultMutableTreeNode)
                           tree.getLastSelectedPathComponent();

   */* if nothing is selected */* 
        if (node == null) return;

   */* retrieve the node that was selected */* 
        Object nodeInfo = node.getUserObject();
        ...
   * /* React to the node selection. */*
        ...
    }
});

```

要指定树应支持单选，程序使用以下代码：

```java
tree.getSelectionModel().setSelectionMode
        (TreeSelectionModel.SINGLE_TREE_SELECTION);

```

`TreeSelectionModel` 接口定义了三个选择模式的值：

`DISCONTIGUOUS_TREE_SELECTION`

这是默认树选择模型的默认模式。使用此模式，可以选择任意组合的节点。

`SINGLE_TREE_SELECTION`

这是前面示例中使用的模式。一次最多只能选择一个节点。

`CONTIGUOUS_TREE_SELECTION`

使用此模式，只能选择相邻行中的节点。

## Tree Selection Listener API

TreeSelectionListener 接口

*因为 `TreeSelectionListener` 只有一个方法，所以没有相应的适配器类。*

| 方法 | 目的 |
| --- | --- |
| [valueChanged(TreeSelectionEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionListener.html#valueChanged-javax.swing.event.TreeSelectionEvent-) | 每当选择更改时调用。 |

TreeSelectionEvent API

| 方法 | 目的 |
| --- | --- |
| [Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--) (*在 `java.util.EventObject` 中*) | 返回触发事件的对象。 |
| [TreePath getNewLeadSelectionPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#getNewLeadSelectionPath--) | 返回当前主导路径。 |
| [TreePath getOldLeadSelectionPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#getOldLeadSelectionPath--) | 返回先前的主导路径。 |
| [TreePath getPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#getPath--) | 返回第一个路径元素。 |
| [TreePath[] getPaths()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#getPaths--) | 返回已添加或移除的路径。 |
| [boolean isAddedPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#isAddedPath--) | 如果第一个路径元素已添加到选择中，则返回 true。如果第一个路径已从选择中移除，则返回 false。 |
| [boolean isAddedPath(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#isAddedPath-int-) | 如果指定索引的路径已添加到选择中，则返回 true。 |
| [boolean isAddedPath(TreePath)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeSelectionEvent.html#isAddedPath-javax.swing.tree.TreePath-) | 如果指定的路径已添加到选择中，则返回 true。 |
| [Object getLastSelectedPathComponent()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#getLastSelectedPathComponent--) | 返回当前选择中第一个节点中的最后路径组件。 |
| [TreePath getLeadSelectionPath()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#getLeadSelectionPath--) (*在`JTree`中*) | 返回当前主导路径。 |

## 使用树选择监听器的示例

以下表格列出了使用树选择监听器的示例。

| 示例 | 描述位置 | 注释 |
| --- | --- | --- |
| `TreeDemo`  | 如何使用树 | 树监听器通过显示适当的 HTML 文档来响应节点点击。 |
