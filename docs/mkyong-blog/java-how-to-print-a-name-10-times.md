# Java——如何将一个名字打印 10 次？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-print-a-name-10-times/>

这篇文章向你展示了打印一个名字十次的不同方法。

## 1.环

1.1 For 循环

JavaSample1.java

```java
 package com.mkyong.samples;

public class JavaSample1 {

    public static void main(String[] args) {

        for (int i = 0; i < 10; i++) {
            System.out.println("Java ");
        }

    }

} 
```

输出

```java
 Java
Java
Java
Java
Java
Java
Java
Java
Java
Java 
```

1.2 While 循环

JavaSample2.java

```java
 package com.mkyong.samples;

public class JavaSample2 {

    public static void main(String[] args) {

        int i = 0;
        while (i < 10) {
            System.out.println("Java");
            i++;
        }

    }

} 
```

## 2.阅读和打印

这个例子将从控制台读取输入并打印十次。

JavaSampleReadPrint.java

```java
 package com.mkyong.samples;

import java.util.Scanner;

public class JavaSampleReadPrint {

    public static void main(String[] args) {

        String name = "";
        // read an input and print 10 times
        try (Scanner in = new Scanner(System.in)) {
            System.out.print("Your name: ");
            name = in.nextLine();
        }

        for (int i = 0; i < 10; i++) {
            System.out.println(name);
        }
    }

} 
```

输出

```java
 Your name: mkyong
mkyong
mkyong
mkyong
mkyong
mkyong
mkyong
mkyong
mkyong
mkyong
mkyong 
```

## 3.递归

这个例子将使用递归循环。

JavaSampleReadRecursion.java

```java
 package com.mkyong.samples;

public class JavaSampleReadRecursion {

    public static void main(String[] args) {

        print("mkyong", 10);

    }

    static void print(String name, int times) {

        System.out.println(times + ":" + name);

        if (times > 1) {
            print(name, times - 1);
        }
    }

} 
```

输出

```java
 10:mkyong
9:mkyong
8:mkyong
7:mkyong
6:mkyong
5:mkyong
4:mkyong
3:mkyong
2:mkyong
1:mkyong 
```

## 4.没有循环，没有递归

这个例子很有趣，它打印一个字符串 1000 次没有循环，只是简单的数学。

JavaNoLoop.java

```java
 package com.mkyong.samples;

public class JavaNoLoop {

    public static void main(String[] args) {

        String s1 = "Java\n";
        String s3 = s1 + s1 + s1;
        String s10 = s3 + s3 + s3 + s1;
        String s30 = s10 + s10 + s10;
        String s100 = s30 + s30 + s30 + s10;
        String s300 = s100 + s100 + s100;
        String s1000 = s300 + s300 + s300 + s100;
        System.out.print(s1000);

    }

} 
```

## 5.字符+字符串并替换

JavaCharStrReplace.java

```java
 package com.mkyong.samples;

public class JavaCharStrReplace {

    public static void main(String[] args) {

        char[] chars = new char[10];
        String str = new String(chars);
        System.out.print(str.replace("\0", "Mkyong\n"));

    }
} 
```

## 6.Java 8 字符串连接

6.1 `Collections.nCopies`和`String.join`

JavaStringJoin.java

```java
 package com.mkyong.samples;

import java.util.Collections;

public class JavaStringJoinNCopies {

    public static void main(String[] args) {

        System.out.print(String.join("\n", Collections.nCopies(10, "Mkyong")));

    }
} 
```

6.2 `Arrays.fill`和`String.join`

JavaStringJoinArray.java

```java
 package com.mkyong.samples;

import java.util.Arrays;

public class JavaStringJoinArray {

    public static void main(String[] args) {

        String[] str = new String[10];
        Arrays.fill(str, "Mkyong");
        System.out.println(String.join("\n", str));

    }
} 
```

## 7.Java 8 IntStream.range

JavaIntStream.java

```java
 package com.mkyong.samples;

import java.util.stream.IntStream;

public class JavaIntStream {

    public static void main(String[] args) {

        IntStream.range(0,10).forEach(x->System.out.println("Mkyong"));

    }
} 
```

## 8.Java 11 重复

Java11Repeat.java

```java
 package com.mkyong.samples;

public class Java11Repeat {

    public static void main(String[] args) {

        String str = "Mkyong\n";
        System.out.println(str.repeat(10));

    }
} 
```

## 参考

*   [如何从控制台读取输入–Java](/web/20221206171235/https://mkyong.com/java/how-to-read-input-from-console-java/)
*   [collections . n copies JavaDoc](http://web.archive.org/web/20221206171235/https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html#nCopies(int,%20T))
*   [Java 11 重复](http://web.archive.org/web/20221206171235/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#repeat(int))

<input type="hidden" id="mkyong-current-postId" value="15335">