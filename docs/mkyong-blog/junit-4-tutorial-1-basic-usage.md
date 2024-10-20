# JUnit–基本注释示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/junit-4-tutorial-1-basic-usage/>

这里有一些您应该了解的基本 JUnit 注释:

1.  @ before class–在类中的任何测试方法之前运行一次，`public static void`
2.  @ after class–在运行完该类中的所有测试后运行一次，`public static void`
3.  @ Before–在@Test 之前运行，`public void`
4.  @ After–在@Test，`public void`之后运行
5.  @ Test–这是要运行的测试方法，`public void`

*P.S .用 JUnit 4.12 测试*

BasicAnnotationTest.java

```java
 package com.mkyong;

import org.junit.*;

public class BasicAnnotationTest {

    // Run once, e.g. Database connection, connection pool
    @BeforeClass
    public static void runOnceBeforeClass() {
        System.out.println("@BeforeClass - runOnceBeforeClass");
    }

    // Run once, e.g close connection, cleanup
    @AfterClass
    public static void runOnceAfterClass() {
        System.out.println("@AfterClass - runOnceAfterClass");
    }

    // Should rename to @BeforeTestMethod
    // e.g. Creating an similar object and share for all @Test
    @Before
    public void runBeforeTestMethod() {
        System.out.println("@Before - runBeforeTestMethod");
    }

    // Should rename to @AfterTestMethod
    @After
    public void runAfterTestMethod() {
        System.out.println("@After - runAfterTestMethod");
    }

    @Test
    public void test_method_1() {
        System.out.println("@Test - test_method_1");
    }

    @Test
    public void test_method_2() {
        System.out.println("@Test - test_method_2");
    }

} 
```

输出

```java
 @BeforeClass - runOnceBeforeClass

@Before - runBeforeTestMethod
@Test - test_method_1
@After - runAfterTestMethod

@Before - runBeforeTestMethod
@Test - test_method_2
@After - runAfterTestMethod

@AfterClass - runOnceAfterClass 
```

## 参考

1.  [JUnit @Before](http://web.archive.org/web/20210509081808/http://junit.org/junit4/javadoc/4.12/org/junit/Before.html)
2.  [JUnit @BeforeClass](http://web.archive.org/web/20210509081808/http://junit.org/junit4/javadoc/4.12/org/junit/BeforeClass.html)

Tags : [junit](http://web.archive.org/web/20210509081808/https://mkyong.com/tag/junit/) [unit test](http://web.archive.org/web/20210509081808/https://mkyong.com/tag/unit-test/)<input type="hidden" id="mkyong-current-postId" value="1384">