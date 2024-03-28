# 问题和练习：字符和字符串

> 原文：[`docs.oracle.com/javase/tutorial/java/data/QandE/characters-questions.html`](https://docs.oracle.com/javase/tutorial/java/data/QandE/characters-questions.html)

## 问题

1.  以下字符串构建器的初始容量是多少？

    ```java
    StringBuilder sb = new StringBuilder("Able was I ere I saw Elba.");

    ```

1.  考虑以下字符串：

    ```java
    String hannah = "Did Hannah see bees? Hannah did.";

    ```

    1.  表达式`hannah.length()`显示的值是多少？

    1.  方法调用`hannah.charAt(12)`返回的值是多少？

    1.  编写一个表达式，引用`hannah`所指的字符串中的字母`b`。

1.  以下表达式返回的字符串有多长？这个字符串是什么？

    ```java
    "Was it a car or a cat I saw?".substring(9, 12)

    ```

1.  在下面的程序中，名为`ComputeResult`，每个编号行执行后`result`的值是多少？

    ```java
    public class ComputeResult {
        public static void main(String[] args) {
            String original = "software";
            StringBuilder result = new StringBuilder("hi");
            int index = original.indexOf('a');

    /*1*/   result.setCharAt(0, original.charAt(0));
    /*2*/   result.setCharAt(1, original.charAt(original.length()-1));
    /*3*/   result.insert(1, original.charAt(4));
    /*4*/   result.append(original.substring(1,4));
    /*5*/   result.insert(3, (original.substring(index, index+2) + " ")); 

            System.out.println(result);
        }
    }

    ```

## 练习

1.  展示两种方法将以下两个字符串连接在一起以得到字符串`"Hi, mom."`：

    ```java
    String hi = "Hi, ";
    String mom = "mom.";

    ```

1.  编写一个程序，从你的全名中计算出你的缩写并显示出来。

1.  一个变位词是由另一个单词或短语的字母重新排列而成的单词或短语；例如，“parliament”是“partial men”的变位词，“software”是“swear oft”的变位词。编写一个程序，判断一个字符串是否是另一个字符串的变位词。该程序应忽略空格和标点符号。

检查你的答案。
