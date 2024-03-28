# `if-then` 和 `if-then-else` 语句

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/if.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/if.html)

## `if-then` 语句

`if-then` 语句是所有控制流语句中最基本的。它告诉你的程序仅当特定测试评估为 `true` 时才执行某个代码段。例如，`Bicycle` 类可以允许刹车减少自行车的速度，仅当自行车已经在运动中时。`applyBrakes` 方法的一个可能的实现如下：

```java
void applyBrakes() {
    // the "if" clause: bicycle must be moving
    if (isMoving){ 
        // the "then" clause: decrease current speed
        currentSpeed--;
    }
}

```

如果这个测试评估为 `false`（意味着自行车没有在运动中），控制跳转到 `if-then` 语句的末尾。

此外，如果“then”子句只包含一个语句，则开头和结尾的大括号是可选的：

```java
void applyBrakes() {
    // same as above, but without braces 
    if (isMoving)
        currentSpeed--;
}

```

决定何时省略大括号是个人品味的问题。省略它们可能会使代码更脆弱。如果稍后向“then”子句添加第二个语句，一个常见的错误是忘记添加新需要的大括号。编译器无法捕捉到这种错误；你只会得到错误的结果。

## `if-then-else` 语句

`if-then-else` 语句在“if”子句评估为 `false` 时提供了执行的第二路径。你可以在 `applyBrakes` 方法中使用 `if-then-else` 语句，如果自行车在静止状态下刹车，就采取一些行动。在这种情况下，行动就是简单地打印一个错误消息，说明自行车已经停止了。

```java
void applyBrakes() {
    if (isMoving) {
        currentSpeed--;
    } else {
        System.err.println("The bicycle has already stopped!");
    } 
}

```

以下程序，`IfElseDemo`，根据测试分数的值分配等级：90% 或以上为 A，80% 或以上为 B，依此类推。

```java

class IfElseDemo {
    public static void main(String[] args) {

        int testscore = 76;
        char grade;

        if (testscore >= 90) {
            grade = 'A';
        } else if (testscore >= 80) {
            grade = 'B';
        } else if (testscore >= 70) {
            grade = 'C';
        } else if (testscore >= 60) {
            grade = 'D';
        } else {
            grade = 'F';
        }
        System.out.println("Grade = " + grade);
    }
}

```

程序的输出是：

```java
    Grade = C

```

你可能已经注意到 `testscore` 的值可以满足复合语句中的多个表达式：`76 >= 70` 和 `76 >= 60`。然而，一旦条件满足，适当的语句就会被执行（`grade = 'C';`），剩余的条件就不会被评估。
