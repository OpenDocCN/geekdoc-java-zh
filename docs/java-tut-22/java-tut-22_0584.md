# BCP 47 扩展

> 原文：[`docs.oracle.com/javase/tutorial/i18n/locale/extensions.html`](https://docs.oracle.com/javase/tutorial/i18n/locale/extensions.html)

Java SE 7 版本符合 IETF BCP 47 标准，支持向`Locale`添加扩展。任何单个字符都可以用于表示扩展，但有两个预定义的扩展代码：`'u'`指定*Unicode 语言环境扩展*，`'x'`指定*私有使用扩展*。

Unicode 语言环境扩展由 Unicode[通用语言环境数据存储库（CLDR）](http://cldr.unicode.org/)项目定义。它们用于指定非语言特定的信息，例如日历或货币。私有使用扩展可用于指定任何其他信息，例如平台（例如 Windows、UNIX 或 Linux）或发布信息（例如 6u23 或 JDK 7）。

扩展被指定为键/值对，其中键是单个字符（通常为`'u'`或`'x'`）。格式良好的值具有以下格式：

```java
SUBTAG ('-' SUBTAG)*

```

在这种格式中：

```java
SUBTAG = [0-9a-zA-Z]{2,8}    *For key='u'*
SUBTAG = [0-9a-zA-Z]{1,8}    *For key='x'*

```

请注意，私有使用扩展允许单个字符值。但是，Unicode 语言环境扩展中的值至少需要 2 个字符。

扩展字符串不区分大小写，但`Locale`类将所有键和值映射为小写。

[`getExtensionKeys()`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getExtensionKeys--)方法返回`Locale`的（如果有的话）扩展键集。[`getExtension(key)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getExtension-char-)方法返回指定键的值字符串（如果有）。

## Unicode 语言环境扩展

如前所述，Unicode 语言环境扩展由`'u'`键代码或`UNICODE_LOCALE_EXTENSION`常量指定。值本身也由键/类型对指定。合法值在[键/类型定义](http://www.unicode.org/reports/tr35/#Key_Type_Definitions)表中在[Unicode](http://www.unicode.org)网站上定义。键代码由两个字母字符指定。以下表列出了 Unicode 语言环境扩展键：

| 键代码 | 描述 |
| --- | --- |
| ca | 日历算法 |
| co | 排序类型 |
| ka | 排序参数 |
| cu | 货币类型 |
| nu | 数字类型 |
| va | 通用变体类型 |

* * *

**注意：**

指定 Unicode 语言环境扩展，例如数字格式，并不保证底层平台的语言环境服务会遵守该请求。

* * *

以下表格显示了 Unicode 语言环境扩展的一些键/类型对示例。

| 键/类型对 | 描述 |
| --- | --- |
| ca-buddhist | 泰国佛教日历 |
| co-pinyin | 拉丁文拼音排序 |
| cu-usd | 美元 |
| nu-jpanfin | 日本金融数字 |
| tz-aldav | Europe/Andorra |

以下字符串代表德国语言区域设置，使用 Linux 平台的电话簿样式排序。此示例还包含一个名为`"email"`的属性。

```java
de-DE-u-email-co-phonebk-x-linux

```

可以使用以下`Locale`方法访问有关 Unicode 区域扩展的信息。这些方法使用前面的德语区域示例进行描述。

+   [`getUnicodeLocaleKeys()`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getUnicodeLocaleKeys--)  返回 Unicode 区域键代码或空集（如果区域设置没有）。对德语示例，这将返回一个包含单个字符串`"co"`的集合。

+   [`getUnicodeLocaleType(String)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getUnicodeLocaleType-java.lang.String-)  返回与 Unicode 区域键代码关联的 Unicode 区域类型。对德语示例调用`getUnicodeLocaleType("co")`将返回字符串`"phonebk"`。

+   [`getUnicodeLocaleAttributes()`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getUnicodeLocaleAttributes--)  返回与此区域设置关联的 Unicode 区域设置属性集（如果有的话）。在德语示例中，这将返回一个包含单个字符串`"email"`的集合。

## 私有使用扩展

私有使用扩展，由`'x'`键代码或`PRIVATE_USE_EXTENSION`常量指定，可以是任何值，只要值格式正确即可。

以下是可能的私有使用扩展示例：

```java
x-jdk-1-7
x-linux

```
