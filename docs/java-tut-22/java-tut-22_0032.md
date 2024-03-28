# 问题和练习：运算符

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/QandE/questions_operators.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/QandE/questions_operators.html)

## 问题

1.  考虑以下代码片段。

    ```java
    arrayOfInts[j] > arrayOfInts[j+1]

    ```

    代码包含哪些运算符？

1.  考虑以下代码片段。

    ```java
    int i = 10;
    int n = i++%5;

    ```

    1.  在代码执行后，`i`和`n`的值是多少？

    1.  如果不使用后缀递增运算符（`i++`），而是使用前缀版本（`++i`），`i`和`n`的最终值是多少？

1.  要反转`boolean`的值，你会使用哪个运算符？

1.  用于比较两个值的运算符是`=`还是`==`？

1.  解释以下代码示例：`result = someCondition ? value1 : value2;`

## 练习

1.  修改以下程序以使用复合赋值：

    ```java
    class ArithmeticDemo {

         public static void main (String[] args){

              int result = 1 + 2; // result is now 3
              System.out.println(result);

              result = result - 1; // result is now 2
              System.out.println(result);

              result = result * 2; // result is now 4
              System.out.println(result);

              result = result / 2; // result is now 2
              System.out.println(result);

              result = result + 8; // result is now 10
              result = result % 7; // result is now 3
              System.out.println(result);
         }
    }

    ```

1.  在以下程序中，解释为什么值“6”连续打印两次：

    ```java
    class PrePostDemo {
        public static void main(String[] args){
            int i = 3;
            i++;
            System.out.println(i);    // "4"
            ++i;                     
            System.out.println(i);    // "5"
            System.out.println(++i);  // "6"
            System.out.println(i++);  // "6"
            System.out.println(i);    // "7"
        }
    }

    ```

检查你的答案
