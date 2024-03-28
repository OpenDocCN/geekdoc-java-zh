# 使用文本属性来设置文本样式

> 原文：[`docs.oracle.com/javase/tutorial/2d/text/textattributes.html`](https://docs.oracle.com/javase/tutorial/2d/text/textattributes.html)

应用程序通常需要能够应用以下文本属性：

+   *下划线* – 在文本下方绘制的线

+   *删除线* – 通过文本绘制的水平线

+   *上标*或*下标* – 文本或字母略高于一行或相应地低于一行

+   *字间距* – 调整字符之间的空间

这些以及其他文本属性可以通过使用 Java 2D `TextAttribute` 类来应用。

要应用这些文本属性，请将它们添加到 `Font` 对象中。例如：

```java
Map<TextAttribute, Object> map =
    new Hashtable<TextAttribute, Object>();
map.put(TextAttribute.KERNING,
    TextAttribute.KERNING_ON);
font = font.deriveFont(map);
graphics.setFont(font);

```

下面的代码示例显示了按以下顺序应用文本属性：

1.  示例字符串（未应用文本属性）

1.  字间距

1.  字间距和下划线

1.  字间距、下划线和删除线

1.  字间距、下划线、删除线和颜色

<applet code="AttributedText" archive="examples/lib/AttributedTextApplet.jar" alt="AttributedText applet" width="300" height="250"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果您看不到 applet 运行，您需要安装至少 [Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 版本。

* * *

此 applet 的完整代码在 `AttributedText.java` 中。
