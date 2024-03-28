# for 语句

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html)

`for`语句提供了一种紧凑的方式来遍历一系列值。程序员通常将其称为"for 循环"，因为它会重复循环，直到满足特定条件为止。`for`语句的一般形式可以表示如下：

```java
for (*initialization*; *termination*;
     *increment*) {
    *statement(s)*
}

```

使用这个版本的`for`语句时，请记住：

+   *初始化*表达式初始化循环；它在循环开始时执行一次。

+   当*终止*表达式评估为`false`时，循环终止。

+   *增量*表达式在每次循环迭代之后被调用；这个表达式完全可以递增或递减一个值。

以下程序`ForDemo`使用`for`语句的一般形式将数字 1 到 10 打印到标准输出：

```java

class ForDemo {
    public static void main(String[] args){
         for(int i=1; i<11; i++){
              System.out.println("Count is: " + i);
         }
    }
}

```

该程序的输出是：

```java
Count is: 1
Count is: 2
Count is: 3
Count is: 4
Count is: 5
Count is: 6
Count is: 7
Count is: 8
Count is: 9
Count is: 10

```

请注意代码如何在初始化表达式中声明一个变量。该变量的作用域从其声明延伸到由`for`语句控制的块的末尾，因此它也可以在终止和增量表达式中使用。如果控制`for`语句的变量在循环外不需要，则最好在初始化表达式中声明该变量。通常使用`i`、`j`和`k`这些名称来控制`for`循环；在初始化表达式中声明它们会限制它们的生命周期并减少错误。

`for`循环的三个表达式是可选的；可以创建一个无限循环，如下所示：

```java
// infinite loop
for ( ; ; ) {

    // your code goes here
}

```

`for`语句还有另一种形式，专为遍历集合和数组设计。这种形式有时被称为*增强型 for*语句，可使您的循环更加简洁和易读。为了演示，考虑以下数组，其中包含 1 到 10 的数字：

```java
int[] numbers = {1,2,3,4,5,6,7,8,9,10};

```

以下程序`EnhancedForDemo`使用增强型`for`循环遍历数组：

```java

class EnhancedForDemo {
    public static void main(String[] args){
         int[] numbers = 
             {1,2,3,4,5,6,7,8,9,10};
         for (int item : numbers) {
             System.out.println("Count is: " + item);
         }
    }
}

```

在这个例子中，变量`item`保存来自数字数组的当前值。该程序的输出与之前相同：

```java
Count is: 1
Count is: 2
Count is: 3
Count is: 4
Count is: 5
Count is: 6
Count is: 7
Count is: 8
Count is: 9
Count is: 10

```

我们建议尽可能使用这种形式的`for`语句，而不是一般形式。
