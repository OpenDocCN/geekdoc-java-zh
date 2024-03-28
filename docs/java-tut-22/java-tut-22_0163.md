# 问题和练习

> 原文：[`docs.oracle.com/javase/tutorial/essential/exceptions/QandE/questions.html`](https://docs.oracle.com/javase/tutorial/essential/exceptions/QandE/questions.html)

## 问题

1.  以下代码是否合法？

    ```java
    try {

    } finally {

    }

    ```

1.  以下处理程序可以捕获哪些异常类型？

    ```java
    catch (Exception e) {

    }

    ```

    使用这种类型的异常处理程序有什么问题？

1.  以下异常处理程序的写法有什么问题？这段代码能编译吗？

    ```java
    try {

    } catch (Exception e) {

    } catch (ArithmeticException a) {

    }

    ```

1.  将第一个列表中的每种情况与第二个列表中的一项进行匹配。

    1.  `int[] A;

        A[0] = 0;`

    1.  JVM 开始运行您的程序，但 JVM 找不到 Java 平台类。（Java 平台类位于`classes.zip`或`rt.jar`中。）

    1.  一个程序正在读取流并达到`流结束`标记。

    1.  在关闭流之前和达到`流结束`标记之后，一个程序尝试再次读取流。

    1.  __ 错误

    1.  __ 已检查异常

    1.  __ 编译错误

    1.  __ 无例外

    ## 练习

    1.  在``ListOfNumbers.java``中添加一个`readList`方法。该方法应从文件中读取`int`值，打印每个值，并将它们附加到向量的末尾。您应该捕获所有适当的错误。您还需要一个包含要读取的数字的文本文件。

    1.  修改以下`cat`方法以便能够编译。

        ```java
        public static void cat(File file) {
            RandomAccessFile input = null;
            String line = null;

            try {
                input = new RandomAccessFile(file, "r");
                while ((line = input.readLine()) != null) {
                    System.out.println(line);
                }
                return;
            } finally {
                if (input != null) {
                    input.close();
                }
            }
        }

        ```

        检查您的答案。
