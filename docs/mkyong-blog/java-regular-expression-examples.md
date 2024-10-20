# Java 正则表达式示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-regular-expression-examples/>

Java 8 流和正则表达式示例。

**Note**
Learn the basic regular expression at [Wikipedia](http://web.archive.org/web/20221129224605/https://en.wikipedia.org/wiki/Regular_expression)

## 1.字符串匹配(正则表达式)

1.1 本例中，检查字符串是否为数字。

JavaRegEx1.java

```java
 package com.mkyong.regex;

import java.util.Arrays;
import java.util.List;

public class JavaRegEx1 {

    public static void main(String[] args) {

        List<String> numbers = Arrays.asList("1", "20", "A1", "333", "A2A211");

        for (String number : numbers) {

            if (number.matches("\\d+")) {
                System.out.println(number);		// 1, 20, 333
            }
        }

        // Java 8 stream example
        numbers.stream()
                .filter(x -> x.matches("\\d+"))
                .forEach(System.out::println);

    }
} 
```

输出

```java
 1
20
333

1
20
333 
```

## 2.String.replaceAll(正则表达式，替换)

2.1 本例用`#`替换所有数字

JavaRegEx2.java

```java
 package com.mkyong.regex;

import java.util.Arrays;
import java.util.List;

public class JavaRegEx2 {

    public static void main(String[] args) {

        List<String> numbers = Arrays.asList("1", "20", "A1", "333", "A2A211");

        for (String number : numbers) {
            System.out.println(number.replaceAll("\\d", "#"));
        }

        // Java 8 stream example
        numbers.stream()
                .map(x -> x.replaceAll("\\d", "#"))
                .forEach(System.out::println);

    }
} 
```

输出

```java
 #
##
A#
###
A#A###

#
##
A#
###
A#A### 
```

## 3.模式和匹配器

3.1 从字符串列表中查找所有数字。

JavaRegEx3.java

```java
 package com.mkyong.regex;

import java.util.Arrays;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class JavaRegEx3 {

    public static void main(String[] args) {

        List<String> numbers = Arrays.asList("1", "20", "A1", "333", "A2A211");

        Pattern pattern = Pattern.compile("\\d+");

        for (String number : numbers) {

            Matcher matcher = pattern.matcher(number);
            while (matcher.find()) {
                System.out.println(matcher.group(0));
            }

        }

	}
} 
```

输出

```java
 1
20
1
333
2
211 
```

3.2 对于 Java 8 流，首先我们尝试这样转换:

```java
 numbers.stream()
		.map(x -> pattern.matcher(x))
		.filter(Matcher::find)          // A2A211, will it loop?
		.map(x -> x.group())
		.forEach(x -> System.out.println(x)); 
```

输出，最后一个 211 不见了？

```java
 1
20
1
333
2 
```

该流不能循环使用`.filter`来获取所有的组，我们需要一个自定义的`Spliterators`hack

JavaRegEx4.java

```java
 package com.mkyong.regex;

import java.util.Arrays;
import java.util.List;
import java.util.Spliterators;
import java.util.function.Consumer;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
import java.util.stream.StreamSupport;

public class JavaRegEx4 {

    public static void main(String[] args) {

        List<String> numbers = Arrays.asList("1", "20", "A1", "333", "A2A211");

        Pattern pattern = Pattern.compile("\\d+");

        numbers.stream()
                .flatMap(x ->
                        StreamSupport.stream(new MatchItr(pattern.matcher(x)), false))
                .forEach(x -> System.out.println(x));

    }

    final static class MatchItr extends Spliterators.AbstractSpliterator<String> {
        private final Matcher matcher;

        MatchItr(Matcher m) {
            super(m.regionEnd() - m.regionStart(), ORDERED | NONNULL);
            matcher = m;
        }

        public boolean tryAdvance(Consumer<? super String> action) {
            if (!matcher.find()) return false;
            action.accept(matcher.group());
            return true;
        }
    }

} 
```

输出

```java
 1
20
1
333
2
211 
```

## 4.Java 9，Scanner.findAll(regex)

4.1 Java 9，我们可以使用`Scanner.findAll(regex)`返回一个匹配结果流，匹配提供的正则表达式。

```java
 Scanner scan = new Scanner("A2A211");
	List<String> collect = scan
			.findAll("\\d+")
			.map(m -> m.group())
			.collect(Collectors.toList());
	collect.forEach(x -> System.out.println(x)); 
```

输出

```java
 2
211 
```

最终版本，用 Java 9。

JavaRegEx5.java

```java
 package com.mkyong.regex;

import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.regex.Pattern;
import java.util.stream.Collectors;

public class JavaRegEx5 {

    public static void main(String[] args) {

        List<String> numbers = Arrays.asList("1", "20", "A1", "333", "A2A211");

        Pattern pattern = Pattern.compile("\\d+");

        List<String> collect = numbers.stream()
                .map(x -> new Scanner(x).findAll(pattern)
                        .map(m -> m.group())
                        .collect(Collectors.toList())
                )
                .flatMap(List::stream)
                .collect(Collectors.toList());

        collect.forEach(x -> System.out.println(x));

    }

} 
```

输出

```java
 1
20
1
333
2
211 
```

## 参考

*   [Scanner.findAll JavaDocs](http://web.archive.org/web/20221129224605/https://docs.oracle.com/javase/9/docs/api/java/util/Scanner.html#findAll-java.lang.String-)
*   [维基百科–正则表达式](http://web.archive.org/web/20221129224605/https://en.wikipedia.org/wiki/Regular_expression)

<input type="hidden" id="mkyong-current-postId" value="15145">