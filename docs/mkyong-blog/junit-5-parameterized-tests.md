# JUnit 5 参数化测试

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/junit5/junit-5-parameterized-tests/>

![junit 5 param test](img/b7a34705a5a0ca2be46f97355957415a.png)

本文向您展示了如何使用不同的参数多次运行一个测试，即所谓的“参数化测试”，让我们来看看以下为测试提供参数的方法:

*   `@ValueSource`
*   `@EnumSource`
*   `@MethodSource`
*   `@CsvSource`
*   `@CsvFileSource`
*   `@ArgumentsSource`

我们需要`junit-jupiter-params`来支持参数化测试。

pom.xml

```java
 <dependency>
		<groupId>org.junit.jupiter</groupId>
		<artifactId>junit-jupiter-engine</artifactId>
		<version>5.5.2</version>
		<scope>test</scope>
	</dependency>

	<!-- Parameterized Tests -->
	<dependency>
		<groupId>org.junit.jupiter</groupId>
		<artifactId>junit-jupiter-params</artifactId>
		<version>5.5.2</version>
		<scope>test</scope>
	</dependency> 
```

*用 JUnit 5.5.2 测试的 PS*

## 1.@ValueSource

1.1 为单参数测试。

ValueSourceTest.java

```java
 package com.mkyong.params;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

import static org.junit.jupiter.api.Assertions.assertTrue;

public class ValueSourceTest {

    // This test will run 3 times with different arguments
    @ParameterizedTest
    @ValueSource(ints = {1, 2, 3})
    void test_int_arrays(int arg) {
        assertTrue(arg > 0);
    }

    @ParameterizedTest(name = "#{index} - Run test with args={0}")
    @ValueSource(ints = {1, 2, 3})
    void test_int_arrays_custom_name(int arg) {
        assertTrue(arg > 0);
    }

    @ParameterizedTest(name = "#{index} - Run test with args={0}")
    @ValueSource(strings = {"apple", "banana", "orange"})
    void test_string_arrays_custom_name(String arg) {
        assertTrue(arg.length() > 1);
    }

} 
```

输出

![output](img/93bd41ba51113fa8fd7d3a0a96d960de.png)

1.2 我们可以通过`@EmptySource`、`@NullSource`或`@NullAndEmptySource`将空值或 null 值传递到测试中(从 JUnit 5.4 开始)。让我们看下面的例子来测试一个`isEmpty()`方法。

ValueSourceEmptyTest.java

```java
 package com.mkyong.params;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.EmptySource;
import org.junit.jupiter.params.provider.NullSource;
import org.junit.jupiter.params.provider.ValueSource;

import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class ValueSourceEmptyTest {

    boolean isEmpty(String input) {
        return (input == null || input.length() == 0);
    }

	// run 3 times, 1 for empty, 1 for null, 1 for ""
    @ParameterizedTest(name = "#{index} - isEmpty()? {0}")
    @EmptySource
    @NullSource
    //@NullAndEmptySource
    @ValueSource(strings = {""})
    void test_is_empty_true(String arg) {
        assertTrue(isEmpty(arg));
    }

    @ParameterizedTest(name = "#{index} - isEmpty()? {0}")
    @ValueSource(strings = {" ", "\n", "a", "\t"})
    void test_is_empty_false(String arg) {
        assertFalse(isEmpty(arg));
    }

} 
```

输出

![output](img/a32c186a6990fbbddba635d17acdc448.png)

## 2.@EnumSource

2.1 运行以`Enum`为参数的测试。

EnumSourceTest.java

```java
 package com.mkyong.params;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.EnumSource;

import java.util.EnumSet;

import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertTrue;
import static org.junit.jupiter.params.provider.EnumSource.Mode.EXCLUDE;

public class EnumSourceTest {

    enum Size {
        XXS, XS, S, M, L, XL, XXL, XXXL;
    }

    @ParameterizedTest
    @EnumSource(Size.class)
    void test_enum(Size size) {
        assertNotNull(size);
    }

    @ParameterizedTest(name = "#{index} - Is size contains {0}?")
    @EnumSource(value = Size.class, names = {"L", "XL", "XXL", "XXXL"})
    void test_enum_include(Size size) {
        assertTrue(EnumSet.allOf(Size.class).contains(size));
    }

    // Size = M, L, XL, XXL, XXXL
    @ParameterizedTest
    @EnumSource(value = Size.class, mode = EXCLUDE, names = {"XXS", "XS", "S"})
    void test_enum_exclude(Size size) {
        EnumSet<Size> excludeSmallSize = EnumSet.range(Size.M, Size.XXXL);
        assertTrue(excludeSmallSize.contains(size));
    }

} 
```

输出。

```java
 $ java -jar junit-platform-console-standalone-1.5.2.jar 
	-cp "target/test-classes/" 
	--select-class com.mkyong.params.EnumSourceTest 
	--disable-ansi-colors

+-- JUnit Jupiter [OK]
| '-- EnumSourceTest [OK]
|   +-- test_enum_include(Size) [OK]
|   | +-- #1 - Is size contains L? [OK]
|   | +-- #2 - Is size contains XL? [OK]
|   | +-- #3 - Is size contains XXL? [OK]
|   | '-- #4 - Is size contains XXXL? [OK]
|   +-- test_enum(Size) [OK]
|   | +-- [1] XXS [OK]
|   | +-- [2] XS [OK]
|   | +-- [3] S [OK]
|   | +-- [4] M [OK]
|   | +-- [5] L [OK]
|   | +-- [6] XL [OK]
|   | +-- [7] XXL [OK]
|   | '-- [8] XXXL [OK]
|   '-- test_enum_exclude(Size) [OK]
|     +-- [1] M [OK]
|     +-- [2] L [OK]
|     +-- [3] XL [OK]
|     +-- [4] XXL [OK]
|     '-- [5] XXXL [OK]
'-- JUnit Vintage [OK] 
```

