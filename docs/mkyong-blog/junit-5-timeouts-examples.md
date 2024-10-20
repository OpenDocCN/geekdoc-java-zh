# JUnit 5 超时示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/junit5/junit-5-timeouts-examples/>

在 JUnit 5 中，如果执行时间超过给定的持续时间，我们可以使用`@Timeout`使测试失败。

*用 JUnit 5.5.2 测试的 PS*

## 1.@超时

TimeOutExample1.java

```java
 package com.mkyong.timeout;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.Timeout;

import java.util.concurrent.TimeUnit;

public class TimeOutExample1 {

    // timed out after 5 seconds
    @BeforeEach
    @Timeout(5)
    void setUpDB() throws InterruptedException {
        //TimeUnit.SECONDS.sleep(10);
    }

    // timed out after 500 miliseconds
    @Test
    @Timeout(value = 500, unit = TimeUnit.MILLISECONDS)
    void test_this() {
    }

} 
```

## 2.assertTimeout

2.1 我们也可以使用`assertTimeout`让测试超时。

TimeOutExample2.java

```java
 package com.mkyong.timeout;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.Timeout;

import java.time.Duration;
import java.util.concurrent.TimeUnit;

import static org.junit.jupiter.api.Assertions.assertTimeout;

public class TimeOutExample2 {

    // timed out after 5 seconds
    @Test
    void test_timeout_fail() {
        // assertTimeout(Duration.ofSeconds(5), () -> delaySecond(10)); // this will fail

        assertTimeout(Duration.ofSeconds(5), () -> delaySecond(1)); // pass
    }

    void delaySecond(int second) {
        try {
            TimeUnit.SECONDS.sleep(second);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

} 
```

## 下载源代码

$ git clone [https://github.com/mkyong/junit-examples](http://web.archive.org/web/20221225035521/https://github.com/mkyong/junit-examples)
$ cd junit5-examples
$ check src/test/java/com/mkyong/timeout/*.java

# 参考

*   [JUnit 5 超时](http://web.archive.org/web/20221225035521/https://junit.org/junit5/docs/current/user-guide/#writing-tests-declarative-timeouts)
*   [JUnit 5 断言](http://web.archive.org/web/20221225035521/https://junit.org/junit5/docs/current/user-guide/#writing-tests-assertions)

<input type="hidden" id="mkyong-current-postId" value="15264">