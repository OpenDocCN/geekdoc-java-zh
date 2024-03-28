# 示例用法

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/usage.html`](https://docs.oracle.com/javase/tutorial/i18n/text/usage.html)

本页面包含一些代码片段，展示了几种常见场景。 

## 从代码点创建 `String`

```java
String newString(int codePoint) {
    return new String(Character.toChars(codePoint));
}

```

## 从代码点创建 `String` - 为 BMP 字符进行优化

`Character.toChars` 方法创建一个临时数组，仅使用一次然后丢弃。如果这对性能产生负面影响，你可以使用以下针对 BMP 字符（由单个 `char` 值表示的字符）进行优化的方法。在这种方法中，`toChars` 仅用于补充字符。

```java
String newString(int codePoint) {
    if (Character.charCount(codePoint) == 1) {
        return String.valueOf(codePoint);
    } else {
        return new String(Character.toChars(codePoint));
    }
}

```

## 批量创建 `String` 对象

要创建大量字符串，前面代码片段的批量版本重用了 `toChars` 方法使用的数组。该方法为每个代码点创建一个单独的 `String` 实例，并针对 BMP 字符进行了优化。

```java
String[] newStrings(int[] codePoints) {
    String[] result = new String[codePoints.length];
    char[] codeUnits = new char[2];
    for (int i = 0; i < codePoints.length; i++) {
        int count = Character.toChars(codePoints[i], codeUnits, 0);
        result[i] = new String(codeUnits, 0, count);
    }
    return result;
}

```

## 生成消息

格式化 API 支持补充字符。以下示例是生成消息的简单方法。

```java
// recommended
System.out.printf("Character %c is invalid.%n", codePoint);

```

以下方法简单且避免了连接，这使得文本更难本地化，因为并非所有语言都按照英语的顺序将数字值插入字符串中。

```java
// not recommended
System.out.println("Character " + String.valueOf(char) + " is invalid.");

```
