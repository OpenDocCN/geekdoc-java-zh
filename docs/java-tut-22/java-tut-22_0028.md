# 赋值、算术和一元运算符

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/op1.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op1.html)

## 简单赋值运算符

您将遇到的最常见运算符之一是简单赋值运算符"`=`"。您在自行车类中看到了这个运算符；它将右侧的值分配给左侧的操作数：

```java
 int cadence = 0;
 int speed = 0;
 int gear = 1;

```

这个运算符也可以用于对象上，分配*对象引用*，如创建对象中所讨论的。

## 算术运算符

Java 编程语言提供了执行加法、减法、乘法和除法的运算符。您很可能会通过基本数学中的对应物来认识它们。唯一可能看起来新的符号是"`%`"，它将一个操作数除以另一个操作数，并返回余数作为其结果。

| 运算符 | 描述 |
| --- | --- |
| `+` | 加法运算符（也用于字符串连接） |
| `-` | 减法运算符 |
| `*` | 乘法运算符 |
| `/` | 除法运算符 |
| `%` | 取余运算符 |

下面的程序，`ArithmeticDemo`，测试了算术运算符。

```java

class ArithmeticDemo {

    public static void main (String[] args) {

        int result = 1 + 2;
        // result is now 3
        System.out.println("1 + 2 = " + result);
        int original_result = result;

        result = result - 1;
        // result is now 2
        System.out.println(original_result + " - 1 = " + result);
        original_result = result;

        result = result * 2;
        // result is now 4
        System.out.println(original_result + " * 2 = " + result);
        original_result = result;

        result = result / 2;
        // result is now 2
        System.out.println(original_result + " / 2 = " + result);
        original_result = result;

        result = result + 8;
        // result is now 10
        System.out.println(original_result + " + 8 = " + result);
        original_result = result;

        result = result % 7;
        // result is now 3
        System.out.println(original_result + " % 7 = " + result);
    }
}

```

该程序打印如下内容：

```java
1 + 2 = 3
3 - 1 = 2
2 * 2 = 4
4 / 2 = 2
2 + 8 = 10
10 % 7 = 3

```

您还可以将算术运算符与简单赋值运算符结合使用，创建*复合赋值*。例如，`x+=1;` 和 `x=x+1;` 都会将 `x` 的值增加 1。

`+` 运算符也可以用于连接（拼接）两个字符串，如下所示的`ConcatDemo`程序：

```java

class ConcatDemo {
    public static void main(String[] args){
        String firstString = "This is";
        String secondString = " a concatenated string.";
        String thirdString = firstString+secondString;
        System.out.println(thirdString);
    }
}

```

到程序结束时，变量 `thirdString` 包含"这是一个连接的字符串。"，并将其打印到标准输出。

## 一元运算符

一元运算符只需要一个操作数；它们执行各种操作，如将值增加/减少一，否定表达式，或反转布尔值的值。

| 运算符 | 描述 |
| --- | --- |
| `+` | 一元加号运算符；表示正值（数字没有这个也是正的） |
| `-` | 一元减号运算符；否定表达式 |
| `++` | 递增运算符；将值增加 1 |
| `--` | 递减运算符；将值减少 1 |
| `!` | 逻辑补运算符；反转布尔值的值 |

下面的程序，`UnaryDemo`，测试了一元运算符：

```java

class UnaryDemo {

    public static void main(String[] args) {

        int result = +1;
        // result is now 1
        System.out.println(result);

        result--;
        // result is now 0
        System.out.println(result);

        result++;
        // result is now 1
        System.out.println(result);

        result = -result;
        // result is now -1
        System.out.println(result);

        boolean success = false;
        // false
        System.out.println(success);
        // true
        System.out.println(!success);
    }
}

```

递增/递减运算符可以在操作数之前（前缀）或之后（后缀）应用。代码 `result++;` 和 `++result;` 都会使 `result` 增加一。唯一的区别在于前缀版本（`++result`）会计算为递增后的值，而后缀版本（`result++`）会计算为原始值。如果只是进行简单的递增/递减，选择哪个版本并不重要。但如果在较大表达式的一部分中使用此运算符，则您选择的版本可能会产生重大差异。

以下程序，`PrePostDemo`，演示了前缀/后缀一元递增运算符：

```java

class PrePostDemo {
    public static void main(String[] args){
        int i = 3;
        i++;
        // prints 4
        System.out.println(i);
        ++i;			   
        // prints 5
        System.out.println(i);
        // prints 6
        System.out.println(++i);
        // prints 6
        System.out.println(i++);
        // prints 7
        System.out.println(i);
    }
}

```
