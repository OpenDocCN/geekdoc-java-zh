# 关于 ResourceBundle 类

> 原文：[`docs.oracle.com/javase/tutorial/i18n/resbundle/concept.html`](https://docs.oracle.com/javase/tutorial/i18n/resbundle/concept.html)

## 如何 ResourceBundle 与 Locale 相关联

从概念上讲，每个 `ResourceBundle` 都是一组相关的子类，共享相同的基本名称。以下列表显示了一组相关的子类。`ButtonLabel` 是基本名称。基本名称后面的字符表示 `Locale` 的语言代码、国家代码和变体。例如，`ButtonLabel_en_GB` 匹配由英语语言代码 (`en`) 和英国国家代码 (`GB`) 指定的 `Locale`。

```java
ButtonLabel
ButtonLabel_de
ButtonLabel_en_GB
ButtonLabel_fr_CA_UNIX

```

要选择适当的 `ResourceBundle`，请调用 `ResourceBundle.getBundle` 方法。以下示例选择与法语语言、加拿大国家和 UNIX 平台匹配的 `ButtonLabel` `ResourceBundle`。

```java
Locale currentLocale = new Locale("fr", "CA", "UNIX");
ResourceBundle introLabels = ResourceBundle.getBundle(
                                 "ButtonLabel", currentLocale);

```

如果指定 `Locale` 的 `ResourceBundle` 类不存在，`getBundle` 将尝试找到最接近的匹配项。例如，如果期望的类是 `ButtonLabel_fr_CA_UNIX`，默认 `Locale` 是 `en_US`，`getBundle` 将按以下顺序查找类：

```java
ButtonLabel_fr_CA_UNIX
ButtonLabel_fr_CA
ButtonLabel_fr
ButtonLabel_en_US
ButtonLabel_en
ButtonLabel

```

请注意，在选择基类（`ButtonLabel`）之前，`getBundle` 会根据默认 `Locale` 查找类。如果 `getBundle` 在前述类列表中找不到匹配项，则会抛出 `MissingResourceException`。为避免抛出此异常，您应始终提供没有后缀的基类。

## ListResourceBundle 和 PropertyResourceBundle 子类

抽象类 `ResourceBundle` 有两个子类：`PropertyResourceBundle` 和 `ListResourceBundle`。

`PropertyResourceBundle` 由属性文件支持。属性文件是包含可翻译文本的纯文本文件。属性文件不是 Java 源代码的一部分，它们只能包含 `String` 对象的值。如果需要存储其他类型的对象，请改用 `ListResourceBundle`。章节 使用属性文件支持 ResourceBundle 展示了如何使用 `PropertyResourceBundle`。

`ListResourceBundle` 类使用方便的列表管理资源。每个 `ListResourceBundle` 都由一个类文件支持。您可以在 `ListResourceBundle` 中存储任何特定于区域设置的对象。要为其他 `Locale` 添加支持，您需要创建另一个源文件并将其编译为类文件。章节 使用 ListResourceBundle 中有一个您可能会发现有用的编码示例。

`ResourceBundle` 类是灵活的。如果您首先将特定于区域设置的 `String` 对象放入 `PropertyResourceBundle` 中，然后稍后决定改用 `ListResourceBundle`，则对您的代码没有影响。例如，对 `getBundle` 的以下调用将检索适当 `Locale` 的 `ResourceBundle`，无论 `ButtonLabel` 是由类支持还是由属性文件支持：

```java
ResourceBundle introLabels = ResourceBundle.getBundle(
                                 "ButtonLabel", currentLocale);

```

## 键-值对

`ResourceBundle`对象包含一组键值对。当您想要从`ResourceBundle`中检索值时，您需要指定键，该键必须是一个`String`。该值是特定于区域设置的对象。以下示例中的键是`OkKey`和`CancelKey`字符串：

```java
class ButtonLabel_en extends ListResourceBundle {
    // English version
    public Object[][] getContents() {
        return contents;
    }
    static final Object[][] contents = {
        {"OkKey", "OK"},
        {"CancelKey", "Cancel"},
    };
}

```

要从`ResourceBundle`中检索`OK` `String`，您需要在调用`getString`时指定适当的键：

```java
String okLabel = ButtonLabel.getString("OkKey");

```

属性文件包含键值对。键位于等号的左侧，值位于右侧。每对位于单独的一行。值只能表示`String`对象。以下示例显示了名为`ButtonLabel.properties`的属性文件的内容：

```java
OkKey = OK
CancelKey = Cancel

```
