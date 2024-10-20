# JUnit 5–如何禁用测试？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/junit5/junit-5-how-to-disable-tests/>

![JUnit 5 disabled](img/910e5482ad61dcdf7e19701c83430747.png)

JUnit 5 `@Disabled`示例禁用对整个测试类或单个测试方法的测试。

*用 JUnit 5.5.2 测试的 PS*

**Note**
You can also [disable tests based on conditions](/web/20221225035521/https://mkyong.com/junit5/junit-5-conditional-test-examples/).

## 1.@在方法上禁用

1.1 测试方法`testCustomerServiceGet`被禁用。

DisabledMethodTest.java

```java
 package com.mkyong.disable;

import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class DisabledMethodTest {

    @Disabled("Disabled until CustomerService is up!")
    @Test
    void testCustomerServiceGet() {
        assertEquals(2, 1 + 1);
    }

    @Test
    void test3Plus3() {
        assertEquals(6, 3 + 3);
    }

} 
```

输出–使用 Intellij IDE 运行。

![output](img/728405be59a20a1841cdce4b26eed3f1.png)

## 2.@在课堂上被禁用

2.1 整个测试类将被禁用。

DisabledClassTest.java

```java
 package com.mkyong.disable;

import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assumptions.assumeTrue;

@Disabled("Disabled until bug #2019 has been fixed!")
public class DisabledClassTest {

    @Test
    void test1Plus1() {
        assertEquals(2, 1 + 1);
    }

    @Test
    void test2Plus2() {
        assertEquals(4, 2 + 2);
    }

} 
```

经过测试，它在 Maven 或 Gradle 构建工具中运行正常。

**Note**
However, run the above test under Intellij IDE, the `@Disabled` on class level is NOT working as expected, no idea why?

## 下载源代码

$ git clone [https://github.com/mkyong/junit-examples](http://web.archive.org/web/20221225035521/https://github.com/mkyong/junit-examples)
$ cd junit5-examples
$ check src/test/java/com/mkyong/disable/*.java

# 参考

*   [JUnit 5 禁用测试](http://web.archive.org/web/20221225035521/https://junit.org/junit5/docs/current/user-guide/#writing-tests-disabling)
*   [Maven–如何跳过单元测试](http://web.archive.org/web/20221225035521/https://www.mkyong.com/maven/how-to-skip-maven-unit-test/)

<input type="hidden" id="mkyong-current-postId" value="15239">