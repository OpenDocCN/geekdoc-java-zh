# Java regex 验证用户名示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/regular-expressions/how-to-validate-username-with-regular-expression/>

![Java regex username](img/7f752235c8bed35626582cf36055bc47.png)

本文展示了如何使用 regex 在 Java 中验证用户名。

用户名要求

1.  用户名由字母数字字符(a-zA-Z0-9)、小写字母或大写字母组成。
2.  点()允许的用户名。)、下划线(_)和连字符(-)。
3.  圆点(。)、下划线(_)或连字符(-)不能是第一个或最后一个字符。
4.  圆点(。)、下划线(_)或连字符(-)不会连续出现，例如 java..正则表达式
5.  字符数必须在 5 到 20 之间。

下面是符合上述所有要求的正则表达式。

```java
 ^[a-zA-Z0-9]([._-](?![._-])|[a-zA-Z0-9]){3,18}[a-zA-Z0-9]$ 
```

## 1.用户名正则表达式解释

下面是用户名正则表达式的解释。

```java
 ^[a-zA-Z0-9]      # start with an alphanumeric character
  (                 # start of (group 1)
    [._-](?![._-])  # follow by a dot, hyphen, or underscore, negative lookahead to
                    # ensures dot, hyphen, and underscore does not appear consecutively
    |               # or
    [a-zA-Z0-9]     # an alphanumeric character
  )                 # end of (group 1)
  {3,18}            # ensures the length of (group 1) between 3 and 18
  [a-zA-Z0-9]$      # end with an alphanumeric character

                    # {3,18} plus the first and last alphanumeric characters,
                    # total length became {5,20} 
```

**是什么？！**
正则符号`?!`是一个[负前瞻](http://web.archive.org/web/20220424145629/https://www.regular-expressions.info/lookaround.html)，确保某个东西后面没有其他东西；例如，`_(?!_)`确保下划线后面没有下划线。

## 2.验证用户名的正则表达式

下面是一个使用上述正则表达式验证用户名的 Java 示例。

UsernameValidator.java

```java
 package com.mkyong.regex.username;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class UsernameValidator {

    // simple regex
    //private static final String USERNAME_PATTERN = "^[a-z0-9\\._-]{5,20}$";

    // strict regex
    private static final String USERNAME_PATTERN =
            "^[a-zA-Z0-9]([._-](?![._-])|[a-zA-Z0-9]){3,18}[a-zA-Z0-9]$";

    private static final Pattern pattern = Pattern.compile(USERNAME_PATTERN);

    public static boolean isValid(final String username) {
        Matcher matcher = pattern.matcher(username);
        return matcher.matches();
    }

} 
```

## 3.用户名正则表达式单元测试

下面是验证有效和无效用户名列表的单元测试。

UsernameValidatorTest.java

```java
 package com.mkyong.regex.username;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class UsernameValidatorTest {

    @ParameterizedTest(name = "#{index} - Run test with username = {0}")
    @MethodSource("validUsernameProvider")
    void test_username_regex_valid(String username) {
        assertTrue(UsernameValidator.isValid(username));
    }

    @ParameterizedTest(name = "#{index} - Run test with username = {0}")
    @MethodSource("invalidUsernameProvider")
    void test_username_regex_invalid(String username) {
        assertFalse(UsernameValidator.isValid(username));
    }

    static Stream<String> validUsernameProvider() {
        return Stream.of(
                "mkyong",
                "javaregex",
                "JAVAregex",
                "java.regex",
                "java-regex",
                "java_regex",
                "java.regex.123",
                "java-regex-123",
                "java_regex_123",
                "javaregex123",
                "123456",
                "java123",
                "01234567890123456789");
    }

    static Stream<String> invalidUsernameProvider() {
        return Stream.of(
                "abc",                      // invalid length 3, length must between 5-20
                "01234567890123456789a",    // invalid length 21, length must between 5-20
                "_javaregex_",              // invalid start and last character
                ".javaregex.",              // invalid start and last character
                "-javaregex-",              // invalid start and last character
                "javaregex#$%@123",         // invalid symbols, support dot, hyphen and underscore
                "java..regex",              // dot cant appear consecutively
                "java--regex",              // hyphen can't appear consecutively
                "java__regex",              // underscore can't appear consecutively
                "java._regex",              // dot and underscore can't appear consecutively
                "java.-regex",              // dot and hyphen can't appear consecutively
                " ",                        // empty
                "");                        // empty
    }

} 
```

都通过了。

![unit tests passed](img/cad6ebe483fb5d3aa9a6f52cffd5247f.png)![unit tests passed](img/240e9be4a52734f910e39c83c176ac8f.png)

## 4.反正则表达式，Java 用户名验证器

下面是反正则表达式开发人员验证用户名的等效 Java 代码(满足上述用户名的所有要求)。

UsernameValidatorCode.java

```java
 package com.mkyong.regex.username;

public class UsernameValidatorCode {

    private static final char[] SUPPORT_SYMBOLS_CHAR = {'.', '_', '-'};

    public static boolean isValid(final String username) {

        // check empty
        if (username == null || username.length() == 0) {
            return false;
        }

        // check length
        if (username.length() < 5 || username.length() > 20) {
            return false;
        }

        return isValidUsername(username.toCharArray());
    }

    private static boolean isValidUsername(final char[] username) {

        int currentPosition = 0;
        boolean valid = true;

        // check char by char
        for (char c : username) {

            // if alphanumeric char, no need check, process next
            if (!Character.isLetterOrDigit(c)) {

                // for non-alphanumeric char, also not a supported symbol, break
                if (!isSupportedSymbols(c)) {
                    valid = false;
                    break;
                }

                // ensures first and last char not a supported symbol
                if (username[0] == c || username[username.length - 1] == c) {
                    valid = false;
                    break;
                }

                // ensure supported symbol does not appear consecutively
                // is next position also a supported symbol?
                if (isSupportedSymbols(username[currentPosition + 1])) {
                    valid = false;
                    break;
                }

            }

            currentPosition++;
        }

        return valid;

    }

    private static boolean isSupportedSymbols(final char symbol) {
        for (char temp : SUPPORT_SYMBOLS_CHAR) {
            if (temp == symbol) {
                return true;
            }
        }
        return false;
    }

} 
```

同样的#3 单元再次测试，用上面的 Java 用户名验证器替换 regex 验证器，所有测试都通过了。

**延伸阅读**
如果你使用 email 作为用户名，试试这个 [Java email regex 例子](/web/20220424145629/https://mkyong.com/regular-expressions/how-to-validate-email-address-with-regular-expression/)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220424145629/https://github.com/mkyong/core-java)

$ CD Java-regex/用户名

## 参考

*   [维基百科–字母数字](http://web.archive.org/web/20220424145629/https://en.wikipedia.org/wiki/Alphanumeric)
*   [Regex–前视和后视零长度断言](http://web.archive.org/web/20220424145629/https://www.regular-expressions.info/lookaround.html)
*   [Java 电子邮件正则表达式示例](/web/20220424145629/https://mkyong.com/regular-expressions/how-to-validate-email-address-with-regular-expression/)
*   [密码&用户名最佳实践](http://web.archive.org/web/20220424145629/https://security.intuit.com/index.php/protect-your-information/password-username-best-practices)
*   [JUnit 5 参数化测试](/web/20220424145629/https://mkyong.com/junit5/junit-5-parameterized-tests/)

<input type="hidden" id="mkyong-current-postId" value="1897">