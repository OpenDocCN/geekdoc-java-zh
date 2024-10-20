# Java——如何打印金字塔

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-print-a-pyramid/>

一个 Java 例子，打印半金字塔和全金字塔，好玩。

CreatePyramid.java

```java
 package com.mkyong;

import java.util.Collections;

public class CreatePyramid {

    public static void main(String[] args) {

        int rows = 5;

        System.out.println("\n1\. Half Pyramid\n");
        for (int i = 0; i < rows; i++) {

            for (int j = 0; j <= i; j++) {
                System.out.print("*");
            }

            System.out.println("");
        }

        System.out.println("\n2\. Full Pyramid\n");
        for (int i = 0; i < rows; i++) {

            for (int j = 0; j < rows - i; j++) {
                System.out.print(" ");
            }

            for (int k = 0; k <= i; k++) {
                System.out.print("* ");
            }

            System.out.println("");
        }

        //java 8 , one line
        System.out.println("\n3\. Full Pyramid (Compact)\n");
        for (int i = 0; i < rows; i++) {

            System.out.println(String.join("", Collections.nCopies(5 - i - 1, " "))
                    + String.join("", Collections.nCopies(2 * i + 1, "*")));

        }

        // java 8
        System.out.println("\n4\. Inverted Pyramid\n");
        for (int i = rows; i > 0; i--) {

            System.out.println(String.join("", Collections.nCopies(5 - i, " "))
                    + String.join("", Collections.nCopies(2 * i - 1, "*")));

        }

    }

} 
```

输出

```java
 1\. Half Pyramid

*
**
***
****
*****

2\. Full Pyramid

     * 
    * * 
   * * * 
  * * * * 
 * * * * * 

3\. Full Pyramid (Compact)

    *
   ***
  *****
 *******
*********

4\. Inverted Pyramid

*********
 *******
  *****
   ***
    * 
```

## 参考

1.  [collections . n copies JavaDoc](http://web.archive.org/web/20221225035539/https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#nCopies-int-T-)
2.  [Python–打印金字塔](http://web.archive.org/web/20221225035539/http://www.mkyong.com/python/python-how-to-print-a-pyramid/)

<input type="hidden" id="mkyong-current-postId" value="14631">