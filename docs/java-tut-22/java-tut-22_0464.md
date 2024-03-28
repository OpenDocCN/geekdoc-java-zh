# 演示 - DropDemo

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/dropmodedemo.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/dropmodedemo.html)

现在我们将看一个演示，使用自定义传输处理程序为列表组件实现拖放。尽管列表的默认传输处理程序实现了导出，但因为我们正在创建一个自定义传输处理程序来实现导入，所以我们必须重新实现导出。

如您从屏幕截图中看到的那样，`DropDemo`包含一个可编辑的文本区域、一个列表和一个允许您为列表选择拖放模式的组合框。

![DropDemo 屏幕截图](img/33c4d168420b7b01a7bab0f2bf54f140.png)

* * *

**试试这个：**

1.  点击“启动”按钮以使用[Java™ Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html)运行`DropDemo`（[下载 JDK 7 或更高版本](http://www.oracle.com/technetwork/java/javase/downloads/index.html)）。或者，要自行编译和运行示例，请参考示例索引。![启动 DropDemo 示例](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/DropDemoProject/DropDemo.jnlp)

1.  在文本区域中选择一些文本并将其拖放到列表中。所选列表条目将被替换，并且该项目将成为当前选择。这就是`USE_SELECTION`的工作原理，提供了向后兼容性，但否则不建议使用。

1.  将列表拖放模式更改为`ON`并尝试相同操作。再次，所选列表项将被替换，但当前选择不会移动。

1.  将列表拖放模式更改为`INSERT`并重复相同操作。添加的文本将插入到拖放位置。在此模式下，无法修改现有列表项。

1.  将列表拖放模式更改为`ON_OR_INSERT`。根据光标位置，您可以插入新文本或替换现有文本。

* * *

这里是``ListTransferHandler``的实现，用于``DropDemo.java``。

此列表的传输处理程序支持复制和移动，并重新实现了列表默认提供的拖动支持。

```java
public class ListTransferHandler extends TransferHandler {
    private int[] indices = null;
    private int addIndex = -1; //Location where items were added
    private int addCount = 0;  //Number of items added.

    /**
     * We only support importing strings.
     */
    public boolean canImport(TransferHandler.TransferSupport info) {
        // Check for String flavor
        if (!info.isDataFlavorSupported(DataFlavor.stringFlavor)) {
            return false;
        }
        return true;
   }

    /**
     * Bundle up the selected items in a single list for export.
     * Each line is separated by a newline.
     */
    protected Transferable createTransferable(JComponent c) {
        JList list = (JList)c;
        indices = list.getSelectedIndices();
        Object[] values = list.getSelectedValues();

        StringBuffer buff = new StringBuffer();

        for (int i = 0; i < values.length; i++) {
            Object val = values[i];
            buff.append(val == null ? "" : val.toString());
            if (i != values.length - 1) {
                buff.append("\n");
            }
        }

        return new StringSelection(buff.toString());
    }

    /**
     * We support both copy and move actions.
     */
    public int getSourceActions(JComponent c) {
        return TransferHandler.COPY_OR_MOVE;
    }

    /**
     * Perform the actual import.  This demo only supports drag and drop.
     */
    public boolean importData(TransferHandler.TransferSupport info) {
        if (!info.isDrop()) {
            return false;
        }

        JList list = (JList)info.getComponent();
        DefaultListModel listModel = (DefaultListModel)list.getModel();
        JList.DropLocation dl = (JList.DropLocation)info.getDropLocation();
        int index = dl.getIndex();
        boolean insert = dl.isInsert();

        // Get the string that is being dropped.
        Transferable t = info.getTransferable();
        String data;
        try {
            data = (String)t.getTransferData(DataFlavor.stringFlavor);
        } 
        catch (Exception e) { return false; }

        // Wherever there is a newline in the incoming data,
        // break it into a separate item in the list.
        String[] values = data.split("\n");

        addIndex = index;
        addCount = values.length;

        // Perform the actual import.  
        for (int i = 0; i < values.length; i++) {
            if (insert) {
                listModel.add(index++, values[i]);
            } else {
                // If the items go beyond the end of the current
                // list, add them in.
                if (index < listModel.getSize()) {
                    listModel.set(index++, values[i]);
                } else {
                    listModel.add(index++, values[i]);
                }
            }
        }
        return true;
    }

    /**
     * Remove the items moved from the list.
     */
    protected void exportDone(JComponent c, Transferable data, int action) {
        JList source = (JList)c;
        DefaultListModel listModel  = (DefaultListModel)source.getModel();

        if (action == TransferHandler.MOVE) {
            for (int i = indices.length - 1; i >= 0; i--) {
                listModel.remove(indices[i]);
            }
        }

        indices = null;
        addCount = 0;
        addIndex = -1;
    }
}

```

接下来我们将看一下目标如何选择拖放操作。
