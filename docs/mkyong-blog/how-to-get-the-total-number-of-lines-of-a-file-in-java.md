# Java–计算文件中的行数

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-get-the-total-number-of-lines-of-a-file-in-java/>

本文展示了几个 Java 示例来获取文件中的总行数。步骤是相似的:

1.  打开文件。
2.  逐行读取，每行增加计数+ 1。
3.  关闭文件。
4.  读伯爵。

测试 Java 方法:

1.  `Files.lines`
2.  `BufferedReader`
3.  `LineNumberReader`
4.  `BufferedInputStream`

在本文的最后，我们还展示了在一个包含 500 万行、每行 1053 个字符的大文件中计算总行数的不同方法的性能。

## 1.Files.lines (Java 8)

这个`Files.lines`是最简单的实现。

```java
 public static long countLineJava8(String fileName) {

      Path path = Paths.get(fileName);

      long lines = 0;
      try {

          // much slower, this task better with sequence access
          //lines = Files.lines(path).parallel().count();

          lines = Files.lines(path).count();

      } catch (IOException e) {
          e.printStackTrace();
      }

      return lines;

  } 
```

## 2.缓冲阅读器

本例使用`BufferedReader`逐行读取，并增加计数。

```java
 public static long countLineBufferedReader(String fileName) {

      long lines = 0;
      try (BufferedReader reader = new BufferedReader(new FileReader(fileName))) {
          while (reader.readLine() != null) lines++;
      } catch (IOException e) {
          e.printStackTrace();
      }
      return lines;

  } 
```

## 3.行号阅读器

这个`LineNumberReader`和上面的`BufferedReader`类似。

```java
 public static long countLineNumberReader(String fileName) {

      File file = new File(fileName);

      long lines = 0;

      try (LineNumberReader lnr = new LineNumberReader(new FileReader(file))) {

          while (lnr.readLine() != null) ;

          lines = lnr.getLineNumber();

      } catch (IOException e) {
          e.printStackTrace();
      }

      return lines;

  } 
```

## 4.缓冲输入流

这个`BufferedInputStream`例子是从这个 [StackOverflow 答案](http://web.archive.org/web/20221201143630/https://stackoverflow.com/questions/453018/number-of-lines-in-a-file-in-java)里抄来的。

```java
 public static long countLineFast(String fileName) {

      long lines = 0;

      try (InputStream is = new BufferedInputStream(new FileInputStream(fileName))) {
          byte[] c = new byte[1024];
          int count = 0;
          int readChars = 0;
          boolean endsWithoutNewLine = false;
          while ((readChars = is.read(c)) != -1) {
              for (int i = 0; i < readChars; ++i) {
                  if (c[i] == '\n')
                      ++count;
              }
              endsWithoutNewLine = (c[readChars - 1] != '\n');
          }
          if (endsWithoutNewLine) {
              ++count;
          }
          lines = count;
      } catch (IOException e) {
          e.printStackTrace();
      }

      return lines;
  } 
```

## 5.基准

5.1 创建一个包含 500 万行、每行 1053 个字符、文件大小为 5G 的大文件。

```java
 public static void writeLargeFile() {

      String fileName = "/home/mkyong/large-file.txt";

      // 1053 chars per line
      String content = "Hello 123456 ";
      content = content + content + content;
      content = content + content + content;
      content = content + content + content;
      content = content + content + content;

      System.out.println(content.length()); // 1053

      try (BufferedWriter bw = new BufferedWriter(new FileWriter(fileName))) {

          for (int i = 0; i < 5_000_000; i++) {
              bw.write(content);
              bw.write(System.lineSeparator());
          }

      } catch (IOException e) {
          e.printStackTrace();
      }

  } 
```

5.2 同样的方法重新运行 5-10 次，得到平均基准，结果如下:

1.  `Files.lines`–6-8 秒。
2.  `BufferedReader`–6-8 秒。
3.  `LineNumberReader`–6-8 秒。
4.  `BufferedInputStream`–4-5 秒。

这个`BufferedInputStream` ( [StackOverflow 回答](http://web.archive.org/web/20221201143630/https://stackoverflow.com/questions/453018/number-of-lines-in-a-file-in-java))，是计算一个大文件(5G 文件大小，500 万行)行数的最快方法。尽管如此，差别并不明显，而且实现容易出错，有点复杂。如果我们测试一个更小的文件，比如 1G 的文件大小和 100 万行，我们几乎注意不到区别。

最后，Java NIO `Files.lines`易于使用，性能也没有太大的不同，是计算文件行数的最佳选择。

## 6.wc -l

在 Linux 上，命令`wc -l`是计算文件行数的最快方法。

Terminal

```java
 $ time wc -l large-file.txt
5000000 large-file.txt

real	0m2.344s
user	0m0.113s
sys	0m1.306s

$ time wc -l large-file.txt
5000000 large-file.txt

real	0m0.630s
user	0m0.092s
sys	0m0.537s 
```

对`wc -l`命令背后的算法有什么建议和想法吗？

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221201143630/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [文件 JavaDoc](http://web.archive.org/web/20221201143630/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)
*   [LineNumberReader JavaDoc](http://web.archive.org/web/20221201143630/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/LineNumberReader.html)

<input type="hidden" id="mkyong-current-postId" value="5459">