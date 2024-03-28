# 监听器 API 表

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/events/api.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/api.html)

在下表中，第一列给出了监听器接口的名称，其中包含指向讨论该监听器的教程页面的链接，或者如果教程未讨论该监听器，则包含指向 API 文档的链接。第二列列出了相应的适配器类（如果有）。（有关使用适配器的讨论，请参见使用适配器和内部类处理事件。）第三列列出了监听器接口包含的方法，并显示传递到方法中的事件对象的类型。通常，监听器、适配器和事件类型具有相同的名称前缀，但并非总是如此。

要查看哪些 Swing 组件可以触发哪些类型的事件，请参见 Swing 组件支持的监听器。

| 监听器接口 | 适配器类 | 监听器方法 |
| --- | --- | --- |
| `动作监听器` | *无* | `actionPerformed(ActionEvent)` |

| [`祖先监听器`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/AncestorListener.html) | *无* | `ancestorAdded(AncestorEvent)` `ancestorMoved(AncestorEvent)`

`ancestorRemoved(AncestorEvent)` |

| `插入符监听器` | *无* | `caretUpdate(CaretEvent)` |
| --- | --- | --- |
| [`单元编辑器监听器`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/CellEditorListener.html) | *无* | `editingStopped(ChangeEvent)` `editingCanceled(ChangeEvent)` |
| `变更监听器` | *无* | `stateChanged(ChangeEvent)` |

| `组件监听器` | `组件适配器` | `componentHidden(ComponentEvent)` `componentMoved(ComponentEvent)`

`componentResized(ComponentEvent)`

`componentShown(ComponentEvent)` |

| `容器监听器` | `容器适配器` | `componentAdded(ContainerEvent)` `componentRemoved(ContainerEvent)` |
| --- | --- | --- |

| `文档监听器` | *无* | `changedUpdate(DocumentEvent)` `insertUpdate(DocumentEvent)`

`removeUpdate(DocumentEvent)` |