## 3.@MethodSource

3.1 运行使用静态`method`生成参数的测试。

MethodSourceTest.java

```java
 package com.mkyong.params;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class MethodSourceTest {

    @ParameterizedTest(name = "#{index} - Test with String : {0}")
    @MethodSource("stringProvider")
    void test_method_string(String arg) {
        assertNotNull(arg);
    }

    // this need static
    static Stream<String> stringProvider() {
        return Stream.of("java", "rust");
    }

    @ParameterizedTest(name = "#{index} - Test with Int : {0}")
    @MethodSource("rangeProvider")
    void test_method_int(int arg) {
        assertTrue(arg < 10);
    }

    // this need static
    static IntStream rangeProvider() {
        return IntStream.range(0, 10);
    }

} 
```

输出

```java
 +-- JUnit Jupiter [OK]
| '-- MethodSourceTest [OK]
|   +-- test_method_int(int) [OK]
|   | +-- #1 - Test with Int : 0 [OK]
|   | +-- #2 - Test with Int : 1 [OK]
|   | +-- #3 - Test with Int : 2 [OK]
|   | +-- #4 - Test with Int : 3 [OK]
|   | +-- #5 - Test with Int : 4 [OK]
|   | +-- #6 - Test with Int : 5 [OK]
|   | +-- #7 - Test with Int : 6 [OK]
|   | +-- #8 - Test with Int : 7 [OK]
|   | +-- #9 - Test with Int : 8 [OK]
|   | '-- #10 - Test with Int : 9 [OK]
|   '-- test_method_string(String) [OK]
|     +-- #1 - Test with String : java [OK]
|     '-- #2 - Test with String : rust [OK]
'-- JUnit Vintage [OK] 
```

3.2 多重论证

MethodSourceMultiTest.java

```java
 package com.mkyong.params;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertTrue;
import static org.junit.jupiter.params.provider.Arguments.arguments;

public class MethodSourceMultiTest {

    @ParameterizedTest
    @MethodSource("stringIntAndListProvider")
    void testWithMultiArgMethodSource(String str, int length, List<String> list) {
        assertTrue(str.length() > 0);
        assertEquals(length, list.size());
    }

    static Stream<Arguments> stringIntAndListProvider() {
        return Stream.of(
                arguments("abc", 3, Arrays.asList("a", "b", "c")),
                arguments("lemon", 2, Arrays.asList("x", "y"))
        );
    }

} 
```

输出

```java
 +-- JUnit Jupiter [OK]
| '-- MethodSourceMultiTest [OK]
|   '-- test_method_multi(String, int, List) [OK]
|     +-- [1] abc, 3, [a, b, c] [OK]
|     '-- [2] lemon, 2, [x, y] [OK]
'-- JUnit Vintage [OK] 
```

## 4.@CsvSource

4.1 运行以逗号分隔值(csv)作为参数的测试。

CsvSourceTest.java

```java
 package com.mkyong.params;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class CsvSourceTest {

    @ParameterizedTest
    @CsvSource({
            "java,      4",
            "clojure,   7",
            "python,    6"
    })
    void test_csv(String str, int length) {
        assertEquals(length, str.length());
    }

} 
```

## 5.@CsvFileSource

5.1 从文件中导入逗号分隔值(csv)作为参数。

src/test/resources/simple.csv

```java
 java,      4
clojure,   7
python,    6 
```

CsvFileSourceTest.java

```java
 package com.mkyong.params;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvFileSource;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class CsvFileSourceTest {

	// Skip the first line
    @ParameterizedTest
    @CsvFileSource(resources = "/simple.csv", numLinesToSkip = 1)
    void test_csv_file(String str, int length) {
        assertEquals(length, str.length());
    }

} 
```

输出

```java
 +-- JUnit Jupiter [OK]
| '-- CsvFileSourceTest [OK]
|   '-- test_csv_file(String, int) [OK]
|     +-- [1] clojure, 7 [OK]
|     '-- [2] python, 6 [OK]
'-- JUnit Vintage [OK] 
```

## 6.@ArgumentsSource

6.1 创建可重用的参数提供程序。

CustomArgumentsProvider.java

```java
 package com.mkyong.params;

import org.junit.jupiter.api.extension.ExtensionContext;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.ArgumentsProvider;

import java.util.stream.Stream;

public class CustomArgumentsProvider implements ArgumentsProvider {

    @Override
    public Stream<? extends Arguments> 
		provideArguments(ExtensionContext extensionContext) throws Exception {
        return Stream.of("java", "rust", "kotlin").map(Arguments::of);
    }
} 
```

ArgumentsSourceTest.java

```java
 package com.mkyong.params;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ArgumentsSource;

import static org.junit.jupiter.api.Assertions.assertNotNull;

public class ArgumentsSourceTest {

    @ParameterizedTest
    @ArgumentsSource(CustomArgumentsProvider.class)
    void test_argument_custom(String arg) {
        assertNotNull(arg);
    }

} 
```

## 下载源代码

$ git clone [https://github.com/mkyong/junit-examples](http://web.archive.org/web/20221206155213/https://github.com/mkyong/junit-examples)
$ cd junit5-examples
$ check src/test/java/com/mkyong/params/*.java

## 参考

*   [JUnit 5 参数化测试](http://web.archive.org/web/20221206155213/https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests)
*   [Maven 标准目录布局](http://web.archive.org/web/20221206155213/https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)

<input type="hidden" id="mkyong-current-postId" value="15259">