# 将所有内容整合在一起 - 拖放和剪切复制粘贴

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/together.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/together.html)

我们已经展示了如何实现拖放支持以及如何实现剪切、复制、粘贴支持。如何在一个组件中同时实现两者？

你需要在`TransferHandler`的`importData`方法中实现这两种功能，就像这样：

```java
if (transferSupport.isDrop()) {
    // put data in transferSupport.getDropLocation()
} else {
    // determine where you want the paste to go (ex: after current selection)
    // put data there
}

```

在非文本组件中的 CCP 页面上讨论的`ListCutPaste`示例支持拖放和剪切复制粘贴。这是它的`importData`方法（用粗体标记了`if`-`else`的逻辑）：

```java
    public boolean importData(TransferHandler.TransferSupport info) {
        String data = null;

        //If we cannot handle the import, bail now.
        if (!canImport(info)) {
            return false;
        }

        JList list = (JList)info.getComponent();
        DefaultListModel model = (DefaultListModel)list.getModel();
        //Fetch the data -- bail if this fails
        try {
            data = (String)info.getTransferable().getTransferData(DataFlavor.stringFlavor);
        } catch (UnsupportedFlavorException ufe) {
            System.out.println("importData: unsupported data flavor");
            return false;
        } catch (IOException ioe) {
            System.out.println("importData: I/O exception");
            return false;
        }

        if (info.isDrop()) { //This is a drop
            JList.DropLocation dl = (JList.DropLocation)info.getDropLocation();
            int index = dl.getIndex();
            if (dl.isInsert()) {
                model.add(index, data);
                return true;
            } else {
                model.set(index, data);
                return true;
            }
        } else { //This is a paste
            int index = list.getSelectedIndex();
            // if there is a valid selection,
            // insert data after the selection
            if (index >= 0) {
                model.add(list.getSelectedIndex()+1, data);
            // else append to the end of the list
            } else {
                model.addElement(data);
            }
            return true;
        }
    }

```

这是唯一需要安装`if`-`else`逻辑来区分拖放和剪切复制粘贴的地方。