| [`异常监听器`](https://docs.oracle.com/javase/8/docs/api/java/beans/ExceptionListener.html) | *无* | `exceptionThrown(Exception)` |
| --- | --- | --- |
| `焦点监听器` | `焦点适配器` | `focusGained(FocusEvent)` `focusLost(FocusEvent)` |
| [`层次边界监听器`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/HierarchyBoundsListener.html) | `层次边界适配器` | `ancestorMoved(HierarchyEvent)` `ancestorResized(HierarchyEvent)` |
| [`层次监听器`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/HierarchyListener.html) | *无* | `hierarchyChanged(HierarchyEvent)` |
| [`超链接监听器`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/HyperlinkListener.html) | *无* | `hyperlinkUpdate(HyperlinkEvent)` |
| [`InputMethodListener`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputMethodListener.html) | *none* | `caretPositionChanged(InputMethodEvent)` `inputMethodTextChanged(InputMethodEvent)` |

| `InternalFrameListener` | `InternalFrameAdapter` | `internalFrameActivated(InternalFrameEvent)` `internalFrameClosed(InternalFrameEvent)`

`internalFrameClosing(InternalFrameEvent)`

`internalFrameDeactivated(InternalFrameEvent)`

`internalFrameDeiconified(InternalFrameEvent)`

`internalFrameIconified(InternalFrameEvent)`

`internalFrameOpened(InternalFrameEvent)` |

| `ItemListener` | *none* | `itemStateChanged(ItemEvent)` |
| --- | --- | --- |

| `KeyListener` | `KeyAdapter` | `keyPressed(KeyEvent)` `keyReleased(KeyEvent)`

`keyTyped(KeyEvent)` |

| `ListDataListener` | *none* | `contentsChanged(ListDataEvent)` `intervalAdded(ListDataEvent)`

`intervalRemoved(ListDataEvent)` |

| `ListSelectionListener` | *none* | `valueChanged(ListSelectionEvent)` |
| --- | --- | --- |

| [`MenuDragMouseListener`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/MenuDragMouseListener.html) | *none* | `menuDragMouseDragged(MenuDragMouseEvent)` `menuDragMouseEntered(MenuDragMouseEvent)`

`menuDragMouseExited(MenuDragMouseEvent)`

`menuDragMouseReleased(MenuDragMouseEvent)` |

| [`MenuKeyListener`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/MenuKeyListener.html) | *none* | `menuKeyPressed(MenuKeyEvent)` `menuKeyReleased(MenuKeyEvent)`

`menuKeyTyped(MenuKeyEvent)` |

| [`MenuListener`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/MenuListener.html) | *none* | `menuCanceled(MenuEvent)` `menuDeselected(MenuEvent)`

`menuSelected(MenuEvent)` |

| `MouseInputListener` (extends `MouseListener` and `MouseMotionListener` | `MouseInputAdapter` `MouseAdapter` | `mouseClicked(MouseEvent)` `mouseEntered(MouseEvent)`

`mouseExited(MouseEvent)`

`mousePressed(MouseEvent)`

`mouseReleased(MouseEvent)`

`mouseDragged(MouseEvent)`

`mouseMoved(MouseEvent)`

`MouseAdapter(MouseEvent)` |

| `MouseListener` | `MouseAdapter`, `MouseInputAdapter` | `mouseClicked(MouseEvent)` `mouseEntered(MouseEvent)`

`mouseExited(MouseEvent)`

`mousePressed(MouseEvent)`

`mouseReleased(MouseEvent)` |

| `MouseMotionListener` | `MouseMotionAdapter`, `MouseInputAdapter` | `mouseDragged(MouseEvent)` `mouseMoved(MouseEvent)` |
| --- | --- | --- |
| `MouseWheelListener` | `MouseAdapter` | `mouseWheelMoved(MouseWheelEvent)` `MouseAdapter<MouseEvent>` |

| [`PopupMenuListener`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/PopupMenuListener.html) | *none* | `popupMenuCanceled(PopupMenuEvent)` `popupMenuWillBecomeInvisible(PopupMenuEvent)`

`popupMenuWillBecomeVisible(PopupMenuEvent)` |

| `属性更改监听器` | *无* | `propertyChange(PropertyChangeEvent)` |
| --- | --- | --- |

| [`表列模型监听器`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/TableColumnModelListener.html) | *无* | `columnAdded(TableColumnModelEvent)` `columnMoved(TableColumnModelEvent)`

`columnRemoved(TableColumnModelEvent)`

`columnMarginChanged(ChangeEvent)`

`columnSelectionChanged(ListSelectionEvent)` |

| `表模型监听器` | *无* | `tableChanged(TableModelEvent)` |
| --- | --- | --- |
| `树展开监听器` | *无* | `treeCollapsed(TreeExpansionEvent)` `treeExpanded(TreeExpansionEvent)` |

| `树模型监听器` | *无* | `treeNodesChanged(TreeModelEvent)` `treeNodesInserted(TreeModelEvent)`

`treeNodesRemoved(TreeModelEvent)`

`treeStructureChanged(TreeModelEvent)` |

| `树选择监听器` | *无* | `valueChanged(TreeSelectionEvent)` |
| --- | --- | --- |
| `树将展开监听器` | *无* | `treeWillCollapse(TreeExpansionEvent)` `treeWillExpand(TreeExpansionEvent)` |
| `可撤销编辑监听器` | *无* | `undoableEditHappened(UndoableEditEvent)` |
| [`可否更改监听器`](https://docs.oracle.com/javase/8/docs/api/java/beans/VetoableChangeListener.html) | *无* | `vetoableChange(PropertyChangeEvent)` |
| `窗口焦点监听器` | `窗口适配器` | `windowGainedFocus(WindowEvent)` `windowLostFocus(WindowEvent)` |

| `窗口监听器` | `窗口适配器` | `windowActivated(WindowEvent)` `windowClosed(WindowEvent)`

`windowClosing(WindowEvent)`

`windowDeactivated(WindowEvent)`

`windowDeiconified(WindowEvent)`

`windowIconified(WindowEvent)`

`windowOpened(WindowEvent)` |

| `窗口状态监听器` | `窗口适配器` | `windowStateChanged(WindowEvent)` |
| --- | --- | --- |
