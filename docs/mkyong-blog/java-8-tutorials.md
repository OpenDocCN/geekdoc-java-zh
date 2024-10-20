# Java 8 教程

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/tutorials/java-8-tutorials/>

![java 8 logo](img/1b1f9e703fcbad927f26aede4227afea.png)

一系列 Java 8 的提示和例子，希望你喜欢。

### 常见问题

一些常见问题。

*   [Java 8 forEach 示例](/web/20221230032430/https://mkyong.com/java8/java-8-foreach-examples/)
*   [Java 8 将列表转换为映射](/web/20221230032430/https://mkyong.com/java8/java-8-convert-list-to-map/)
*   [Java 8 Lambda:比较器示例](/web/20221230032430/https://mkyong.com/java8/java-8-lambda-comparator-example/)
*   [Java 8 方法引用，双冒号(::)操作符](/web/20221230032430/https://mkyong.com/java8/java-8-method-references-double-colon-operator/)
*   [Java 8 流过滤器示例](java8/java-8-streams-filter-examples/)
*   [Java 8 Streams map()示例](/web/20221230032430/https://mkyong.com/java8/java-8-streams-map-examples/)

## 1.功能接口

Java 8 引入了`@FunctionalInterface`，一个只有一个抽象方法的接口。编译器会将任何满足函数接口的[定义的接口视为函数接口；这意味着`@FunctionalInterface`注释是可选的。](http://web.archive.org/web/20221230032430/https://docs.oracle.com/javase/8/docs/api/java/lang/FunctionalInterface.html)

让我们看看六个基本的功能界面。

| 连接 | 签名 | 例子 |
| :-- | :-- | :-- |
| `UnaryOperator<T>` | `T apply(T t)` | `String::toLowerCase`，`Math::tan` |
| `BinaryOperator<T>` | `T apply(T t1, T t2)` | `BigInteger::add`，`Math::pow` |
| `Function<T, R>` | `R apply(T t)` | `Arrays::asList`，`Integer::toBinaryString` |
| `Predicate<T, U>` | `boolean test(T t, U u)` | `String::isEmpty`，`Character::isDigit` |
| `Supplier<T>` | `T get()` | `LocalDate::now`，`Instant::now` |
| `Consumer<T>` | `void accept(T t)` | `System.out::println`，`Error::printStackTrace` |

*   [Java 8 函数示例](/web/20221230032430/https://mkyong.com/java8/java-8-function-examples/)
*   [Java 8 双功能示例](/web/20221230032430/https://mkyong.com/java8/java-8-bifunction-examples/)
*   [Java 8 二元运算符示例](/web/20221230032430/https://mkyong.com/java8/java-8-binaryoperator-examples/)
*   [Java 8 一元运算符示例](/web/20221230032430/https://mkyong.com/java8/java-8-unaryoperator-examples/)
*   [Java 8 谓词示例](/web/20221230032430/https://mkyong.com/java8/java-8-predicate-examples/)
*   [Java 8 双预测示例](/web/20221230032430/https://mkyong.com/java8/java-8-bipredicate-examples/)
*   [Java 8 消费者示例](/web/20221230032430/https://mkyong.com/java8/java-8-consumer-examples/)
*   [Java 8 双消费示例](/web/20221230032430/https://mkyong.com/java8/java-8-biconsumer-examples/)
*   [Java 8 供应商示例](/web/20221230032430/https://mkyong.com/java8/java-8-supplier-examples/)

## 2.Lambda 表达式和方法引用

Java 8 引入了 lambda 表达式来实现函数接口的抽象方法。

**延伸阅读> > >** [Java 8 Lambda:比较器示例](/web/20221230032430/https://mkyong.com/java8/java-8-lambda-comparator-example/)

回顾一下 JDK 的`Iterable`类，它有一个`default`方法`forEach()`来接受一个函数接口`Consumer`

Iterable.java

```java
 public interface Iterable<T> {

    default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }

   //...
} 
```

首先，我们可以提供一个匿名类作为`forEach`实现。

```java
 List<String> list = Arrays.asList("node", "java", "python", "ruby");
list.forEach(new Consumer<String>() {       // anonymous class
    @Override
    public void accept(String str) {
        System.out.println(str);
    }
}); 
```

或者，我们可以使用 lambda 表达式来缩短代码，如下所示:

```java
 List<String> list = Arrays.asList("node", "java", "python", "ruby");
list.forEach(str -> System.out.println(str)); // lambda expressions 
```

为了获得更好的可读性，我们可以用方法引用替换 lambda 表达式。

```java
 List<String> list = Arrays.asList("node", "java", "python", "ruby");
list.forEach(System.out::println);           // method references 
```

**延伸阅读> > >** [Java 8 方法引用，双冒号(::)运算符](/web/20221230032430/https://mkyong.com/java8/java-8-method-references-double-colon-operator/)

**注意**
lambda 表达式或方法引用什么都不做，只是对现有方法的另一种方式调用。通过方法引用，它获得了更好的可读性。

### 3.流

*   [Java 8 流过滤器示例](java8/java-8-streams-filter-examples/)
*   [Java 8 Streams map()示例](/web/20221230032430/https://mkyong.com/java8/java-8-streams-map-examples/)
*   [Java 8 平面图示例](/web/20221230032430/https://mkyong.com/java8/java-8-flatmap-example/)
*   [Java 8 并行流示例](/web/20221230032430/https://mkyong.com/java8/java-8-parallel-streams-examples/)
*   [Java 8 Stream.iterate 示例](/web/20221230032430/https://mkyong.com/java8/java-8-stream-iterate-examples/)
*   [Java 8 流收集器按示例分组](/web/20221230032430/https://mkyong.com/java8/java-8-collectors-groupingby-and-mapping-example/)
*   [Java 8 从流中过滤空值](/web/20221230032430/https://mkyong.com/java8/java-8-filter-a-null-value-from-a-stream/)
*   [Java 8 将流转换为列表](/web/20221230032430/https://mkyong.com/java8/java-8-convert-a-stream-to-list/)
*   [Java 8 流 findFirst()和 findAny()](/web/20221230032430/https://mkyong.com/java8/java-8-stream-findfirst-and-findany/)
*   [Java 8 Stream.reduce()示例](/web/20221230032430/https://mkyong.com/java8/java-8-stream-reduce-examples/)
*   [Java 8 将流转换为列表](/web/20221230032430/https://mkyong.com/java8/java-8-convert-a-stream-to-list/)
*   [Java 8 如何用 Stream 对 BigDecimal 求和？](/web/20221230032430/https://mkyong.com/java8/java-8-how-to-sum-bigdecimal-using-stream/)
*   [Java 8 流–逐行读取文件](/web/20221230032430/https://mkyong.com/java8/java-8-stream-read-a-file-line-by-line/)
*   [Java 8 Stream-Convert List<List<String>to List<String>](/web/20221230032430/https://mkyong.com/java/java-8-stream-convert-listliststring-to-liststring/)
*   [Java 8 Stream–peek()不支持 count()？](/web/20221230032430/https://mkyong.com/java8/java-8-stream-the-peek-is-not-working-with-count/)
*   [Java 8 使用后应该关闭流吗？](/web/20221230032430/https://mkyong.com/java/java-8-should-we-close-the-stream-after-use/)
*   [Java 8 将流转换成数组](/web/20221230032430/https://mkyong.com/java8/java-8-how-to-convert-a-stream-to-array/)
*   [Java 8 如何将 IntStream 转换成整数数组](/web/20221230032430/https://mkyong.com/java8/java-8-how-to-convert-intstream-to-integer/)
*   [Java 8 如何将 IntStream 转换成 int 或 int 数组](/web/20221230032430/https://mkyong.com/java8/java-8-how-to-convert-intstream-to-int-or-int/)
*   [Java 8 如何用 stream.sorted()](/web/20221230032430/https://mkyong.com/java8/java-8-how-to-sort-list-with-stream-sorted/) 对列表进行排序
*   [Java–如何对所有流整数求和](/web/20221230032430/https://mkyong.com/java8/java-how-to-sum-all-the-stream-integers/)
*   [Java–如何将原始数组转换为列表](/web/20221230032430/https://mkyong.com/java/java-how-to-convert-a-primitive-array-to-list/)
*   [Java–如何将数组转换成流](/web/20221230032430/https://mkyong.com/java8/java-how-to-convert-array-to-stream/)
*   [Java–流已经被操作或关闭](/web/20221230032430/https://mkyong.com/java8/java-stream-has-already-been-operated-upon-or-closed/)

### 4.新日期时间 API

过去，我们使用`Date`和`Calendar`API 来表示和操作日期。

*   `java.util.Date`–日期和时间，以默认时区打印。
*   `java.util.Calendar`–日期和时间，更多操作日期的方法。
*   `java.text.SimpleDateFormat`–格式化(日期- >文本)，解析(文本- >日期)日期和日历。

Java 8 在`java.time`包中创建了一系列新的日期和时间 API。( [JSR310](http://web.archive.org/web/20221230032430/https://jcp.org/en/jsr/detail?id=310) 并受 Joda-time 启发)。

*   `java.time.LocalDate`–无时间、无时区的日期。
*   `java.time.LocalTime`–无日期、无时区的时间。
*   `java.time.LocalDateTime`–日期和时间，无时区。
*   `java.time.ZonedDateTime`–日期和时间，带时区。
*   `java.time.DateTimeFormatter`–Java . time 的格式化(日期- >文本)、解析(文本- >日期)
*   `java.time.Instant`–机器的日期和时间，自 Unix 纪元时间(世界协调时 1970 年 1 月 1 日午夜)以来经过的秒数
*   `java.time.Duration`–以秒和纳秒为单位测量时间。
*   `java.time.Period`–以年、月、日计量时间。
*   `java.time.TemporalAdjuster`–调整日期。
*   `java.time.OffsetDateTime`–{更新我}

示例…

*   [Java–如何获取当前日期时间](/web/20221230032430/https://mkyong.com/java/java-how-to-get-current-date-time-date-and-calender/)
*   [Java–如何获取当前时间戳](/web/20221230032430/https://mkyong.com/java/how-to-get-current-timestamps-in-java/)
*   [Java–如何将字符串转换成日期](/web/20221230032430/https://mkyong.com/java/how-to-convert-string-to-date-java/)
*   [Java 8–持续时间和周期示例](/web/20221230032430/https://mkyong.com/java8/java-8-period-and-duration-examples/)
*   [Java 8–如何将字符串转换为本地日期](/web/20221230032430/https://mkyong.com/java8/java-8-how-to-convert-string-to-localdate/)
*   [Java 8–如何格式化本地日期时间](/web/20221230032430/https://mkyong.com/java8/java-8-how-to-format-localdatetime/)
*   [Java 8–将 Instant 转换为 LocalDateTime](/web/20221230032430/https://mkyong.com/java8/java-convert-instant-to-localdatetime/)
*   [Java 8–将 Instant 转换为 ZoneDateTime](/web/20221230032430/https://mkyong.com/java8/java-8-convert-instant-to-zoneddatetime/)
*   [Java 8–将日期转换为本地日期和本地日期时间](/web/20221230032430/https://mkyong.com/java8/java-8-convert-date-to-localdate-and-localdatetime/)
*   [Java 8 – ZonedDateTime examples](/web/20221230032430/https://mkyong.com/java8/java-8-zoneddatetime-examples/)
*   [Java–在时区之间转换日期和时间](/web/20221230032430/https://mkyong.com/java/java-convert-date-and-time-between-timezone/)
*   [Java–如何给当前日期添加天数](/web/20221230032430/https://mkyong.com/java/java-how-to-add-days-to-current-date/)
*   [Java 8–temporal adjusts 示例](/web/20221230032430/https://mkyong.com/java8/java-8-temporaladjusters-examples/)
*   [Java 8–将纪元时间毫秒转换为本地日期或本地日期时间](/web/20221230032430/https://mkyong.com/java8/java-8-convert-epoch-time-milliseconds-to-localdate-or-localdatetime/)
*   [Java 8–两个本地日期或本地日期时间之间的差异](/web/20221230032430/https://mkyong.com/java8/java-8-difference-between-two-localdate-or-localdatetime/)
*   [Java 8–如何计算两个日期之间的天数？](/web/20221230032430/https://mkyong.com/java8/java-8-how-to-calculate-days-between-two-dates/)
*   Java 8–如何解析带有“DD MMM”(02 Jan)的日期，而不带年份？
*   [Java 8–将本地日期和本地日期时间转换为日期](/web/20221230032430/https://mkyong.com/java8/java-8-convert-localdate-and-localdatetime-to-date/)
*   [Java 8–无法从 TemporalAccessor 获取本地日期时间](/web/20221230032430/https://mkyong.com/java8/java-8-unable-to-obtain-localdatetime-from-temporalaccessor/)
*   [Java 8–将 ZonedDateTime 转换为时间戳](/web/20221230032430/https://mkyong.com/java8/java-8-convert-zoneddatetime-to-timestamp/)
*   [Java–显示所有 ZoneId 及其 UTC 偏移量](/web/20221230032430/https://mkyong.com/java8/java-display-all-zoneid-and-its-utc-offset/)
*   [Java 8–将本地日期时间转换为时间戳](/web/20221230032430/https://mkyong.com/java8/java-8-convert-localdatetime-to-timestamp/)
*   [Java–如何改变字符串中的日期格式](/web/20221230032430/https://mkyong.com/java/java-how-to-change-date-format-in-a-string/)
*   [检查日期是否超过 6 个月](/web/20221230032430/https://mkyong.com/java8/java-check-if-the-date-is-older-than-6-months/)
*   [Java–如何比较日期](/web/20221230032430/https://mkyong.com/java/how-to-compare-dates-in-java/)
*   [Java–如何计算运行时间](/web/20221230032430/https://mkyong.com/java/how-do-calculate-elapsed-execute-time-in-java/)
*   [Java 8–MinguoDate 示例(台湾日历)](/web/20221230032430/https://mkyong.com/java8/java-8-minguodate-examples/)
*   [Java 8–回历，如何计算斋月日期(伊斯兰历)](/web/20221230032430/https://mkyong.com/java8/java-8-hijrahdate-how-to-calculate-the-ramadan-date/)
*   [Java 日期时间教程](http://web.archive.org/web/20221230032430/http://www.mkyong.com/tutorials/java-date-time-tutorials/)

### 5.Java 8 技巧

*   [Java 8 可选深度](/web/20221230032430/https://mkyong.com/java8/java-8-optional-in-depth/)
*   [Java 8 如何对地图进行排序](/web/20221230032430/https://mkyong.com/java8/java-8-how-to-sort-a-map/)
*   [Java 8 将列表转换为映射](/web/20221230032430/https://mkyong.com/java8/java-8-convert-list-to-map/)
*   [Java 8 过滤一个地图示例](/web/20221230032430/https://mkyong.com/java8/java-8-filter-a-map-examples/)
*   [Java 8 将地图转换为列表](/web/20221230032430/https://mkyong.com/java8/java-8-convert-map-to-list/)
*   [Java 8 StringJoiner 示例](/web/20221230032430/https://mkyong.com/java8/java-8-stringjoiner-example/)
*   [Java 8 数学精确例题](/web/20221230032430/https://mkyong.com/java/java-check-if-array-contains-a-certain-value/)
*   [Java 8 forEach print 带索引](/web/20221230032430/https://mkyong.com/java8/java-8-foreach-print-with-index/)
*   [Java 8 将可选的<字符串>转换为字符串](/web/20221230032430/https://mkyong.com/java8/java-8-convert-optionalstring-to-string/)
*   [Java–如何打印金字塔](/web/20221230032430/https://mkyong.com/java/java-how-to-print-a-pyramid/)
*   Java–检查数组是否包含某个值？
*   [Java–如何连接数组](/web/20221230032430/https://mkyong.com/java/java-how-to-join-arrays/)
*   [Java–生成一个范围内的随机整数](/web/20221230032430/https://mkyong.com/java/java-generate-random-integers-in-a-range/)
*   Java——如何将一个名字打印 10 次？
*   Java–如何在列表中搜索字符串？
*   [Java–如何从映射中获取键和值](/web/20221230032430/https://mkyong.com/java/java-how-to-get-keys-and-values-from-map/)
*   [Java–将文件转换成字符串](/web/20221230032430/https://mkyong.com/java/java-convert-file-to-string/)
*   [Java–将数组转换成数组列表](/web/20221230032430/https://mkyong.com/java/java-convert-array-to-arraylist/)
*   [Java–如何检查一个字符串是否为数字](/web/20221230032430/https://mkyong.com/java/java-how-to-check-if-a-string-is-numeric/)
*   [Java–如何用逗号连接列表字符串](/web/20221230032430/https://mkyong.com/java/java-how-to-join-list-string-with-commas/)
*   [Java–将逗号分隔的字符串转换成列表](/web/20221230032430/https://mkyong.com/java/java-convert-comma-separated-string-to-a-list/)
*   [Java 质数示例](/web/20221230032430/https://mkyong.com/java/java-prime-numbers-examples/)
*   [如何告诉 Maven 使用 Java 8](/web/20221230032430/https://mkyong.com/maven/how-to-tell-maven-to-use-java-8/)
*   [Java . lang . unsupportedclassversionerror](/web/20221230032430/https://mkyong.com/java/java-lang-unsupportedclassversionerror/)
*   [Java 斐波那契例子](/web/20221230032430/https://mkyong.com/java/java-fibonacci-examples/)
*   [如何在 Java 中循环地图](/web/20221230032430/https://mkyong.com/java/how-to-loop-a-map-in-java/)
*   [Java 正则表达式示例](/web/20221230032430/https://mkyong.com/java/java-regular-expression-examples/)
*   [如何在 Java 中读取文件–buffered reader](/web/20221230032430/https://mkyong.com/java/how-to-read-file-from-java-bufferedreader-example/)

### 装置

*   [如何在 CentOS 上安装 Oracle JDK 8](/web/20221230032430/https://mkyong.com/java/how-to-install-oracle-jdk-8-on-centos/)
*   [如何在 Debian 上安装 Oracle JDK 8](/web/20221230032430/https://mkyong.com/java/how-to-install-oracle-jdk-8-on-debian/)
*   [如何在 Mac OS 上安装 Java](/web/20221230032430/https://mkyong.com/java/how-to-install-java-on-mac-osx/)

### 参考

*   [JDK 最新动态 8](http://web.archive.org/web/20221230032430/https://www.oracle.com/technetwork/java/javase/8-whats-new-2157071.html)
*   为什么我们需要一个新的日期和时间库？
*   [JSR 310:日期和时间 API](http://web.archive.org/web/20221230032430/https://jcp.org/en/jsr/detail?id=310)
*   [流 JavaDoc](http://web.archive.org/web/20221230032430/https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)

<input type="hidden" id="mkyong-current-postId" value="14515">