# 物理和逻辑字体

> 原文：[`docs.oracle.com/javase/tutorial/2d/text/fonts.html`](https://docs.oracle.com/javase/tutorial/2d/text/fonts.html)

有两种字体：物理字体和逻辑字体。物理字体是实际的字体库，包括 TrueType 或 PostScript Type 1 字体。物理字体可以是 Time、Helvetica、Courier 或任何其他字体，包括国际字体。逻辑字体是以下五个字体系列：Serif、SansSerif、Monospaced、Dialog 和 DialogInput。这些逻辑字体不是实际的字体库。相反，Java 运行时环境通过逻辑字体名称将其映射到物理字体。

本节帮助您确定在应用程序中使用哪种字体。它涵盖以下主题：

+   物理字体

    +   Lucidia 字体

    +   将物理字体与您的应用程序捆绑

+   逻辑字体

+   使用物理和逻辑字体的优缺点

+   字体配置文件

## 物理字体

物理字体是包含字形数据和表的实际字体库，使用 TrueType 或 PostScript Type 1 等字体技术，以将字符序列映射到字形序列。要获取系统中安装的所有可用字体系列的名称，请调用以下内容：

```java
GraphicsEnvironment ge = GraphicsEnvironment.getLocalGraphicsEnvironment();
String []fontFamilies = ge.getAvailableFontFamilyNames();

```

FontSelector 示例程序（在 `FontSelector.java` 中可用）演示了如何定位和选择这些字体。

* * *

**注意：** 应用程序不应假设任何特定的物理字体存在。然而，逻辑字体是一个安全的选择，因为它们始终存在。有关更多信息，请参阅逻辑字体。

* * *

<applet code="FontSelector" archive="examples/lib/FontSelectorApplet.jar" alt="FontSelector applet" width="500" height="300"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果您看不到 applet 运行，请至少安装[Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 版本。

* * *

### Lucidia 字体

Oracle 的 JRE 包含这一系列物理字体，也被许可用于其他 Java 平台的实现。这些字体是物理字体，但不依赖于主机操作系统。

使用这些字体的应用程序可以在这些字体可用的任何地方实现相同的外观。此外，这些字体涵盖了大量的语言（特别是欧洲和中东地区），因此您可以为支持的语言创建完全多语言的应用程序。然而，这些字体可能不在所有 JRE 中可用。此外，它们目前不涵盖完整的 Unicode 字符集；特别是，不支持中文、日文和韩文。

### 将物理字体与您的应用程序捆绑

有时，应用程序不能依赖于系统上安装的字体，通常是因为该字体是一种不可用的自定义字体。在这种情况下，必须将字体文件与应用程序捆绑在一起。

使用以下方法之一从现有物理字体创建 `Font` 对象：

```java
Font java.awt.Font.createFont(int fontFormat, InputStream in);
Font java.awt.Font.createFont(int fontFormat, File fontFile);

```

要从 TrueType 字体创建 `Font` 对象，形式参数 `fontFormat` 必须是常量 `Font.TRUETYPE_FONT`。以下示例从 TrueType 字体文件 `A.ttf` 创建 `Font` 对象：

```java
Font font = Font.createFont(Font.TRUETYPE_FONT, new File("A.ttf"));

```

直接从文件访问字体更简单、更方便。但是，如果您的代码无法访问文件系统资源，或者字体与应用程序或小程序的其余部分一起打包在 Java 存档（JAR）文件中，则可能需要一个 `InputStream` 对象。

`createFont` 方法创建一个具有点大小为 1 和样式 `PLAIN` 的新 `Font` 对象。然后可以使用 `Font.deriveFont` 方法将此基础字体用于派生具有不同大小、样式、变换和字体特性的新 `Font` 对象。例如：

```java
try {
     //Returned font is of pt size 1
     Font font = Font.createFont(Font.TRUETYPE_FONT, new File("A.ttf"));

     //Derive and return a 12 pt version:
     //Need to use float otherwise
     //it would be interpreted as style

     return font.deriveFont(12f);

} catch (IOException|FontFormatException e) {
     // Handle exception
}

```

使用 `deriveFont` 方法很重要，因为应用程序创建的字体不属于底层字体系统所知的字体集。由于 `deriveFont` 方法是从最初创建的字体工作的，因此它没有这种限制。

解决此问题的方法是将创建的字体注册到图形环境中。例如：

```java
try {
     GraphicsEnvironment ge = 
         GraphicsEnvironment.getLocalGraphicsEnvironment();
     ge.registerFont(Font.createFont(Font.TRUETYPE_FONT, new File("A.ttf"));
} catch (IOException|FontFormatException e) {
     //Handle exception
}

```

在将字体注册到图形环境后，该字体可以在调用 `getAvailableFontFamilyNames()` 时使用，并且可以在字体构造函数中使用。

## 逻辑字体

Java SE 定义了以下五个逻辑字体系列：

+   `Dialog`

+   `DialogInput`

+   `Monospaced`

+   `Serif`

+   `SansSerif`

这些字体在任何 Java 平台上都可用，并且可以被视为某些具有其名称所暗示属性的基础字体的别名。Serif 字体类似于 Times New Roman，通常用于印刷。Sans Serif 字体更适合屏幕使用。

这些字体可以根据用户的语言环境进行定制。此外，这些字体支持最广泛的代码点（Unicode 字符）范围。

除了字体系列，字体还具有其他属性，其中最重要的是 *样式* 和 *大小*。样式有 **Bold** 和 *Italic*。

Java 2D API 使用的默认字体是 12 磅的 Dialog。这种字体是在普通 72-120 DPI 显示设备上阅读文本的典型字号。应用程序可以通过指定以下内容直接创建此字体的实例：

```java
Font font = new Font("Dialog", Font.PLAIN, 12);

```

## 使用物理和逻辑字体的优缺点

物理字体使应用程序能够充分利用所有可用字体，实现不同的文本外观和最大的语言覆盖范围。然而，创建使用物理字体的应用程序要困难得多。

将物理字体与您的应用程序捆绑在一起，可以使您创建的应用程序在任何地方看起来都一样，并且可以完全控制您想要支持的应用程序。然而，捆绑的字体可能会很大，特别是如果您希望您的应用程序支持中文、日文和韩文。此外，您可能需要解决许可问题。

逻辑字体名称保证在任何地方都能正常工作，并且它们至少能够在主机操作系统本地化的语言中进行文本呈现（通常支持更广泛的语言范围）。然而，用于呈现文本的物理字体在不同的实现、主机操作系统和区域设置之间会有所不同，因此应用程序无法在任何地方实现相同的外观。此外，映射机制有时会限制可以呈现的字符范围。这在 JRE 版本 5.0 之前曾经是一个大问题：例如，日文文本只能在日本本地化的主机操作系统上呈现，而在其他本地化系统上即使安装了日文字体也无法呈现。对于使用 2D 字体呈现的应用程序，在 JRE 版本 5.0 及更高版本中，这个问题要少得多，因为映射机制现在通常会识别并使用所有支持的书写系统的字体（如果已安装）。

## 字体配置文件

Java SE 运行时环境使用字体配置文件将逻辑字体名称映射到物理字体。根据主机操作系统版本的不同映射，有几个文件支持不同的映射。这些文件位于 JRE 安装的 `lib` 目录中。您可以编辑或创建自己的字体配置文件，以调整映射到您特定系统设置的映射。有关更多信息，请参阅[字体配置文件](https://docs.oracle.com/javase/8/docs/technotes/guides/intl/fontconfig.html)。
