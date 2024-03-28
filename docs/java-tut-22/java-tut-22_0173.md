# 数据流

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/datastreams.html`](https://docs.oracle.com/javase/tutorial/essential/io/datastreams.html)

数据流支持原始数据类型值（`boolean`、`char`、`byte`、`short`、`int`、`long`、`float`和`double`）以及`String`值的二进制 I/O。所有数据流都实现了[`DataInput`](https://docs.oracle.com/javase/8/docs/api/java/io/DataInput.html)接口或[`DataOutput`](https://docs.oracle.com/javase/8/docs/api/java/io/DataOutput.html)接口。本节重点介绍了这些接口的最常用实现，[`DataInputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/DataInputStream.html)和[`DataOutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/DataOutputStream.html)。

`DataStreams`示例演示了通过写出一组数据记录，然后再次读取它们来演示数据流。每个记录包含与发票上的项目相关的三个值，如下表所示：

| 记录中的顺序 | 数据类型 | 数据描述 | 输出方法 | 输入方法 | 示例值 |
| --- | --- | --- | --- | --- | --- |
| 1 | `double` | 项目价格 | `DataOutputStream.writeDouble` | `DataInputStream.readDouble` | `19.99` |
| 2 | `int` | 单位数量 | `DataOutputStream.writeInt` | `DataInputStream.readInt` | `12` |
| 3 | `String` | 项目描述 | `DataOutputStream.writeUTF` | `DataInputStream.readUTF` | `"Java T-Shirt"` |

让我们来看看`DataStreams`中关键的代码。首先，程序定义了一些包含数据文件名称和将写入其中的数据的常量：

```java
static final String dataFile = "invoicedata";

static final double[] prices = { 19.99, 9.99, 15.99, 3.99, 4.99 };
static final int[] units = { 12, 8, 13, 29, 50 };
static final String[] descs = {
    "Java T-shirt",
    "Java Mug",
    "Duke Juggling Dolls",
    "Java Pin",
    "Java Key Chain"
};

```

然后`DataStreams`打开一个输出流。由于`DataOutputStream`只能作为现有字节流对象的包装器创建，`DataStreams`提供了一个带缓冲的文件输出字节流。

```java
out = new DataOutputStream(new BufferedOutputStream(
              new FileOutputStream(dataFile)));

```

`DataStreams`写出记录并关闭输出流。

```java
for (int i = 0; i < prices.length; i ++) {
    out.writeDouble(prices[i]);
    out.writeInt(units[i]);
    out.writeUTF(descs[i]);
}

```

`writeUTF`方法以修改后的 UTF-8 形式写出`String`值。这是一种只需要一个字节来表示常见西方字符的可变宽度字符编码。

现在`DataStreams`再次读取数据。首先，它必须提供一个输入流和变量来保存输入数据。与`DataOutputStream`一样，`DataInputStream`必须作为字节流的包装器构建。

```java
in = new DataInputStream(new
            BufferedInputStream(new FileInputStream(dataFile)));

double price;
int unit;
String desc;
double total = 0.0;

```

现在`DataStreams`可以读取流中的每个记录，并报告遇到的数据。

```java
try {
    while (true) {
        price = in.readDouble();
        unit = in.readInt();
        desc = in.readUTF();
        System.out.format("You ordered %d" + " units of %s at $%.2f%n",
            unit, desc, price);
        total += unit * price;
    }
} catch (EOFException e) {
}

```

请注意，`DataStreams`通过捕获[`EOFException`](https://docs.oracle.com/javase/8/docs/api/java/io/EOFException.html)来检测文件结束条件，而不是测试无效的返回值。所有`DataInput`方法的实现都使用`EOFException`而不是返回值。

还要注意，`DataStreams` 中的每个专门的 `write` 都与相应的专门的 `read` 完全匹配。程序员需要确保输出类型和输入类型以这种方式匹配：输入流由简单的二进制数据组成，没有任何内容指示个别值的类型，或者它们在流中的位置。

`DataStreams` 使用了一种非常糟糕的编程技术：它使用浮点数来表示货币值。一般来说，浮点数对于精确值是不好的。对于十进制小数来说尤其糟糕，因为常见的值（比如`0.1`）没有二进制表示。

用于货币值的正确类型是[`java.math.BigDecimal`](https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html)。不幸的是，`BigDecimal`是一个对象类型，所以它不能与数据流一起使用。然而，`BigDecimal` *可以* 与对象流一起使用，这将在下一节中介绍。
