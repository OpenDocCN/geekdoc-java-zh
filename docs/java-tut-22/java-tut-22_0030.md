# 位与位移操作符

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/op3.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op3.html)

Java 编程语言还提供对整数类型执行位和位移操作的运算符。本节讨论的运算符很少使用。因此，它们的覆盖范围很简短；目的只是让您知道这些运算符存在。

一元位取反运算符"`~`"反转位模式；它可以应用于任何整数类型，使每个"0"变为"1"，每个"1"变为"0"。例如，一个`byte`包含 8 位；将此运算符应用于位模式为"00000000"的值将使其模式变为"11111111"。

有符号左移位运算符"`<<`"将位模式向左移动，有符号右移位运算符"`>>`"将位模式向右移动。位模式由左操作数给出，要移动的位置数由右操作数给出。无符号右移位运算符"`>>>`"将零移入最左侧位置，而`">>"`后的最左侧位置取决于符号扩展。

位运算符`&`执行位按位与操作。

位运算符`^`执行位按位异或操作。

位运算符`|`执行位按位包含或操作。

以下程序，`BitDemo`，使用位与运算符将数字"2"打印到标准输出。

```java

class BitDemo {
    public static void main(String[] args) {
        int bitmask = 0x000F;
        int val = 0x2222;
        // prints "2"
        System.out.println(val & bitmask);
    }
}

```
