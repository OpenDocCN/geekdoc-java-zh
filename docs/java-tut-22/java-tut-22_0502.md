# 如何编写树模型监听器

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/events/treemodellistener.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/treemodellistener.html)

通过实现树模型监听器，您可以检测到由树显示的数据何时发生更改。您可以使用树模型监听器来检测用户何时编辑树节点。所有通知都描述相对于树中的节点的更改。有关详细信息，请阅读动态更改树。

## 树模型监听器 API

树模型监听器接口

*`TreeModelListener`没有适配器类。*

| 方法 | 目的 |
| --- | --- |
| [treeNodesChanged(TreeModelEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelListener.html#treeNodesChanged-javax.swing.event.TreeModelEvent-) | 当一个或多个兄弟节点以某种方式发生更改时调用。 |
| [treeNodesInserted(TreeModelEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelListener.html#treeNodesInserted-javax.swing.event.TreeModelEvent-) | 在节点插入到树中后调用。 |
| [treeNodesRemoved(TreeModelEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelListener.html#treeNodesRemoved-javax.swing.event.TreeModelEvent-) | 在节点从树中移除后调用。 |
| [treeStructureChanged(TreeModelEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelListener.html#treeStructureChanged-javax.swing.event.TreeModelEvent-) | 在树的结构从当前节点开始发生重大变化后调用。此事件适用于与此节点连接的所有节点。 |

树模型事件 API

| 方法 | 目的 |
| --- | --- |
| [获取源对象](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--) (*在`java.util.EventObject`中*) | 返回触发事件的对象。 |
| [获取子索引](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelEvent.html#getChildIndices--) | 对于`treeNodesChanged`、`treeNodesInserted`和`treeNodesRemoved`，分别返回更改、插入或删除的节点的索引。对于`treeStructureChanged`没有有用的返回。 |
| [获取子节点](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelEvent.html#getChildren--) | 返回与子索引对应的对象。 |
| [获取路径](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelEvent.html#getPath--) | 返回更改、插入或删除节点的父节点的路径。对于`treeStructureChanged`，返回结构发生变化的节点下方的路径。 |
| [获取树路径](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TreeModelEvent.html#getTreePath--) | 返回与`getPath`相同的内容，但作为[`TreePath`](https://docs.oracle.com/javase/8/docs/api/javax/swing/tree/TreePath.html)对象。 |

## 使用树模型监听器的示例

下表列出了使用树展开监听器的示例。

| 示例 | 描述位置 | 备注 |
| --- | --- | --- |
| `DynamicTreeDemo` | 如何使用树 | `DynamicTree` 类实现了树模型监听器，以便检测用户何时编辑了节点的数据。 |
