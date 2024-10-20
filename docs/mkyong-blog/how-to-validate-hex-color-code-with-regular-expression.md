# Java 正则表达式十六进制颜色代码

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/regular-expressions/how-to-validate-hex-color-code-with-regular-expression/>

这篇文章展示了如何使用正则表达式来验证一个十六进制的颜色代码。

十六进制颜色代码的正则表达式。

```java
 ^#([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$ 
```

正则表达式解释

```java
 ^                 # start of the line
  #                 # start with a number sign `#`
  (                 # start of (group 1)
    [a-fA-F0-9]{6}  # support z-f, A-F and 0-9, with a length of 6
    |               # or
    [a-fA-F0-9]{3}  # support z-f, A-F and 0-9, with a length of 3
  )                 # end of (group 1)
  $                 # end of the line 
```

## 1.十六进制颜色代码格式。

在 web 中，我们可以使用 RGB 三元组或十六进制三元组(十六进制颜色代码)来表示网页上的颜色。对于十六进制颜色代码，有两种格式，标准十六进制三联体和简写十六进制格式，两种格式都以前导数字符号`#`开始。

**1.1 十六进制三联体格式**
十六进制三联体或标准十六进制色码使用六位(十六进制)数字形式来表示颜色。例如:

```java
 #000000
  #FFFFFF
  #808080
  #8A2BE2 
```

对于`#8A2BE2`，第一个十六进制`8A`代表红色值，第二个`2B`代表绿色值，第三个`E2`代表蓝色值。

**1.2 速记十六进制格式**
速记十六进制使用三位(十六进制)数字形式来表示一种颜色。这个想法仅仅是将每个数字加倍，成为上面的十六进制三联体格式。例如:

```java
 #000
  #FFF
  #09C 
```

对于`#000`，会变成`#000000`。对于`#09C`，就变成了`#0099CC`。

阅读下面的 CSS 语法；两者代表相同的颜色。

```java
 p   { color: #000;    }   /* black color, hex triplet format */
  h1  { color: #000000; }   /* same color as above, shorthand hex format */ 
```

**网页颜色**
更多信息，请阅读此[维基百科-网页颜色](http://web.archive.org/web/20220808014933/https://en.wikipedia.org/wiki/Web_colors)。

## 2.验证十六进制颜色代码的 Java 正则表达式

下面是一个 Java regex 示例，用于验证标准十六进制三元组和简写格式的十六进制颜色代码。

HexValidatorWebColor.java

```java
 package com.mkyong.regex.hex;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class HexValidatorWebColor {

    private static final String HEX_WEBCOLOR_PATTERN
            = "^#([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$";

    private static final Pattern pattern = Pattern.compile(HEX_WEBCOLOR_PATTERN);

    public static boolean isValid(final String colorCode) {
        Matcher matcher = pattern.matcher(colorCode);
        return matcher.matches();
    }

} 
```

## 3.测试 regex 十六进制颜色代码。

下面是验证有效和无效十六进制颜色代码列表的单元测试。

HexValidatorWebColorTest.java

```java
 package com.mkyong.regex.hex;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class HexValidatorWebColorTest {

    @ParameterizedTest(name = "#{index} - Run test with web color = {0}")
    @MethodSource("validWebColorProvider")
    void test_color_regex_valid(String color) {
        assertTrue(HexValidatorWebColor.isValid(color));
    }

    @ParameterizedTest(name = "#{index} - Run test with web color = {0}")
    @MethodSource("invalidWebColorProvider")
    void test_color_regex_invalid(String color) {
        assertFalse(HexValidatorWebColor.isValid(color));
    }

    static Stream<String> validWebColorProvider() {
        return Stream.of(
                "#000000",
                "#999999",
                "#1a1a1a",
                "#1A1A1A",
                "#0f0f0f",
                "#0F0F0F",
                "#bcbcbc",
                "#BcbCbC",
                "#000",
                "#FFF",
                "#abc",
                "#def");
    }

    static Stream<String> invalidWebColorProvider() {
        return Stream.of(
                "123456",                   // must start with a #
                "#afafah",                  // support a-f only, h is not allowed
                "#123abce",                 // invalid length, must length of 3 or 6
                "#1234",                    // invalid length, must length of 3 or 6
                "#-123",                    // support 0-9 only
                " ",                        // space
                "");                        // empty
    }

} 
```

## 4.从字符串中查找十六进制颜色代码

下面是一个使用正则表达式从字符串中查找或提取十六进制颜色代码的例子。

FindHexColorCode.java

```java
 package com.mkyong.regex.hex;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class FindHexColorCode {

    public static void main(String[] args) {

        String str = "p { color: #000; }";

        Pattern pattern = Pattern.compile(
                "^(.*?)(#([a-fA-F0-9]{6}|[a-fA-F0-9]{3}))\\b(.*)$");

        Matcher matcher = pattern.matcher(str);
        if (matcher.matches()) {
            System.out.println(matcher.groupCount());     // 4
            System.out.println(matcher.group(0));         // p { color: #000; }
            System.out.println(matcher.group(1));         // p { color:
            System.out.println(matcher.group(2));         // #000
            System.out.println(matcher.group(3));         // 000
            System.out.println(matcher.group(4));         // ; }
        } else {
            System.out.println("no match!");
        }

    }

} 
```

输出

Terminal

```java
 4
p { color: #000; }
p { color:
#000
000
; } 
```

在 regex 中，`\b`元字符意味着[单词边界](http://web.archive.org/web/20220808014933/https://www.regular-expressions.info/wordboundaries.html)，这确保十六进制颜色代码后面没有任何单词。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220808014933/https://github.com/mkyong/core-java)

$ cd java-regex/hex

## 参考

*   [维基百科–网页颜色](http://web.archive.org/web/20220808014933/https://en.wikipedia.org/wiki/Web_colors)
*   [Wikipedia – Hexadecimal](http://web.archive.org/web/20220808014933/https://en.wikipedia.org/wiki/Hexadecimal)
*   [正则词边界](http://web.archive.org/web/20220808014933/https://www.regular-expressions.info/wordboundaries.html)

<input type="hidden" id="mkyong-current-postId" value="1937">