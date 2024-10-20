# Java——如何检查一个字符串是否是数字

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-check-if-a-string-is-numeric/>

几个 Java 例子向你展示如何检查一个字符串是否是数字。

## 1.Character.isDigit()

将一个字符串转换成一个`char`数组并用`Character.isDigit()`检查它

NumericExample.java

```java
 package com.mkyong;

public class NumericExample {

    public static void main(String[] args) {

        System.out.println(isNumeric(""));          // false
        System.out.println(isNumeric(" "));         // false
        System.out.println(isNumeric(null));        // false
        System.out.println(isNumeric("1,200"));     // false
        System.out.println(isNumeric("1"));         // true
        System.out.println(isNumeric("200"));       // true
        System.out.println(isNumeric("3000.00"));   // false

    }

    public static boolean isNumeric(final String str) {

        // null or empty
        if (str == null || str.length() == 0) {
            return false;
        }

        for (char c : str.toCharArray()) {
            if (!Character.isDigit(c)) {
                return false;
            }
        }

        return true;

    }

} 
```

输出

```java
 false
false
false
false
true
true
false 
```

## 2.Java 8

这个现在简单多了。

```java
 public static boolean isNumeric(final String str) {

        // null or empty
        if (str == null || str.length() == 0) {
            return false;
        }

        return str.chars().allMatch(Character::isDigit);

    } 
```

## 3.Apache Commons Lang

如果 claspath 中存在 Apache Commons Lang，请尝试`NumberUtils.isDigits()`

pom.xml

```java
 <dependency>
		<groupId>org.apache.commons</groupId>
		<artifactId>commons-lang3</artifactId>
		<version>3.9</version>
	</dependency> 
```

```java
 import org.apache.commons.lang3.math.NumberUtils;

	public static boolean isNumeric(final String str) {

        return NumberUtils.isDigits(str);

    } 
```

## 4.NumberFormatException

此解决方案可以解决性能问题，但不推荐使用。

```java
 public static boolean isNumeric(final String str) {

        if (str == null || str.length() == 0) {
            return false;
        }

        try {

            Integer.parseInt(str);
            return true;

        } catch (NumberFormatException e) {
            return false;
        }

    } 
```

## 参考

*   [character . is digit JavaDocs](http://web.archive.org/web/20221205121713/https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isDigit-char-)
*   [NumberUtils.isDigits JavaDocs](http://web.archive.org/web/20221205121713/https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/math/NumberUtils.html#isDigits-java.lang.String-)

<input type="hidden" id="mkyong-current-postId" value="15074">