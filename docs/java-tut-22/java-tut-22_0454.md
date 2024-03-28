# 更改颜色主题

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/lookandfeel/color.html`](https://docs.oracle.com/javase/tutorial/uiswing/lookandfeel/color.html)

Nimbus 外观具有一组默认颜色，但您不必使用它们。您可以更改颜色以匹配公司品牌或其他颜色方案。

所有 Nimbus 使用的颜色都存储为一组`UIManager`属性。在设置外观之前，您可以更改任何或所有这些属性。例如：

```java
UIManager.put("nimbusBase", new Color(...));
UIManager.put("nimbusBlueGrey", new Color(...));
UIManager.put("control", new Color(...));

for (LookAndFeelInfo info : UIManager.getInstalledLookAndFeels()) {
    if ("Nimbus".equals(info.getName())) {
        UIManager.setLookAndFeel(info.getClassName());
        break;
    }
}

```

这三种基本颜色，`nimbusBase`，`nimbusBlueGrey`和`control`，将满足您大部分需求。在 Nimbus Defaults 页面上查看颜色键及其默认值的完整列表。
