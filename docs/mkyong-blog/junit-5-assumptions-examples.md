# JUnit 5 假设示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/junit5/junit-5-assumptions-examples/>

![JUnit 5 assumptions](img/845abd6e8fbdf22c9ff3a98d95df0201.png)

本文向您展示了如何使用 JUnit 5 假设来执行条件测试。

使用的技术:

*   Maven 3.6
*   Java 8
*   JUnit 5.5.2

## 1.假设

1.1 如果`assumeTrue()`条件为真，则运行测试，否则中止测试。

1.2`assumingThat()`更加灵活，它允许部分代码作为条件测试运行。

AssumptionsTest.java

```java
 package com.mkyong;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assumptions.assumeTrue;
import static org.junit.jupiter.api.Assumptions.assumingThat;

public class AssumptionsTest {

    // Output: org.opentest4j.TestAbortedException: Assumption failed: assumption is not true
    @DisplayName("Run this if `assumeTrue` condition is true, else aborting this test")
    @Test
    void testOnlyOnDevEnvElseAbort() {
        assumeTrue("DEV".equals(System.getenv("APP_MODE")));
        assertEquals(2, 1 + 1);
    }

    // Output: org.opentest4j.TestAbortedException: Assumption failed: Aborting test: not on developer environment
    @DisplayName("Run this if `assumeTrue` condition is true, else aborting this test (Custom Message)")
    @Test
    void testOnlyOnDevEnvElseAbortWithCustomMsg() {
        assumeTrue("DEV".equals(System.getenv("APP_MODE")), () -> "Aborting test: not on developer environment");
        assertEquals(2, 1 + 1);
    }

    @Test
    void testAssumingThat() {

        // run these assertions always, just like normal test
        assertEquals(2, 1 + 1);

        assumingThat("DEV".equals(System.getenv("APP_MODE")),
                () -> {
                    // run this only if assumingThat condition is true
                    assertEquals(2, 1 + 1);
                });

        // run these assertions always, just like normal test
        assertEquals(2, 1 + 1);

    }

} 
```

输出

![ide output](img/0acd584c12490c80d26100a0da49d9f1.png)

## 下载源代码

$ git clone [https://github.com/mkyong/junit-examples](http://web.archive.org/web/20221225035521/https://github.com/mkyong/junit-examples)
$ cd junit5-examples
$ check src/test/java/com/mkyong/AssumptionsTest.java

# 参考

*   [JUnit 5 官方网站](http://web.archive.org/web/20221225035521/https://junit.org/junit5/)
*   [JUnit 5 假设 JavaDoc](http://web.archive.org/web/20221225035521/https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Assumptions.html)

<input type="hidden" id="mkyong-current-postId" value="15186">