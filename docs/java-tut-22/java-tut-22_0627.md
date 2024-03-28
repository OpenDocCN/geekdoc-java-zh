# 字节编码和字符串

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/string.html`](https://docs.oracle.com/javase/tutorial/i18n/text/string.html)

如果字节数组包含非 Unicode 文本，您可以使用 `String` 构造方法之一将文本转换为 Unicode。反之，您可以使用 `String.getBytes` 方法将 `String` 对象转换为非 Unicode 字符的字节数组。在调用这些方法时，您需要将编码标识符作为参数之一指定。

接下来的示例将字符在 UTF-8 和 Unicode 之间进行转换。UTF-8 是一种对 UNIX 文件系统安全的 Unicode 传输格式。示例的完整源代码在文件 `StringConverter.java` 中。

`StringConverter` 程序首先创建一个包含 Unicode 字符的 `String`：

```java
String original = new String("A" + "\u00ea" + "\u00f1" + "\u00fc" + "C");

```

当打印时，名为 `original` 的 `String` 如下所示：

```java
AêñüC

```

要将 `String` 对象转换为 UTF-8，调用 `getBytes` 方法并指定适当的编码标识符作为参数。`getBytes` 方法以 UTF-8 格式返回一个字节数组。要从非 Unicode 字节的数组创建 `String` 对象，请使用带有编码参数的 `String` 构造方法。执行这些调用的代码被封装在一个 `try` 块中，以防指定的编码不受支持：

```java
try {
    byte[] utf8Bytes = original.getBytes("UTF8");
    byte[] defaultBytes = original.getBytes();

    String roundTrip = new String(utf8Bytes, "UTF8");
    System.out.println("roundTrip = " + roundTrip);
    System.out.println();
    printBytes(utf8Bytes, "utf8Bytes");
    System.out.println();
    printBytes(defaultBytes, "defaultBytes");
} 
catch (UnsupportedEncodingException e) {
    e.printStackTrace();
}

```

`StringConverter` 程序打印出 `utf8Bytes` 和 `defaultBytes` 数组中的值，以演示一个重要的观点：转换后的文本长度可能与源文本长度不同。一些 Unicode 字符转换为单个字节，而另一些转换为一对或三元组字节。

`printBytes` 方法通过调用在源文件 `UnicodeFormatter.java` 中定义的 `byteToHex` 方法来显示字节数组。以下是 `printBytes` 方法：

```java
public static void printBytes(byte[] array, String name) {
    for (int k = 0; k < array.length; k++) {
        System.out.println(name + "[" + k + "] = " + "0x" +
            UnicodeFormatter.byteToHex(array[k]));
    }
}

```

`printBytes` 方法的输出如下。请注意，只有第一个和最后一个字节，即 A 和 C 字符，在两个数组中是相同的：

```java
utf8Bytes[0] = 0x41
utf8Bytes[1] = 0xc3
utf8Bytes[2] = 0xaa
utf8Bytes[3] = 0xc3
utf8Bytes[4] = 0xb1
utf8Bytes[5] = 0xc3
utf8Bytes[6] = 0xbc
utf8Bytes[7] = 0x43
defaultBytes[0] = 0x41
defaultBytes[1] = 0xea
defaultBytes[2] = 0xf1
defaultBytes[3] = 0xfc
defaultBytes[4] = 0x43

```
