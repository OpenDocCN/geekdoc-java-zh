# 如何在 Java 中获得一个目录的大小

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-get-size-of-a-directory-in-java/>

在 Java 中，我们可以用`Files.size`来获取一个文件的大小；对于目录或文件夹的大小，我们需要递归地计算一个目录的大小(所有文件的`Files.size`之和)。

这个例子展示了几种获取目录或文件夹大小的常用方法。

1.  `FileVisitor` (Java 7)
2.  `Files.walk` (Java 8)
3.  `FileUtils.sizeOfDirectory`(阿帕奇通用 IO)
4.  所有文件的总和`file.length`。(传统 IO)

## 1.目录大小–file visitor(Java 7)

这个例子使用了`FileVisitor`来访问指定路径中的所有文件，并对所有文件的大小求和。

```java
 // size of directory in bytes
  public static long getDirectorySizeJava7(Path path) {

      AtomicLong size = new AtomicLong(0);

      try {

          Files.walkFileTree(path, new SimpleFileVisitor<>() {

              @Override
              public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) {
                  // sum size of all visit file
                  size.addAndGet(attrs.size());
                  return FileVisitResult.CONTINUE;
              }

              @Override
              public FileVisitResult visitFileFailed(Path file, IOException e) {
                  System.out.printf("Failed to get size of %s%n%s", file, e);
                  return FileVisitResult.CONTINUE;
              }

          });
      } catch (IOException e) {
          System.out.printf("%s", e);
      }

      return size.get();

  } 
```

## 2.目录大小–files . walk(Java 8)

这个例子使用 Java 8 `Files.walk`和`Stream`递归地遍历一个目录，并对所有文件的大小求和。

```java
 // size of directory in bytes
  public static long getDirectorySizeJava8(Path path) {

      long size = 0;

      // need close Files.walk
      try (Stream<Path> walk = Files.walk(path)) {

          size = walk
                  //.peek(System.out::println) // debug
                  .filter(Files::isRegularFile)
                  .mapToLong(p -> {
                      // ugly, can pretty it with an extract method
                      try {
                          return Files.size(p);
                      } catch (IOException e) {
                          System.out.printf("Failed to get size of %s%n%s", p, e);
                          return 0L;
                      }
                  })
                  .sum();

      } catch (IOException e) {
          System.out.printf("IO errors %s", e);
      }

      return size;

  } 
```

## 3.目录大小–FileUtils(Apache 公共 IO)

这个例子使用 Apache Common IO `FileUtils.sizeOfDirectory`来获取目录的大小。

pom.xml

```java
 <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.7</version>
  </dependency> 
```

```java
 public static long getDirectorySizeCommonIO(File dir) {

      return FileUtils.sizeOfDirectory(dir);

  } 
```

## 4.传统 IO

在遗留 IO 中，我们可以使用`file.listFiles()`递归地遍历一个目录，使用`file.length()`对所有文件的大小求和。

```java
 public static long getDirectorySizeLegacy(File dir) {

      long length = 0;
      File[] files = dir.listFiles();
      if (files != null) {
          for (File file : files) {
              if (file.isFile())
                  length += file.length();
              else
                  length += getDirectorySizeLegacy(file);
          }
      }
      return length;

  } 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003336/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [遍历文件树](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/tutorial/essential/io/walk.html)
*   [文件 JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)
*   [FileUtils JavaDoc](http://web.archive.org/web/20220619003336/https://commons.apache.org/proper/commons-io/javadocs/api-2.7/org/apache/commons/io/FileUtils.html)

<input type="hidden" id="mkyong-current-postId" value="16074">