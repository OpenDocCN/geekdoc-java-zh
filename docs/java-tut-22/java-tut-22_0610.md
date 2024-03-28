# 定制整理规则

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/rule.html`](https://docs.oracle.com/javase/tutorial/i18n/text/rule.html)

前一节讨论了如何使用区域设置的预定义规则来比较字符串。这些整理规则确定字符串的排序顺序。如果预定义的整理规则不符合您的需求，您可以设计自己的规则并将其分配给`RuleBasedCollator`对象。

定制的整理规则包含在传递给`RuleBasedCollator`构造函数的`String`对象中。这里是一个简单的例子：

```java
String simpleRule = "< a < b < c < d";
RuleBasedCollator simpleCollator =  new RuleBasedCollator(simpleRule);

```

对于前面示例中的`simpleCollator`对象，`a`小于`b`，`b`小于`c`，依此类推。当比较字符串时，`simpleCollator.compare`方法引用这些规则。用于构造整理规则的完整语法比这个简单示例更灵活和复杂。有关语法的完整描述，请参考[`RuleBasedCollator`](https://docs.oracle.com/javase/8/docs/api/java/text/RuleBasedCollator.html)类的 API 文档。

接下来的示例使用两个整理器对一组西班牙语单词进行排序。此示例的完整源代码在`RulesDemo.java`中。

`RulesDemo`程序首先定义了英语和西班牙语的整理规则。该程序将按传统方式对西班牙语单词进行排序。按照传统规则排序时，字母 ch 和 ll 及其大写形式各自在排序顺序中有自己的位置。这些字符对比较就好像它们是一个字符一样。例如，ch 按照一个字母排序，在排序顺序中紧随 cz。请注意两个整理器的规则如何不同：

```java
String englishRules = (
    "< a,A < b,B < c,C < d,D < e,E < f,F " +
    "< g,G < h,H < i,I < j,J < k,K < l,L " +
    "< m,M < n,N < o,O < p,P < q,Q < r,R " +
    "< s,S < t,T < u,U < v,V < w,W < x,X " +
    "< y,Y < z,Z");

String smallnTilde = new String("\u00F1");    // ñ
String capitalNTilde = new String("\u00D1");  // Ñ

String traditionalSpanishRules = (
    "< a,A < b,B < c,C " +
    "< ch, cH, Ch, CH " +
    "< d,D < e,E < f,F " +
    "< g,G < h,H < i,I < j,J < k,K < l,L " +
    "< ll, lL, Ll, LL " +
    "< m,M < n,N " +
    "< " + smallnTilde + "," + capitalNTilde + " " +
    "< o,O < p,P < q,Q < r,R " +
    "< s,S < t,T < u,U < v,V < w,W < x,X " +
    "< y,Y < z,Z");

```

以下代码行创建整理器并调用排序例程：

```java
try {
    RuleBasedCollator enCollator = new RuleBasedCollator(englishRules);
    RuleBasedCollator spCollator =
        new RuleBasedCollator(traditionalSpanishRules);

    sortStrings(enCollator, words);
    printStrings(words);
    System.out.println();

    sortStrings(spCollator, words);
    printStrings(words);
} catch (ParseException pe) {
    System.out.println("Parse exception for rules");
}

```

名为`sortStrings`的排序例程是通用的。它将根据任何`Collator`对象的规则对任何单词数组进行排序：

```java
public static void sortStrings(Collator collator, String[] words) {
    String tmp;
    for (int i = 0; i < words.length; i++) {
        for (int j = i + 1; j < words.length; j++) {
            if (collator.compare(words[i], words[j]) > 0) {
                tmp = words[i];
                words[i] = words[j];
                words[j] = tmp;
            }
        }
    }
}

```

使用英语整理规则排序时，单词数组如下：

```java
chalina
curioso
llama
luz

```

将前面的列表与以下按照传统西班牙整理规则排序的列表进行比较：

```java
curioso
chalina
luz
llama

```
