# 如何在 Java 中获取文件扩展名

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-get-file-extension-in-java/>

本文展示了如何在 Java 中获取文件的文件扩展名。

主题

1.  [获取文件扩展名正常](#get-file-extension-normal-version)
2.  [获取文件扩展名严格](#get-file-extension-strict-version)
3.  [获取文件扩展名硬编码](#get-file-extension-hardcode-version)
4.  [Apache 通用 IO–filename utils](#apache-common-io)

```java
 Path: /path/foo.txt                  -> File Extension: txt
  Path: .                              -> File Extension:
  Path: ..                             -> File Extension:
  Path: /path/run.exe                  -> File Extension: exe
  Path: /path/makefile                 -> File Extension:
  Path: /path/.htaccess                -> File Extension: htaccess
  Path: /path/.tar.gz                  -> File Extension: gz
  Path: /path/../makefile              -> File Extension:
  Path: /path/dir.test/makefile        -> File Extension: 
```

查看上面从[获取文件扩展名严格示例](#get-file-extension-strict-version)的输出。对于没有扩展名的文件，我们将显示为空。

## 1。获取文件扩展名，普通版本。

这个例子展示了如何使用 [String#lastIndexOf](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#lastIndexOf(int)) 来获取文件的文件扩展名，它应该适合大多数情况。

```java
 String extension = "";

  int index = fileName.lastIndexOf('.');
  if (index > 0) {
      extension = fileName.substring(index + 1);
  } 
```

但是，对于以下情况，上述方法将失败:目录名或文件路径包含一个点或双点，并且文件没有扩展名。

```java
 Path: /path/../makefile              -> File Extension: /makefile
  Path: /path/dir.test/makefile        -> File Extension: test/makefile 
```

完整的例子。

GetFileExtension1.java

```java
 package com.mkyong.io.howto;

public class GetFileExtension1 {

    private static final String OUTPUT_FORMAT = "Path: %-30s -> File Extension: %s";

    public static void main(String[] args) {

        String[] files = {
                "/path/foo.txt",
                ".",
                "..",
                "/path/run.exe",
                "/path/makefile",
                "/path/.htaccess",
                "/path/.tar.gz",
                "/path/../makefile",
                "/path/dir.test/makefile"
        };

        for (String file : files) {
            String output = String.format(OUTPUT_FORMAT, file, getFileExtension(file));
            System.out.println(output);
        }

    }

    /**
     * Fail for below cases
     * <p>
     * "/path/../makefile",
     * "/path/dir.test/makefile"
     */
    public static String getFileExtension(String fileName) {
        if (fileName == null) {
            throw new IllegalArgumentException("fileName must not be null!");
        }

        String extension = "";

        int index = fileName.lastIndexOf('.');
        if (index > 0) {
            extension = fileName.substring(index + 1);
        }

        return extension;

    }

} 
```

输出

Terminal

```java
 Path: /path/foo.txt                  -> File Extension: txt
Path: .                              -> File Extension:
Path: ..                             -> File Extension:
Path: /path/run.exe                  -> File Extension: exe
Path: /path/makefile                 -> File Extension:
Path: /path/.htaccess                -> File Extension: htaccess
Path: /path/.tar.gz                  -> File Extension: gz
Path: /path/../makefile              -> File Extension: /makefile
Path: /path/dir.test/makefile        -> File Extension: test/makefile 
```

## 2。获取文件扩展名，严格版本。

这个例子实现了额外的检查，因为目录包含一个点，文件没有扩展名。

```java
 Path: /path/../makefile              -> File Extension:
  Path: /path/dir.test/makefile        -> File Extension: 
```

额外的检查确保最后一个文件扩展名索引(位置)总是在最后一个文件分隔符(Windows 或 Unix)之后。

```java
 if (indexOfLastExtension > indexOflastSeparator) {
      extension = fileName.substring(indexOfLastExtension + 1);
  } 
```

完整的例子。

GetFileExtension2.java

```java
 package com.mkyong.io.howto;

import java.util.Map;

public class GetFileExtension2 {

    private static final String OUTPUT_FORMAT = "Path: %-30s -> File Extension: %s";
    private static final String WINDOWS_FILE_SEPARATOR = "\\";
    private static final String UNIX_FILE_SEPARATOR = "/";
    private static final String FILE_EXTENSION = ".";

    public static void main(String[] args) {

        String[] files = {
                "/path/foo.txt",
                ".",
                "..",
                "/path/run.exe",
                "/path/makefile",
                "/path/.htaccess",
                "/path/.tar.gz",
                "/path/../makefile",
                "/path/dir.test/makefile"
        };

        for (String file : files) {
            String output = String.format(OUTPUT_FORMAT, file, getFileExtensionImproved(file));
            System.out.println(output);
        }

    }

    /**
     * Add extra checking for below cases
     * <p>
     * "/path/../makefile",
     * "/path/dir.test/makefile"
     */
    public static String getFileExtensionImproved(String fileName) {

        if (fileName == null) {
            throw new IllegalArgumentException("fileName must not be null!");
        }

        String extension = "";

        int indexOfLastExtension = fileName.lastIndexOf(FILE_EXTENSION);

        // check last file separator, windows and unix
        int lastSeparatorPosWindows = fileName.lastIndexOf(WINDOWS_FILE_SEPARATOR);
        int lastSeparatorPosUnix = fileName.lastIndexOf(UNIX_FILE_SEPARATOR);

        // takes the greater of the two values, which mean last file separator
        int indexOflastSeparator = Math.max(lastSeparatorPosWindows, lastSeparatorPosUnix);

        // make sure the file extension appear after the last file separator
        if (indexOfLastExtension > indexOflastSeparator) {
            extension = fileName.substring(indexOfLastExtension + 1);
        }

        return extension;

    }

} 
```

输出

Terminal

```java
 Path: /path/foo.txt                  -> File Extension: txt
Path: .                              -> File Extension:
Path: ..                             -> File Extension:
Path: /path/run.exe                  -> File Extension: exe
Path: /path/makefile                 -> File Extension:
Path: /path/.htaccess                -> File Extension: htaccess
Path: /path/.tar.gz                  -> File Extension: gz
Path: /path/../makefile              -> File Extension:
Path: /path/dir.test/makefile        -> File Extension: 
```

## 3。获取文件扩展名，硬编码版本

这个例子展示了如何硬编码一些文件扩展名并显示预定义的结果。例如，对于文件扩展名`.tar.gz`，我们希望显示`tar.gz`，而不是`gz`。

GetFileExtension3.java

```java
 package com.mkyong.io.howto;

import java.util.HashMap;
import java.util.Map;
import java.util.stream.Collectors;

public class GetFileExtension3 {

    private static final String OUTPUT_FORMAT = "Path: %-30s -> File Extension: %s";
    private static final String WINDOWS_FILE_SEPARATOR = "\\";
    private static final String UNIX_FILE_SEPARATOR = "/";
    private static final String FILE_EXTENSION = ".";
    private static final Map<String, String> KNOWN_EXTENSION = createKnownExtensionMap();

    public static void main(String[] args) {

        String[] files = {
                "/path/foo.txt",
                ".",
                "..",
                "/path/run.exe",
                "/path/makefile",
                "/path/.htaccess",
                "/path/.tar.gz",
                "/path/../makefile",
                "/path/dir.test/makefile"
        };

        for (String file : files) {
            String output = String.format(OUTPUT_FORMAT, file, getFileExtensionKnownExtension(file));
            System.out.println(output);
        }

    }

    public static String getFileExtensionImproved(String fileName) {

        if (fileName == null) {
            throw new IllegalArgumentException("fileName must not be null!");
        }

        String extension = "";

        int indexOfLastExtension = fileName.lastIndexOf(FILE_EXTENSION);

        int indexOflastSeparator = Math.max(
              fileName.lastIndexOf(WINDOWS_FILE_SEPARATOR),
              fileName.lastIndexOf(UNIX_FILE_SEPARATOR)
        );

        if (indexOfLastExtension > indexOflastSeparator) {
            extension = fileName.substring(indexOfLastExtension + 1);
        }

        return extension;

    }

    // hardcoded
    public static String getFileExtensionKnownExtension(final String fileName) {

        if (fileName == null) {
            throw new IllegalArgumentException("fileName must not be null!");
        }

        // if the file name is end with the hard coded extension.
        // Java 8 stream, loop map if key matches get value
        String extension = KNOWN_EXTENSION
                .entrySet()
                .stream()
                .filter(x -> fileName.endsWith(x.getKey()))
                .map(x -> x.getValue())
                .collect(Collectors.joining());

        if ("".equals(extension)) {
            extension = getFileExtensionImproved(fileName); // see example 2
        }

        return extension;

    }

    private static Map<String, String> createKnownExtensionMap() {
        Map<String, String> result = new HashMap<>();
        result.put(".tar.gz", "tar.gz");    // if .tar.gz, gets tar.gz
        result.put("makefile", "make");     // if makefile, get make
        //...extra
        return result;
    }

} 
```

输出

Terminal

```java
 Path: /path/foo.txt                  -> File Extension: txt
Path: .                              -> File Extension:
Path: ..                             -> File Extension:
Path: /path/run.exe                  -> File Extension: exe
Path: /path/makefile                 -> File Extension: make
Path: /path/.htaccess                -> File Extension: htaccess
Path: /path/.tar.gz                  -> File Extension: tar.gz
Path: /path/../makefile              -> File Extension: make
Path: /path/dir.test/makefile        -> File Extension: make 
```

## 4。阿帕奇通用 IO

对于 Apache `commons-io`，我们可以使用`FilenameUtils.getExtension(fileName)`来获取一个文件的文件扩展名。

pom.xml

```java
 <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.7</version>
  </dependency> 
```

完整的例子。

GetFileExtension4.java

```java
 package com.mkyong.io.howto;

import org.apache.commons.io.FilenameUtils;

public class GetFileExtension4 {

    private static final String OUTPUT_FORMAT = "Path: %-30s -> File Extension: %s";

    public static void main(String[] args) {

        String[] files = {
                "/path/foo.txt",
                ".",
                "..",
                "/path/run.exe",
                "/path/makefile",
                "/path/.htaccess",
                "/path/.tar.gz",
                "/path/../makefile",
                "/path/dir.test/makefile"
        };

        for (String file : files) {
            String output = String.format(OUTPUT_FORMAT, file, FilenameUtils.getExtension(fileName));
            System.out.println(output);
        }

    }

} 
```

输出

Terminal

```java
 Path: /path/foo.txt                  -> File Extension: txt
Path: .                              -> File Extension:
Path: ..                             -> File Extension:
Path: /path/run.exe                  -> File Extension: exe
Path: /path/makefile                 -> File Extension:
Path: /path/.htaccess                -> File Extension: htaccess
Path: /path/.tar.gz                  -> File Extension: gz
Path: /path/../makefile              -> File Extension:
Path: /path/dir.test/makefile        -> File Extension: 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003336/https://github.com/mkyong/core-java)

$ cd java-io

## 参考文献

*   Apache common me
*   [FilenameUtils JavaDoc](http://web.archive.org/web/20220619003336/https://commons.apache.org/proper/commons-io/apidocs/org/apache/commons/io/FilenameUtils.html)
*   [String#lastIndexOf JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#lastIndexOf(int))
*   [如何在 Java 中获取文件的文件路径](/web/20220619003336/https://mkyong.com/java/how-to-get-the-filepath-of-a-file-in-java/)
*   [维基百科–文件扩展名](http://web.archive.org/web/20220619003336/https://en.wikipedia.org/wiki/Filename_extension)

<input type="hidden" id="mkyong-current-postId" value="16372">