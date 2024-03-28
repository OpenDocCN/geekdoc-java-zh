# 分支语句

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/branch.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/branch.html)

## `break` 语句

`break` 语句有两种形式：带标签和未标记。在前面讨论 `switch` 语句时看到了未标记形式。您还可以使用未标记的 `break` 终止 `for`、`while` 或 `do-while` 循环，如下所示的 `BreakDemo` 程序：

```java
class BreakDemo {
    public static void main(String[] args) {

        int[] arrayOfInts = 
            { 32, 87, 3, 589,
              12, 1076, 2000,
              8, 622, 127 };
        int searchfor = 12;

        int i;
        boolean foundIt = false;

        for (i = 0; i < arrayOfInts.length; i++) {
            if (arrayOfInts[i] == searchfor) {
                foundIt = true;
                break;
            }
        }

        if (foundIt) {
            System.out.println("Found " + searchfor + " at index " + i);
        } else {
            System.out.println(searchfor + " not in the array");
        }
    }
}

```

该程序在数组中搜索数字 12。粗体显示的 `break` 语句在找到该值时终止 `for` 循环。然后控制流转移到 `for` 循环后的语句。该程序的输出为：

```java
Found 12 at index 4

```

一个未标记的 `break` 语句终止最内层的 `switch`、`for`、`while` 或 `do-while` 语句，但带标签的 `break` 终止外部语句。下面的程序，`BreakWithLabelDemo`，类似于前一个程序，但使用嵌套的 `for` 循环在二维数组中搜索一个值。当找到该值时，带标签的 `break` 终止外部的 `for` 循环（标记为 "search"）：

```java

class BreakWithLabelDemo {
    public static void main(String[] args) {

        int[][] arrayOfInts = { 
            { 32, 87, 3, 589 },
            { 12, 1076, 2000, 8 },
            { 622, 127, 77, 955 }
        };
        int searchfor = 12;

        int i;
        int j = 0;
        boolean foundIt = false;

    search:
        for (i = 0; i < arrayOfInts.length; i++) {
            for (j = 0; j < arrayOfInts[i].length;
                 j++) {
                if (arrayOfInts[i][j] == searchfor) {
                    foundIt = true;
                    break search;
                }
            }
        }

        if (foundIt) {
            System.out.println("Found " + searchfor + " at " + i + ", " + j);
        } else {
            System.out.println(searchfor + " not in the array");
        }
    }
}

```

这是程序的输出。

```java
Found 12 at 1, 0

```

`break` 语句终止带标签的语句；它不会将控制流转移到标签处。控制流会转移到带标签的（终止的）语句之后的语句。

## `continue` 语句

`continue` 语句跳过 `for`、`while` 或 `do-while` 循环的当前迭代。未标记形式跳到最内层循环体的末尾并评估控制循环的 `boolean` 表达式。下面的程序，`ContinueDemo`，遍历一个 `String`，计算字母 "p" 的出现次数。如果当前字符不是 p，则 `continue` 语句跳过循环的其余部分并继续下一个字符。如果是 "p"，程序会增加字母计数。

```java

class ContinueDemo {
    public static void main(String[] args) {

        String searchMe = "peter piper picked a " + "peck of pickled peppers";
        int max = searchMe.length();
        int numPs = 0;

        for (int i = 0; i < max; i++) {
            // interested only in p's
            if (searchMe.charAt(i) != 'p')
                continue;

            // process p's
            numPs++;
        }
        System.out.println("Found " + numPs + " p's in the string.");
    }
}

```

这是该程序的输出：

```java
Found 9 p's in the string.

```

要更清楚地看到这种效果，请尝试删除 `continue` 语句并重新编译。再次运行程序时，计数将出错，显示找到了 35 个 p，而不是 9 个。

带标签的 `continue` 语句跳过带有给定标签的外部循环的当前迭代。下面的示例程序，`ContinueWithLabelDemo`，使用嵌套循环在另一个字符串中搜索子字符串。需要两个嵌套循环：一个用于迭代子字符串，一个用于迭代被搜索的字符串。下面的程序，`ContinueWithLabelDemo`，使用带标签的 continue 形式跳过外部循环的一个迭代。

```java

class ContinueWithLabelDemo {
    public static void main(String[] args) {

        String searchMe = "Look for a substring in me";
        String substring = "sub";
        boolean foundIt = false;

        int max = searchMe.length() - 
                  substring.length();

    test:
        for (int i = 0; i <= max; i++) {
            int n = substring.length();
            int j = i;
            int k = 0;
            while (n-- != 0) {
                if (searchMe.charAt(j++) != substring.charAt(k++)) {
                    continue test;
                }
            }
            foundIt = true;
                break test;
        }
        System.out.println(foundIt ? "Found it" : "Didn't find it");
    }
}

```

这是该程序的输出。

```java
Found it

```

## `return` 语句

分支语句中的最后一个是`return`语句。`return`语句退出当前方法，控制流返回到调用方法的位置。`return`语句有两种形式：一种返回一个值，另一种不返回。要返回一个值，只需在`return`关键字后面放置该值（或计算该值的表达式）。

```java
return ++count;

```

返回值的数据类型必须与方法声明的返回值类型匹配。当方法声明为`void`时，使用不返回值的`return`形式。

```java
return;

```

类和对象课程将涵盖你需要了解的关于编写方法的一切。
