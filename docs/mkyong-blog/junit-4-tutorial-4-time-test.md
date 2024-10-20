# JUnit–超时测试

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/junit-4-tutorial-4-time-test/>

如果测试花费的时间超过定义的“超时”时间，将抛出`TestTimedOutException`并标记测试失败。请参见以下示例:

*P.S .用 JUnit 4.12 测试*

## 1.超时示例

这个`timeout`例子只适用于单个测试方法。超时值以毫秒为单位。

TimeoutTest.java

```java
 package com.mkyong;

import org.junit.Test;

public class TimeoutTest {

    //This test will always failed :)
    @Test(timeout = 1000)
    public void infinity() {
        while (true) ;
    }

    //This test can't run more than 5 seconds, else failed
    @Test(timeout = 5000)
    public void testSlowMethod() {
        //...
    }

} 
```

这个超时测试对于测试方法性能很有用。

 ## 2.全局超时规则示例

这个例子向你展示了如何创建一个全局超时规则，这个规则将应用于一个类中的所有测试方法。

TimeoutTest.java

```java
 package com.mkyong;

import org.junit.Rule;
import org.junit.Test;
import org.junit.rules.Timeout;

import java.util.concurrent.TimeUnit;

public class TimeoutRuleTest {

    //global timeout rule
    @Rule
    public Timeout globalTimeout = Timeout.seconds(1);

	//This test will be failed, because it will take more than 1 second to finish!
    @Test
    public void testSlowMethod1() throws InterruptedException {
        //...
        TimeUnit.SECONDS.sleep(5000);
    }

	//passed
    @Test
    public void testSlowMethod2() {
        //...
    }

} 
```

在上面的例子中，声明了一个全局`Timeout`规则，`testSlowMethod1()`和`testSlowMethod2()`必须在 1 秒内完成测试，否则测试将失败。

这个规则也适用于`@Before`和`@After`方法。

**Note**
All unit test should be fast, and this global timeout rule should be your best helper. ## 参考

1.  [@Test JavaDoc](http://web.archive.org/web/20190223081356/http://junit.sourceforge.net/javadoc/org/junit/Test.html)
2.  [超时规则 JavaDoc](http://web.archive.org/web/20190223081356/http://junit.org/junit4/javadoc/4.12/org/junit/rules/Timeout.html)

[junit](http://web.archive.org/web/20190223081356/http://www.mkyong.com/tag/junit/) [unit test](http://web.archive.org/web/20190223081356/http://www.mkyong.com/tag/unit-test/)







