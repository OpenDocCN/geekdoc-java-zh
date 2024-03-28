# 文本组件 API

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/components/textapi.html`](https://docs.oracle.com/javase/tutorial/uiswing/components/textapi.html)

本节列出了文本组件共享的常用 API 部分。这些 API 大部分由[`JTextComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html)类定义。文本组件特性讨论了如何使用部分 API。

JComponent 类描述了文本组件从`JComponent`继承的 API。有关特定文本组件相关的 API 信息，请参阅该组件的操作页面：文本字段、密码字段、格式化文本字段、文本区域或编辑器窗格和文本窗格。

有关文本 API 的完整详细信息，请参阅[`JTextComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html)的 API 文档以及[text package](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/package-summary.html)中各种类和接口的文档。

本节列出的 API 包括以下类别：

+   设置属性

+   选择操作

+   在模型和视图之间转换位置

+   文本编辑命令

+   表示文档的类和接口

+   处理文档

+   操作插入符和选择高亮器

+   读取和写入文本

**设置属性**

*这些方法在`JTextComponent`类中定义。*

| 方法 | 描述 |
| --- | --- |
| [void setEditable(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setEditable-boolean-) [boolean isEditable()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#isEditable--) | 设置或指示用户是否可以编辑文本组件中的文本。 |
| [void setDragEnabled(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setDragEnabled-boolean-) [boolean getDragEnabled()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getDragEnabled--) | 设置或获取`dragEnabled`属性，必须为 true 才能在此组件上启用拖放处理。默认值为 false。有关更多详细信息，请参阅拖放和数据传输。 |
| [void setDisabledTextColor(Color)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setDisabledTextColor-java.awt.Color-) [Color getDisabledTextColor()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getDisabledTextColor--) | 设置或获取文本组件被禁用时用于显示文本的颜色。 |
| [void setMargin(Insets)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setMargin-java.awt.Insets-) [Insets getMargin()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getMargin--) | 设置或获取文本与文本组件边框之间的边距。 |

操作选择

*这些方法在`JTextComponent`类中定义。*

| 方法 | 描述 |
| --- | --- |
| [String getSelectedText()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getSelectedText--) | 获取当前选定的文本。 |
| [void selectAll()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#selectAll--) [void select(int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#select-int-int-) | 选择所有文本或选择起始和结束范围内的文本。 |

| [void setSelectionStart(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setSelectionStart-int-) [void setSelectionEnd(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setSelectionEnd-int-)

[int getSelectionStart()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getSelectionStart--)

[int getSelectionEnd()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getSelectionEnd--) | 通过索引设置或获取当前选择的范围。 |

| [void setSelectedTextColor(Color)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setSelectedTextColor-java.awt.Color-) [Color getSelectedTextColor()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getSelectedTextColor--) | 设置或获取选定文本的颜色。 |
| --- | --- |
| [void setSelectionColor(Color)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setSelectionColor-java.awt.Color-) [Color getSelectionColor()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getSelectionColor--) | 设置或获取选定文本的背景颜色。 |

在模型和视图之间转换位置

*这些方法在`JTextComponent`类中定义。*

| 方法 | 描述 |
| --- | --- |
| [int viewToModel(Point)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#viewToModel-java.awt.Point-) | 将视图坐标系中的指定点转换为文本中的位置。 |
| [Rectangle modelToView(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#modelToView-int-) | 将文本中指定位置转换为视图坐标系中的矩形。 |

文本编辑命令

| 类或方法 | 描述 |
| --- | --- |

| [void cut()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#cut--) [void copy()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#copy--)

[void paste()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#paste--)

[void replaceSelection(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#replaceSelection-java.lang.String-)

*(在`JTextComponent`中)* | 使用系统剪贴板剪切、复制和粘贴文本，或分别用参数指定的字符串替换所选文本。 |

| [EditorKit](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/EditorKit.html) | 提供文本组件的视图工厂、文档、插入符号和操作，以及读取和写入特定格式的文档。 |
| --- | --- |
| [DefaultEditorKit](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.html) | `EditorKit`的具体子类，提供基本的文本编辑功能。 |
| [StyledEditorKit](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/StyledEditorKit.html) | `Default EditorKit`的子类，为带样式文本提供额外的编辑功能。 |
| [字符串 *xxxx*操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.html#field_summary) *(在`DefaultEditorKit`中)* | 默认编辑器工具支持的所有操作的名称。请参见将文本操作与菜单和按钮关联。 |

| [哔哔操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.BeepAction.html) [复制操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.CopyAction.html)

[剪切操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.CutAction.html)

[默认键入操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.DefaultKeyTypedAction.html)

[插入换行操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.InsertBreakAction.html)

[插入内容操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.InsertContentAction.html)

[插入制表符操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.InsertTabAction.html)

[粘贴操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.PasteAction.html)

*(在`DefaultEditorKit`中)* | 实现各种文本编辑命令的内部类。 |

| [对齐操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/StyledEditorKit.AlignmentAction.html) [加粗操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/StyledEditorKit.BoldAction.html)

[字体族操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/StyledEditorKit.FontFamilyAction.html)

[字体大小操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/StyledEditorKit.FontSizeAction.html)

[前景色操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/StyledEditorKit.ForegroundAction.html)

[斜体操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/StyledEditorKit.ItalicAction.html)

[样式文本操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/StyledEditorKit.StyledTextAction.html)

[下划线操作](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/StyledEditorKit.UnderlineAction.html)

*(在`StyledEditorKit`中)* | 实现各种编辑命令以处理带样式文本的内部类。 |

| [Action[] getActions()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getActions--) *(在`JTextComponent`中)* | 获取此组件支持的操作。如果组件使用编辑器工具包，则此方法从编辑器工具包获取操作数组。 |
| --- | --- |
| [InputMap getInputMap()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getInputMap--) *(在`JComponent`中)* | 获取将键绑定到操作的输入映射。参见将文本操作与键绑定。 |
| [void put(KeyStroke, Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/InputMap.html#put-javax.swing.KeyStroke-java.lang.Object-) *(在`InputMap`中)* | 将指定的键绑定到指定的操作。通常通过名称指定操作，对于标准编辑操作，使用字符串常量表示，如`DefaultEditorKit.backwardAction`。 |

代表文档的类和接口

| 接口或类 | 描述 |
| --- | --- |
| [文档](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html) | 定义所有文档必须实现的 API 的接口。 |
| [抽象文档](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/AbstractDocument.html) | `Document`接口的抽象超类实现。这是 Swing 文本包提供的所有文档的超类。 |
| [纯文本文档](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/PlainDocument.html) | 实现`Document`接口的类。这是纯文本组件（文本字段、密码字段和文本区域）的默认文档。此外，在加载纯文本或未知格式的文本时，编辑器窗格和文本窗格使用此类。 |
| [样式文档](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/StyledDocument.html) | `Document`子接口。定义支持带样式文本的文档必须实现的 API。`JTextPane`要求其文档必须是这种类型。 |
| [默认样式文档](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultStyledDocument.html) | 实现`StyledDocument`接口的类。`JTextPane`的默认文档。 |

处理文档

| 类或方法 | 描述 |
| --- | --- |
| [DocumentFilter](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DocumentFilter.html) | 所有文档过滤器的超类。您可以使用文档过滤器来更改从文档中插入或移除的内容，而无需自己实现文档。参见实现文档过滤器。 |
| [void setDocumentFilter(DocumentFilter)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/AbstractDocument.html#setDocumentFilter-javax.swing.text.DocumentFilter-) *(在`AbstractDocument`中)* | 设置文档过滤器。 |

| [void setDocument(Document)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setDocument-javax.swing.text.Document-) [Document getDocument()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getDocument--) |

*(在`JTextComponent`中)* | 设置或获取文本组件的文档。|

| [Document createDefaultModel()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#createDefaultModel--) *(在`JTextField`中)* | 创建一个默认的 PlainDocument 模型。覆盖此方法以创建自定义文档，而不是默认的`PlainDocument`。 |
| --- | --- |

| [void addDocumentListener(DocumentListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html#addDocumentListener-javax.swing.event.DocumentListener-) [void removeDocumentListener(DocumentListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html#removeDocumentListener-javax.swing.event.DocumentListener-) |

*(在`Document`中)* | 添加或移除文档监听器。参见监听文档变化。|

| [void addUndoableEditListener(UndoableEditListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html#addUndoableEditListener-javax.swing.event.UndoableEditListener-) [void removeUndoableEditListener(UndoableEditlistener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html#removeUndoableEditListener-javax.swing.event.UndoableEditListener-) |

*(在`Document`中)* | 添加或移除可撤销编辑监听器。可撤销编辑监听器用于实现撤销和重做。|

| [int getLength()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html#getLength--) [Position getStartPosition()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html#getStartPosition--) |

[Position getEndPosition()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html#getEndPosition--)

[String getText(int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html#getText-int-int-) |

*(在`Document`中)* | 返回有关文档的各种描述信息的`Document`方法。|

| [Object getProperty(Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html#getProperty-java.lang.Object-) [void putProperty(Object, Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html#putProperty-java.lang.Object-java.lang.Object-)

*(在`Document`中)*

[void setDocumentProperties(Dictionary)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/AbstractDocument.html#setDocumentProperties-java.util.Dictionary-)

[Dictionary getDocumentProperties()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/AbstractDocument.html#getDocumentProperties--)

*(在`AbstractDocument`中)* | 一个`Document`维护一组属性，您可以使用这些方法来操作。 |

操纵插入符和选择高亮器

*这些方法在`JTextComponent`类中定义。*

| 接口、类或方法 | 描述 |
| --- | --- |
| [插入符](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Caret.html) | 定义表示文档内插入点的对象的 API 的接口。 |
| [DefaultCaret](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultCaret.html) | 所有文本组件使用的默认插入符。 |
| [void setCaret(Caret)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setCaret-javax.swing.text.Caret-) [Caret getCaret()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getCaret--) | 设置或获取文本组件使用的插入符对象。 |
| [void setCaretColor(Color)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setCaretColor-java.awt.Color-) [Color getCaretColor()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getCaretColor--) | 设置或获取插入符的颜色。 |

| [void setCaretPosition(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setCaretPosition-int-) [void moveCaretPosition(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#moveCaretPosition-int-)

[int getCaretPosition()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getCaretPosition--) | 设置或获取文档内插入符的当前位置。 |

| [void addCaretListener(CaretListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#addCaretListener-javax.swing.event.CaretListener-) [void removeCaretListener(CaretListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#removeCaretListener-javax.swing.event.CaretListener-) | 向文本组件添加或移除插入符监听器。 |
| --- | --- |
| [NavigationFilter](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/NavigationFilter.html) | 所有导航过滤器的超类。导航过滤器允许您修改即将发生在文本组件中的插入符更改。 |
| [void setNavigationFilter(NavigationFilter)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setNavigationFilter-javax.swing.text.NavigationFilter-) | 将导航过滤器附加到文本组件。 |
| [Highlighter](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Highlighter.html) | 定义用于突出显示当前选择的对象的 API 的接口。 |
| [DefaultHighlighter](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultHighlighter.html) | 所有文本组件使用的默认高亮器。 |
| [void setHighlighter(Highlighter)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setHighlighter-javax.swing.text.Highlighter-) [Highlighter getHighlighter()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getHighlighter--) | 设置或获取文本组件使用的高亮器。 |

读取和写入文本

| 方法 | 描述 |
| --- | --- |

| [void read(Reader, Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#read-java.io.Reader-java.lang.Object-) [void write(Writer)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#write-java.io.Writer-)

*(在`JTextComponent`中)* | 读取或写入文本。 |

| [void read(Reader, Document, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/EditorKit.html#read-java.io.Reader-javax.swing.text.Document-int-) [void read(InputStream, Document, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/EditorKit.html#read-java.io.InputStream-javax.swing.text.Document-int-)

*(在`EditorKit`中)* | 从流中读取文本到文档中。 |

| [void write(Writer, Document, int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/EditorKit.html#write-java.io.Writer-javax.swing.text.Document-int-int-) [void write(OutputStream, Document, int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/EditorKit.html#write-java.io.OutputStream-javax.swing.text.Document-int-int-)

*(在`EditorKit`中)* | 将文本从文档写入流中。 |
