# while 和 do-while 语句

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/while.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/while.html)

`while`语句在特定条件为`true`时持续执行一组语句。其语法可以表示为：

```java
while (expression) {
     statement(s)
}

```

`while`语句评估*表达式*，该表达式必须返回一个`boolean`值。如果表达式评估为`true`，`while`语句执行`while`块中的*语句*。`while`语句继续测试表达式并执行其块，直到表达式评估为`false`。使用`while`语句打印从 1 到 10 的值可以通过以下`WhileDemo`程序实现：

```java

class WhileDemo {
    public static void main(String[] args){
        int count = 1;
        while (count < 11) {
            System.out.println("Count is: " + count);
            count++;
        }
    }
}

```

您可以使用`while`语句实现无限循环，如下所示：

```java
while (true){
    // your code goes here
}

```

Java 编程语言还提供了`do-while`语句，可以表示如下：

```java
do {
     statement(s)
} while (expression);

```

`do-while`和`while`之间的区别在于，`do-while`在循环底部评估其表达式，而不是在顶部。因此，`do`块内的语句始终至少执行一次，如下所示的`DoWhileDemo`程序中所示：

```java

class DoWhileDemo {
    public static void main(String[] args){
        int count = 1;
        do {
            System.out.println("Count is: " + count);
            count++;
        } while (count < 11);
    }
}

```
