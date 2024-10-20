# 如何在 Java 中计算运行/执行时间

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-do-calculate-elapsed-execute-time-in-java/>

在 Java 中，您可以使用以下方法来测量 Java 中的运行时间。

## 1.System.nanoTime()

这是测量 Java 运行时间的推荐解决方案。

ExecutionTime1.java

```java
 package com.mkyong.time;

import java.util.concurrent.TimeUnit;

public class ExecutionTime1 {

    public static void main(String[] args) throws InterruptedException {

		//start
        long lStartTime = System.nanoTime();

		//task
        calculation();

		//end
        long lEndTime = System.nanoTime();

		//time elapsed
        long output = lEndTime - lStartTime;

        System.out.println("Elapsed time in milliseconds: " + output / 1000000);

    }

    private static void calculation() throws InterruptedException {

        //Sleep 2 seconds
        TimeUnit.SECONDS.sleep(2);

    }
} 
```

输出可能会有所不同。

```java
 2004 
```

## 2.System.currentTimeMillis()

ExecutionTime2.java

```java
 package com.mkyong.time;

import java.util.concurrent.TimeUnit;

public class ExecutionTime2 {

    public static void main(String[] args) throws InterruptedException {

        long lStartTime = System.currentTimeMillis();

        calculation();

        long lEndTime = System.currentTimeMillis();

        long output = lEndTime - lStartTime;

        System.out.println("Elapsed time in milliseconds: " + output);

    }

    private static void calculation() throws InterruptedException {

        //Sleep 2 seconds
        TimeUnit.SECONDS.sleep(2);

    }
} 
```

输出可能会有所不同。

```java
 2006 
```

## 3.Instant.now()。托普奇米利()

在 Java 8 中，你可以尝试新的`java.time.Instant`

ExecutionTime3.java

```java
 package com.mkyong.time;

import java.time.Instant;
import java.util.concurrent.TimeUnit;

public class ExecutionTime3 {

    public static void main(String[] args) throws InterruptedException {

        long lStartTime = Instant.now().toEpochMilli();

        calculation();

        long lEndTime = Instant.now().toEpochMilli();

        long output = lEndTime - lStartTime;

        System.out.println("Elapsed time in milliseconds: " + output);

    }

    private static void calculation() throws InterruptedException {

        //Sleep 2 seconds
        TimeUnit.SECONDS.sleep(2);

    }
} 
```

输出可能会有所不同。

```java
 2006 
```

## 4.日期()。getTime()

ExecutionTime4.java

```java
 package com.mkyong.time;

import java.util.Date;
import java.util.concurrent.TimeUnit;

public class ExecutionTime4 {

    public static void main(String[] args) throws InterruptedException {

        long lStartTime = new Date().getTime();

        calculation();

        long lEndTime = new Date().getTime();

        long output = lEndTime - lStartTime;

        System.out.println("Elapsed time in milliseconds: " + output);

    }

    private static void calculation() throws InterruptedException {

        //Sleep 2 seconds
        TimeUnit.SECONDS.sleep(2);

    }
} 
```

输出可能会有所不同。

```java
 2007 
```

## 参考

1.  [stack overflow–system . nano time()完全没用吗？](http://web.archive.org/web/20220627214126/https://stackoverflow.com/questions/510462/is-system-nanotime-completely-useless)
2.  [stack overflow–system . current time millis vs system . nano time](http://web.archive.org/web/20220627214126/https://stackoverflow.com/questions/351565/system-currenttimemillis-vs-system-nanotime)
3.  [系统#nanoTime JavaDoc](http://web.archive.org/web/20220627214126/https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#nanoTime--)
4.  [Instant # toEpochMilli JavaDoc](http://web.archive.org/web/20220627214126/https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#toEpochMilli--)

<input type="hidden" id="mkyong-current-postId" value="889">