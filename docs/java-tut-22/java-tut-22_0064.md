# 本地类

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html)

本地类是在*块*中定义的类，块是在平衡大括号之间的零个或多个语句组成的组。通常在方法体中定义本地类。

本节涵盖以下主题：

+   声明本地类

+   访问封闭类的成员

    +   遮蔽和本地类

+   本地类类似于内部类

## 声明本地类

您可以在任何块中定义本地类（请参阅表达式、语句和块了解更多信息）。例如，您可以在方法体、`for`循环或`if`子句中定义本地类。

以下示例，`LocalClassExample`，验证两个电话号码。它在方法`validatePhoneNumber`中定义了本地类`PhoneNumber`：

```java

public class LocalClassExample {

    static String regularExpression = "[⁰-9]";

    public static void validatePhoneNumber(
        String phoneNumber1, String phoneNumber2) {

        final int numberLength = 10;

        // Valid in JDK 8 and later:

        // int numberLength = 10;

        class PhoneNumber {

            String formattedPhoneNumber = null;

            PhoneNumber(String phoneNumber){
                // numberLength = 7;
                String currentNumber = phoneNumber.replaceAll(
                  regularExpression, "");
                if (currentNumber.length() == numberLength)
                    formattedPhoneNumber = currentNumber;
                else
                    formattedPhoneNumber = null;
            }

            public String getNumber() {
                return formattedPhoneNumber;
            }

            // Valid in JDK 8 and later:

//            public void printOriginalNumbers() {
//                System.out.println("Original numbers are " + phoneNumber1 +
//                    " and " + phoneNumber2);
//            }
        }

        PhoneNumber myNumber1 = new PhoneNumber(phoneNumber1);
        PhoneNumber myNumber2 = new PhoneNumber(phoneNumber2);

        // Valid in JDK 8 and later:

//        myNumber1.printOriginalNumbers();

        if (myNumber1.getNumber() == null) 
            System.out.println("First number is invalid");
        else
            System.out.println("First number is " + myNumber1.getNumber());
        if (myNumber2.getNumber() == null)
            System.out.println("Second number is invalid");
        else
            System.out.println("Second number is " + myNumber2.getNumber());

    }

    public static void main(String... args) {
        validatePhoneNumber("123-456-7890", "456-7890");
    }
}

```

该示例通过首先从电话号码中删除除数字 0 到 9 之外的所有字符来验证电话号码。然后，它检查电话号码是否恰好包含十个数字（北美电话号码的长度）。该示例打印如下内容：

```java
First number is 1234567890
Second number is invalid
```

## 访问封闭类的成员

本地类可以访问其封闭类的成员。在前面的示例中，`PhoneNumber`构造函数访问成员`LocalClassExample.regularExpression`。

此外，本地类可以访问局部变量。但是，本地类只能访问声明为 final 的局部变量。当本地类访问封闭块的局部变量或参数时，它会*捕获*该变量或参数。例如，`PhoneNumber`构造函数可以访问局部变量`numberLength`，因为它声明为 final；`numberLength`是一个*捕获的变量*。

然而，从 Java SE 8 开始，本地类可以访问封闭块中的局部变量和参数，这些变量是 final 或*有效地 final*。一旦初始化后值不会改变的变量或参数是有效地 final。例如，假设变量`numberLength`没有声明为 final，并且您在`PhoneNumber`构造函数中添加了突出显示的赋值语句以将有效电话号码的长度更改为 7 位：

```java
PhoneNumber(String phoneNumber) {
    numberLength = 7;
    String currentNumber = phoneNumber.replaceAll(
        regularExpression, "");
    if (currentNumber.length() == numberLength)
        formattedPhoneNumber = currentNumber;
    else
        formattedPhoneNumber = null;
}
```

由于这个赋值语句，变量`numberLength`不再是有效地 final。因此，当内部类`PhoneNumber`尝试访问`numberLength`变量时，Java 编译器生成类似于"从内部类引用的局部变量必须是 final 或有效地 final"的错误消息：

```java
if (currentNumber.length() == numberLength)
```

从 Java SE 8 开始，如果你在方法中声明局部类，它可以访问方法的参数。例如，你可以在`PhoneNumber`局部类中定义以下方法：

```java
public void printOriginalNumbers() {
    System.out.println("Original numbers are " + phoneNumber1 +
        " and " + phoneNumber2);
}
```

方法`printOriginalNumbers`访问方法`validatePhoneNumber`的参数`phoneNumber1`和`phoneNumber2`。

### 遮蔽和局部类

在局部类中声明的类型（如变量）会遮蔽在外部作用域中具有相同名称的声明。更多信息请参见 Shadowing。

## 局部类类似于内部类

局部类类似于内部类，因为它们不能定义或声明任何静态成员。在静态方法中的局部类，比如在静态方法`validatePhoneNumber`中定义的`PhoneNumber`类，只能引用封闭类的静态成员。例如，如果你没有将成员变量`regularExpression`定义为静态的，那么 Java 编译器会生成类似“非静态变量`regularExpression`无法从静态上下文中引用”的错误。

局部类是非静态的，因为它们可以访问封闭块的实例成员。因此，它们不能包含大多数类型的静态声明。

你不能在块内部声明接口；接口本质上是静态的。例如，以下代码摘录不会编译，因为接口`HelloThere`是在方法`greetInEnglish`的主体内定义的：

```java
    public void greetInEnglish() {
        interface HelloThere {
           public void greet();
        }
        class EnglishHelloThere implements HelloThere {
            public void greet() {
                System.out.println("Hello " + name);
            }
        }
        HelloThere myGreeting = new EnglishHelloThere();
        myGreeting.greet();
    }
```

你不能在局部类中声明静态初始化程序或成员接口。以下代码摘录不会编译，因为方法`EnglishGoodbye.sayGoodbye`被声明为`static`。当编译器遇到这个方法定义时，会生成类似“修饰符'static'仅允许在常量变量声明中使用”的错误：

```java
    public void sayGoodbyeInEnglish() {
        class EnglishGoodbye {
            public static void sayGoodbye() {
                System.out.println("Bye bye");
            }
        }
        EnglishGoodbye.sayGoodbye();
    }
```

局部类可以拥有静态成员，前提是它们是常量变量。（*常量变量*是指声明为 final 并用编译时常量表达式初始化的原始类型或`String`类型的变量。编译时常量表达式通常是一个可以在编译时评估的字符串或算术表达式。更多信息请参见理解类成员。）以下代码摘录可以编译，因为静态成员`EnglishGoodbye.farewell`是一个常量变量：

```java
    public void sayGoodbyeInEnglish() {
        class EnglishGoodbye {
            public static final String farewell = "Bye bye";
            public void sayGoodbye() {
                System.out.println(farewell);
            }
        }
        EnglishGoodbye myEnglishGoodbye = new EnglishGoodbye();
        myEnglishGoodbye.sayGoodbye();
    }
```
