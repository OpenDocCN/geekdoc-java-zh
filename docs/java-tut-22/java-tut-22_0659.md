# 读取/加载图像

> 原文：[`docs.oracle.com/javase/tutorial/2d/images/loadimage.html`](https://docs.oracle.com/javase/tutorial/2d/images/loadimage.html)

当你想到数字图像时，你可能会想到采样图像格式，比如数字摄影中使用的 JPEG 图像格式，或者网页常用的 GIF 图像。所有可以使用这些图像的程序都必须首先将它们从外部格式转换为内部格式。

Java 2D 支持使用其 Image I/O API 将这些外部图像格式加载到其 `BufferedImage` 格式中，该 API 位于 `javax.imageio` 包中。Image I/O 内置支持 GIF、PNG、JPEG、BMP 和 WBMP。Image I/O 也是可扩展的，因此开发人员或管理员可以为其他格式“插入”支持。例如，TIFF 和 JPEG 2000 的插件是单独可用的。

要从特定文件加载图像，请使用以下代码，这段代码来自 `LoadImageApp.java`：

```java
BufferedImage img = null;
try {
    img = ImageIO.read(new File("strawberry.jpg"));
} catch (IOException e) {
}

```

Image I/O 将文件的内容识别为 JPEG 格式图像，并将其解码为可以被 Java 2D 直接使用的 `BufferedImage`。

`LoadImageApp.java` 展示了如何显示这个图像。

如果代码在小程序中运行，那么从小程序代码库获取图像就像轻而易举一样。以下摘录来自 `LoadImageApplet.java`：

```java
try {
    URL url = new URL(getCodeBase(), "examples/strawberry.jpg");
    img = ImageIO.read(url);
} catch (IOException e) {
}

```

这个示例中使用的 `getCodeBase` 方法返回部署在 Web 服务器上时包含这个小程序的目录的 URL。如果小程序是本地部署的，`getCodeBase` 返回 null，小程序将无法运行。

以下示例展示了如何使用 `getCodeBase` 方法加载 `strawberry.jpg` 文件。

<applet code="LoadImageApplet" archive="examples/lib/LoadImageApplet.jar" width="200" height="200" alt="LoadImageApplet example"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果你看不到小程序运行，你需要至少安装 [Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 版本。

* * *

`LoadImageApplet.java` 包含了这个示例的完整代码，这个小程序需要 `strawberry.jpg` 图像文件。

除了从文件或 URLS 读取外，Image I/O 还可以从其他来源读取，比如 `InputStream`。`ImageIO.read()` 对于大多数应用程序来说是最直接的便利 API，但 `javax.imageio.ImageIO` 类提供了更多静态方法，用于更高级的 Image I/O API 的用法。这个类上的方法集仅代表了用于发现关于图像信息和控制图像解码（读取）过程的丰富 API 集合的一部分。

我们将在 写入/保存图像 部分中进一步探讨 Image I/O 的其他功能。
