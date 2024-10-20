# JUnit 5 显示名称

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/junit5/junit-5-display-names/>

在 JUnit 5 中，我们可以使用`@DisplayName`来声明测试类和测试方法的定制显示名称。

*用 JUnit 5.5.2 测试的 PS*

## 1.@DisplayName

1.1 测试类和方法的默认名称。

DisplayNameTest.java

```java
 package com.mkyong.display;

import org.junit.jupiter.api.Test;

public class DisplayNameTest {

    @Test
    void test_spaces_ok() {
    }

    @Test
    void test_spaces_fail() {
    }

} 
```

输出

![output](img/c780b12242ed5507c4397bb888068b36.png)

```java
 +-- JUnit Jupiter [OK]
| '-- DisplayNameTest [OK]
|   +-- test_spaces_ok() [OK]
|   '-- test_spaces_fail() [OK] 
```

1.2 `@DisplayName`

DisplayNameCustomTest.java

```java
 package com.mkyong.display;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

@DisplayName("I'm a Test Class")
public class DisplayNameCustomTest {

    @Test
    @DisplayName("Test with spaces, expected ok")
    void test_spaces_ok() {
    }

    @DisplayName("Test with spaces, expected failed")
    @Test
    void test_spaces_fail() {
    }

} 
```

输出

![output](img/f5d6960b5109cede5f68ff8db1fe1e26.png)

```java
 +-- JUnit Jupiter [OK]
| '-- I'm a Test Class [OK]
|   +-- Test with spaces, expected ok [OK]
|   '-- Test with spaces, expected failed [OK] 
```

## 2.显示名称生成器

2.1 我们还可以创建一个自定义显示名称生成器，并通过`@DisplayNameGeneration`进行配置。

2.2 这个例子使用 JUnit `ReplaceUnderscores`生成器将下划线替换为空格。

DisplayNameGenerator1Test.java

```java
 package com.mkyong.display;

import org.junit.jupiter.api.DisplayNameGeneration;
import org.junit.jupiter.api.DisplayNameGenerator;
import org.junit.jupiter.api.Test;

import java.lang.reflect.Method;

@DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class)
public class DisplayNameGenerator1Test {

    @Test
    void test_spaces_ok() {
    }

    @Test
    void test_spaces_fail() {
    }

} 
```

输出

![output](img/a64e6614b4c65e99ec02d9f8d5235689.png)

2.3 我们可以扩展 JUnit `DisplayNameGenerator`来创建我们的自定义显示名称生成器。

DisplayNameGenerator2Test.java

```java
 package com.mkyong.display;

import org.junit.jupiter.api.DisplayNameGeneration;
import org.junit.jupiter.api.DisplayNameGenerator;
import org.junit.jupiter.api.Test;

import java.lang.reflect.Method;
import java.util.Arrays;
import java.util.stream.Collectors;

@DisplayNameGeneration(DisplayNameGenerator2Test.CustomDisplayNameGenerator.class)
public class DisplayNameGenerator2Test {

    @Test
    void test_spaces_ok() {
    }

    @Test
    void test_spaces_fail() {
    }

    static class CustomDisplayNameGenerator extends DisplayNameGenerator.Standard {

        @Override
        public String generateDisplayNameForClass(Class<?> testClass) {
            return "New Name for test class";
        }

        @Override
        public String generateDisplayNameForNestedClass(Class<?> nestedClass) {
            return super.generateDisplayNameForNestedClass(nestedClass);
        }

        @Override
        public String generateDisplayNameForMethod(Class<?> testClass, Method testMethod) {
            String name = testMethod.getName();
            return Arrays.stream(name.split("_")).collect(Collectors.joining(" | "));
        }
    }

} 
```

输出

![output](img/0aba5db6f7b28377c1b54d744d97a598.png)

## 3.参数化测试

3.1 对于参数化测试，我们可以通过`@ParameterizedTest`的 name 属性来声明自定义显示名称，参见下面的例子:

DisplayNameParamTest.java

```java
 package com.mkyong.display;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.EnumSource;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.concurrent.TimeUnit;
import java.util.stream.Stream;

import static org.junit.jupiter.params.provider.Arguments.arguments;

public class DisplayNameParamTest {

    @ParameterizedTest(name = "#{index} - Test with TimeUnit: {0}")
    @EnumSource(value = TimeUnit.class, names = {"MINUTES", "SECONDS"})
    void test_timeunit_ok(TimeUnit time) {
    }

    @ParameterizedTest(name = "#{index} - Test with {0} and {1}")
    @MethodSource("argumentProvider")
    void test_method_multi(String str, int length) {
    }

    static Stream<Arguments> argumentProvider() {
        return Stream.of(
                arguments("abc", 3),
                arguments("lemon", 2)
        );
    }

} 
```

输出

![output](img/fac0907fba7d1601a0a1c1a13af7eccb.png)

## 下载源代码

$ git clone [https://github.com/mkyong/junit-examples](http://web.archive.org/web/20221225035521/https://github.com/mkyong/junit-examples)
$ cd junit5-examples
$ check src/test/java/com/mkyong/display/*.java

# 参考

*   [JUnit 5 显示名称](http://web.archive.org/web/20221225035521/https://junit.org/junit5/docs/current/user-guide/#writing-tests-display-names)
*   [JUnit 5 参数化测试](/web/20221225035521/https://mkyong.com/junit5/junit-5-parameterized-tests/)

<input type="hidden" id="mkyong-current-postId" value="15266">