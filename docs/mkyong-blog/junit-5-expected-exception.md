# JUnit 5 预期异常

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/junit5/junit-5-expected-exception/>

在 JUnit 5 中，我们可以使用`assertThrows`来断言抛出了异常。

*用 JUnit 5.5.2 测试的 PS*

## 1.未检查的异常

1.1 捕捉运行时异常的 JUnit 示例。

ExceptionExample1.java

```java
 package com.mkyong.assertions;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

public class ExceptionExample1 {

    @Test
    void test_exception() {

        Exception exception = assertThrows(
			ArithmeticException.class, 
			() -> divide(1, 0));

        assertEquals("/ by zero", exception.getMessage());

        assertTrue(exception.getMessage().contains("zero"));

    }

    int divide(int input, int divide) {
        return input / divide;
    }
} 
```

## 2.检查异常

2.1 捕捉自定义/编译时异常的 JUnit 示例。

NameNotFoundException.java

```java
 package com.mkyong.assertions;

public class NameNotFoundException extends Exception {
    public NameNotFoundException(String message) {
        super(message);
    }
} 
```

ExceptionExample2.java

```java
 package com.mkyong.assertions;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

public class ExceptionExample2 {

    @Test
    void test_exception_custom() {
        Exception exception = assertThrows(
			NameNotFoundException.class, 
			() -> findByName("mkyong"));

        assertTrue(exception.getMessage().contains("not found"));
    }

    String findByName(String name) throws NameNotFoundException{
        throw new NameNotFoundException( name + " not found!");
    }
} 
```

## 下载源代码

$ git clone [https://github.com/mkyong/junit-examples](http://web.archive.org/web/20221225035521/https://github.com/mkyong/junit-examples)
$ cd junit5-examples
$ check src/test/java/com/mkyong/assertions/*.java

# 参考

*   [JUnit 5 断言](http://web.archive.org/web/20221225035521/https://junit.org/junit5/docs/current/user-guide/#writing-tests-assertions)
*   [Java 自定义异常示例](/web/20221225035521/https://mkyong.com/java/java-custom-exception-examples/)

<input type="hidden" id="mkyong-current-postId" value="15263">