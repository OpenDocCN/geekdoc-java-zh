# 如何在 Java 中获取文件路径分隔符

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-get-file-path-separator-in-java/>

对于[文件路径或目录分隔符](http://web.archive.org/web/20220629160043/https://en.wikipedia.org/wiki/Path_(computing))，Unix 系统引入了斜杠字符`/`作为目录分隔符，微软 Windows 引入了反斜杠字符`\`作为目录分隔符。简单来说，这就是 UNIX 上的`/`和 Windows 上的`\`。

在 Java 中，我们可以使用以下三种方法来获得与平台无关的文件路径分隔符。

1.  `System.getProperty("file.separator")`
2.  `FileSystems.getDefault().getSeparator()`
3.  `File.separator` Java IO

## 1.系统属性

通过[系统属性](http://web.archive.org/web/20220629160043/https://docs.oracle.com/javase/tutorial/essential/environment/sysprop.html)T0 获取文件路径分隔符。

GetFileSeparator1.java

```java
 package com.mkyong.io.howto;

public class GetFileSeparator {

    public static void main(String[] args) {

        // unix / , windows \
        String separator = System.getProperty("file.separator");
        System.out.println(separator);

    }

} 
```

## 2.Java 九

Java 7，九`FileSystems.getDefault().getSeparator()`中的一个。

GetFileSeparator2.java

```java
 package com.mkyong.io.howto;

import java.nio.file.FileSystems;

public class GetFileSeparator2 {

    public static void main(String[] args) {

        // unix / , windows \
        String separator = FileSystems.getDefault().getSeparator();
        System.out.println(separator);

    }

} 
```

## 3.Java IO

Java IO `File.separator`例子。

GetFileSeparator3.java

```java
 package com.mkyong.io.howto;

import java.io.File;

public class GetFileSeparator3 {

    public static void main(String[] args) {

        // unix / , windows \
        String separator = File.separator;
        System.out.println(separator);

    }

} 
```

## 4.哪一个？

对于`System.getProperty("file.separator")`，我们可以通过`System.setProperty()`或命令行`-Dfile.separator`覆盖该值。`File.separator`和`FileSystems.getDefault().getSeparator()`将返回相同的分离器。

读了这个[遗留文件 I/O 代码的弊端](http://web.archive.org/web/20220629160043/https://docs.oracle.com/javase/tutorial/essential/io/legacy.html)，挑选了 Java NIO `FileSystems.getDefault().getSeparator()`。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220629160043/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [维基百科–路径](http://web.archive.org/web/20220629160043/https://en.wikipedia.org/wiki/Path_(computing))
*   [文件 JavaDoc](http://web.archive.org/web/20220629160043/https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html)
*   [文件 JavaDoc](http://web.archive.org/web/20220629160043/https://docs.oracle.com/javase/8/docs/api/java/io/File.html)
*   [Java 系统属性](http://web.archive.org/web/20220629160043/https://docs.oracle.com/javase/tutorial/essential/environment/sysprop.html)
*   [Java 打印所有系统属性](/web/20220629160043/https://mkyong.com/java/how-to-list-all-system-properties-key-and-value-in-java/)

<input type="hidden" id="mkyong-current-postId" value="15985">