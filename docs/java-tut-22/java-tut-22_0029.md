# 相等、关系和条件运算符

> 原文：[`docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html`](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html)

## 相等和关系运算符

相等和关系运算符确定一个操作数是否大于、小于、等于或不等于另一个操作数。这些运算符中的大多数可能对您来说看起来很熟悉。请记住，在测试两个原始值是否相等时，必须使用"`==`"，而不是"`=`"。

```java
==      equal to
!=      not equal to
>       greater than
>=      greater than or equal to
<       less than
<=      less than or equal to

```

以下程序，`ComparisonDemo`，测试比较运算符：

```java

class ComparisonDemo {

    public static void main(String[] args){
        int value1 = 1;
        int value2 = 2;
        if(value1 == value2)
            System.out.println("value1 == value2");
        if(value1 != value2)
            System.out.println("value1 != value2");
        if(value1 > value2)
            System.out.println("value1 > value2");
        if(value1 < value2)
            System.out.println("value1 < value2");
        if(value1 <= value2)
            System.out.println("value1 <= value2");
    }
}

```

输出：

```java
value1 != value2
value1 <  value2
value1 <= value2

```

## 条件运算符

`&&`和`||`运算符对两个布尔表达式执行*条件-AND*和*条件-OR*操作。这些运算符表现出“短路”行为，这意味着只有在需要时才会评估第二个操作数。

```java
&& Conditional-AND
|| Conditional-OR

```

以下程序，`ConditionalDemo1`，测试了这些运算符：

```java

class ConditionalDemo1 {

    public static void main(String[] args){
        int value1 = 1;
        int value2 = 2;
        if((value1 == 1) && (value2 == 2))
            System.out.println("value1 is 1 AND value2 is 2");
        if((value1 == 1) || (value2 == 1))
            System.out.println("value1 is 1 OR value2 is 1");
    }
}

```

另一个条件运算符是`?:`，可以被视为`if-then-else`语句的简写（在本课程的控制流语句部分讨论）。这个运算符也被称为*三元运算符*，因为它使用三个操作数。在下面的例子中，这个运算符应该被理解为：“如果`someCondition`为`true`，则将`value1`的值赋给`result`。否则，将`value2`的值赋给`result`。”

以下程序，`ConditionalDemo2`，测试了`?:`运算符：

```java

class ConditionalDemo2 {

    public static void main(String[] args){
        int value1 = 1;
        int value2 = 2;
        int result;
        boolean someCondition = true;
        result = someCondition ? value1 : value2;

        System.out.println(result);
    }
}

```

因为`someCondition`为真，这个程序将"1"打印到屏幕上。如果使用`?:`运算符而不是`if-then-else`语句可以使您的代码更易读；例如，当表达式紧凑且没有副作用（如赋值）时。

## 类型比较运算符 instanceof

`instanceof`运算符将对象与指定类型进行比较。您可以使用它来测试对象是否是类的实例，子类的实例，或者实现特定接口的类的实例。

以下程序，`InstanceofDemo`，定义了一个父类（名为`Parent`），一个简单接口（名为`MyInterface`），以及一个继承自父类并实现接口的子类（名为`Child`）。

```java

class InstanceofDemo {
    public static void main(String[] args) {

        Parent obj1 = new Parent();
        Parent obj2 = new Child();

        System.out.println("obj1 instanceof Parent: "
            + (obj1 instanceof Parent));
        System.out.println("obj1 instanceof Child: "
            + (obj1 instanceof Child));
        System.out.println("obj1 instanceof MyInterface: "
            + (obj1 instanceof MyInterface));
        System.out.println("obj2 instanceof Parent: "
            + (obj2 instanceof Parent));
        System.out.println("obj2 instanceof Child: "
            + (obj2 instanceof Child));
        System.out.println("obj2 instanceof MyInterface: "
            + (obj2 instanceof MyInterface));
    }
}

class Parent {}
class Child extends Parent implements MyInterface {}
interface MyInterface {}

```

输出：

```java
obj1 instanceof Parent: true
obj1 instanceof Child: false
obj1 instanceof MyInterface: false
obj2 instanceof Parent: true
obj2 instanceof Child: true
obj2 instanceof MyInterface: true

```

当使用`instanceof`运算符时，请记住`null`不是任何东西的实例。
