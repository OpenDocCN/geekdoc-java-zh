# switch 语句

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html)

与 `if-then` 和 `if-then-else` 语句不同，`switch` 语句可以有多个可能的执行路径。`switch` 适用于 `byte`、`short`、`char` 和 `int` 原始数据类型。它还适用于*枚举类型*（在 枚举类型 中讨论）、[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) 类以及包装某些原始类型的几个特殊类：[`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html)、[`Byte`](https://docs.oracle.com/javase/8/docs/api/java/lang/Byte.html)、[`Short`](https://docs.oracle.com/javase/8/docs/api/java/lang/Short.html) 和 [`Integer`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html)（在 数字和字符串 中讨论）。

以下代码示例，`SwitchDemo`，声明了一个名为 `month` 的 `int`，其值表示一个月份。该代码根据 `month` 的值使用 `switch` 语句显示月份的名称。

```java

public class SwitchDemo {
    public static void main(String[] args) {

        int month = 8;
        String monthString;
        switch (month) {
            case 1:  monthString = "January";
                     break;
            case 2:  monthString = "February";
                     break;
            case 3:  monthString = "March";
                     break;
            case 4:  monthString = "April";
                     break;
            case 5:  monthString = "May";
                     break;
            case 6:  monthString = "June";
                     break;
            case 7:  monthString = "July";
                     break;
            case 8:  monthString = "August";
                     break;
            case 9:  monthString = "September";
                     break;
            case 10: monthString = "October";
                     break;
            case 11: monthString = "November";
                     break;
            case 12: monthString = "December";
                     break;
            default: monthString = "Invalid month";
                     break;
        }
        System.out.println(monthString);
    }
}

```

在这种情况下，`August` 被打印到标准输出。

`switch` 语句的主体称为*switch 块*。`switch` 块中的语句可以带有一个或多个 `case` 或 `default` 标签。`switch` 语句评估其表达式，然后执行所有匹配 `case` 标签后的语句。

你也可以使用 `if-then-else` 语句显示月份的名称：

```java
int month = 8;
if (month == 1) {
    System.out.println("January");
} else if (month == 2) {
    System.out.println("February");
}
...  // and so on

```

决定是使用 `if-then-else` 语句还是 `switch` 语句取决于可读性和语句测试的表达式。`if-then-else` 语句可以测试基于值范围或条件的表达式，而 `switch` 语句仅基于单个整数、枚举值或 `String` 对象测试表达式。

`break` 语句是另一个有趣的点。每个 `break` 语句终止包含的 `switch` 语句。控制流继续执行 `switch` 块后的第一条语句。`break` 语句是必要的，因为没有它们，`switch` 块中的语句会*穿透*：匹配的 `case` 标签后的所有语句会按顺序执行，而不管后续 `case` 标签的表达式如何，直到遇到 `break` 语句。程序 `SwitchDemoFallThrough` 展示了在 `switch` 块中穿透的语句。该程序显示了对应整数 `month` 的月份以及该年后续的月份：

```java

public class SwitchDemoFallThrough {

    public static void main(String[] args) {
        java.util.ArrayList<String> futureMonths =
            new java.util.ArrayList<String>();

        int month = 8;

        switch (month) {
            case 1:  futureMonths.add("January");
            case 2:  futureMonths.add("February");
            case 3:  futureMonths.add("March");
            case 4:  futureMonths.add("April");
            case 5:  futureMonths.add("May");
            case 6:  futureMonths.add("June");
            case 7:  futureMonths.add("July");
            case 8:  futureMonths.add("August");
            case 9:  futureMonths.add("September");
            case 10: futureMonths.add("October");
            case 11: futureMonths.add("November");
            case 12: futureMonths.add("December");
                     break;
            default: break;
        }

        if (futureMonths.isEmpty()) {
            System.out.println("Invalid month number");
        } else {
            for (String monthName : futureMonths) {
               System.out.println(monthName);
            }
        }
    }
}

```

这是代码的输出：

```java
August
September
October
November
December

```

从技术上讲，最后的`break`并不是必需的，因为流程会跳出`switch`语句。建议使用`break`，这样修改代码会更容易，也更少出错。`default`部分处理了所有未被`case`部分显式处理的值。

以下代码示例，`SwitchDemo2`，展示了一个语句可以有多个`case`标签。该代码示例计算了特定月份的天数：

```java

class SwitchDemo2 {
    public static void main(String[] args) {

        int month = 2;
        int year = 2000;
        int numDays = 0;

        switch (month) {
            case 1: case 3: case 5:
            case 7: case 8: case 10:
            case 12:
                numDays = 31;
                break;
            case 4: case 6:
            case 9: case 11:
                numDays = 30;
                break;
            case 2:
                if (((year % 4 == 0) && 
                     !(year % 100 == 0))
                     || (year % 400 == 0))
                    numDays = 29;
                else
                    numDays = 28;
                break;
            default:
                System.out.println("Invalid month.");
                break;
        }
        System.out.println("Number of Days = "
                           + numDays);
    }
}

```

这是代码的输出：

```java
Number of Days = 29

```

## 在`switch`语句中使用字符串。

在 Java SE 7 及更高版本中，可以在`switch`语句的表达式中使用`String`对象。以下代码示例，`StringSwitchDemo`，根据名为`month`的`String`的值显示月份的数字：

```java

public class StringSwitchDemo {

    public static int getMonthNumber(String month) {

        int monthNumber = 0;

        if (month == null) {
            return monthNumber;
        }

        switch (month.toLowerCase()) {
            case "january":
                monthNumber = 1;
                break;
            case "february":
                monthNumber = 2;
                break;
            case "march":
                monthNumber = 3;
                break;
            case "april":
                monthNumber = 4;
                break;
            case "may":
                monthNumber = 5;
                break;
            case "june":
                monthNumber = 6;
                break;
            case "july":
                monthNumber = 7;
                break;
            case "august":
                monthNumber = 8;
                break;
            case "september":
                monthNumber = 9;
                break;
            case "october":
                monthNumber = 10;
                break;
            case "november":
                monthNumber = 11;
                break;
            case "december":
                monthNumber = 12;
                break;
            default: 
                monthNumber = 0;
                break;
        }

        return monthNumber;
    }

    public static void main(String[] args) {

        String month = "August";

        int returnedMonthNumber =
            StringSwitchDemo.getMonthNumber(month);

        if (returnedMonthNumber == 0) {
            System.out.println("Invalid month");
        } else {
            System.out.println(returnedMonthNumber);
        }
    }
}

```

这段代码的输出是`8`。

`switch`表达式中的`String`与与每个`case`标签关联的表达式进行比较，就好像使用了[`String.equals`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#equals-java.lang.Object-)方法一样。为了使`StringSwitchDemo`示例接受任何大小写的月份，`month`被转换为小写（使用[`toLowerCase`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#toLowerCase--)方法），并且所有与`case`标签关联的字符串都是小写的。

**注意**：此示例检查`switch`语句中的表达式是否为`null`。确保任何`switch`语句中的表达式不为 null，以防止抛出`NullPointerException`。
