# 比较字符串和字符串部分

> 原文：[`docs.oracle.com/javase/tutorial/java/data/comparestrings.html`](https://docs.oracle.com/javase/tutorial/java/data/comparestrings.html)

`String`类有许多用于比较字符串和字符串部分的方法。以下表格列出了这些方法。

比较字符串的方法

| 方法 | 描述 |
| --- | --- |
| `boolean endsWith(String suffix) boolean startsWith(String prefix)` | 如果此字符串以指定为方法参数的子字符串结尾或以其开头，则返回`true`。 |
| `boolean startsWith(String prefix, int offset)` | 考虑从索引`offset`开始的字符串，并返回如果以指定为参数的子字符串开头则返回`true`。 |
| `int compareTo(String anotherString)` | 按字典顺序比较两个字符串。返回一个整数，指示此字符串是否大于（结果为> 0）、等于（结果为= 0）或小于（结果为< 0）参数。 |
| `int compareToIgnoreCase(String str)` | 按字典顺序比较两个字符串，忽略大小写差异。返回一个整数，指示此字符串是否大于（结果为> 0）、等于（结果为= 0）或小于（结果为< 0）参数。 |
| `boolean equals(Object anObject)` | 如果参数是表示与此对象相同字符序列的`String`对象，则返回`true`。 |
| `boolean equalsIgnoreCase(String anotherString)` | 如果参数是表示与此对象相同字符序列的`String`对象，则返回`true`，忽略大小写差异。 |
| `boolean regionMatches(int toffset, String other, int ooffset, int len)` | 测试此字符串的指定区域是否与 String 参数的指定区域匹配。区域长度为`len`，从此字符串的索引`toffset`和另一个字符串的索引`ooffset`开始。 |
| `boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)` | 测试此字符串的指定区域是否与 String 参数的指定区域匹配。区域长度为`len`，从此字符串的索引`toffset`和另一个字符串的索引`ooffset`开始。布尔参数指示是否应忽略大小写；如果为 true，则在比较字符时忽略大小写。 |
| `boolean matches(String regex)` | 测试此字符串是否与指定的正则表达式匹配。正则表达式在标题为“正则表达式”的课程中讨论。 |

以下程序`RegionMatchesDemo`使用`regionMatches`方法在另一个字符串中搜索字符串：

```java

public class RegionMatchesDemo {
    public static void main(String[] args) {
        String searchMe = "Green Eggs and Ham";
        String findMe = "Eggs";
        int searchMeLength = searchMe.length();
        int findMeLength = findMe.length();
        boolean foundIt = false;
        for (int i = 0; 
             i <= (searchMeLength - findMeLength);
             i++) {
           if (searchMe.regionMatches(i, findMe, 0, findMeLength)) {
              foundIt = true;
              System.out.println(searchMe.substring(i, i + findMeLength));
              break;
           }
        }
        if (!foundIt)
            System.out.println("No match found.");
    }
}

```

此程序的输出为`Eggs`。

该程序逐个字符遍历`searchMe`引用的字符串。对于每个字符，程序调用`regionMatches`方法来确定从当前字符开始的子字符串是否与程序正在查找的字符串匹配。
