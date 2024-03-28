# 嵌套类

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/nested.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)

Java 编程语言允许您在另一个类中定义一个类。这样的类称为*嵌套类*，如下所示：

```java
class OuterClass {
    ...
    class NestedClass {
        ...
    }
}

```

* * *

**术语：** 嵌套类分为两类：非静态和静态。非静态嵌套类称为*内部类*。声明为`static`的嵌套类称为*静态嵌套类*。

* * *

```java
class OuterClass {
    ...
    class InnerClass {
        ...
    }
    static class StaticNestedClass {
        ...
    }
}

```

嵌套类是其封闭类的成员。非静态嵌套类（内部类）可以访问封闭类的其他成员，即使它们被声明为 private。静态嵌套类无法访问封闭类的其他成员。作为`OuterClass`的成员，嵌套类可以声明为`private`、`public`、`protected`或*包私有*。（请记住，外部类只能声明为`public`或*包私有*。）

## 为什么使用嵌套类？

使用嵌套类的引人注目的原因包括以下内容：

+   **它是一种逻辑上将仅在一个地方使用的类分组的方法**：如果一个类仅对另一个类有用，则将其嵌入该类并将两者保持在一起是合乎逻辑的。嵌套这样的“辅助类”使其包更加简洁。

+   **它增加了封装性**：考虑两个顶层类 A 和 B，其中 B 需要访问 A 的成员，否则这些成员将被声明为`private`。通过将类 B 隐藏在类 A 中，A 的成员可以被声明为 private，并且 B 可以访问它们。此外，B 本身可以对外部世界隐藏。

+   **它可以导致更易读和易维护的代码**：将小类嵌套在顶层类中可以使代码更接近其使用位置。

## 内部类

与实例方法和变量一样，内部类与其封闭类的实例相关联，并且可以直接访问该对象的方法和字段。此外，因为内部类与实例相关联，它本身不能定义任何静态成员。

作为内部类的实例存在于外部类的实例*内部*。考虑以下类：

```java
class OuterClass {
    ...
    class InnerClass {
        ...
    }
}

```

`InnerClass`的实例只能存在于`OuterClass`的实例中，并且可以直接访问其封闭实例的方法和字段。

要实例化内部类，必须首先实例化外部类。然后，使用以下语法在外部对象中创建内部对象：

```java
OuterClass outerObject = new OuterClass();
OuterClass.InnerClass innerObject = outerObject.new InnerClass();

```

有两种特殊类型的内部类：局部类和匿名类。

## 静态嵌套类

与类方法和变量一样，静态嵌套类与其外部类相关联。并且像静态类方法一样，静态嵌套类不能直接引用其封闭类中定义的实例变量或方法：它只能通过对象引用使用它们。内部类和嵌套静态类示例演示了这一点。

* * *

**注意：** 静态嵌套类与其外部类（以及其他类）的实例成员交互方式与任何其他顶层类相同。实际上，静态嵌套类在行为上就像是为了包装方便而嵌套在另一个顶层类中的顶层类。内部类和嵌套静态类示例也演示了这一点。

* * *

您可以像实例化顶层类一样实例化静态嵌套类：

```java
StaticNestedClass staticNestedObject = new StaticNestedClass();

```

## 内部类和嵌套静态类示例

以下示例，`OuterClass`，以及`TopLevelClass`，演示了内部类(`InnerClass`)、嵌套静态类(`StaticNestedClass`)和顶层类(`TopLevelClass`)可以访问`OuterClass`的哪些类成员：

### OuterClass.java

```java

public class OuterClass {

    String outerField = "Outer field";
    static String staticOuterField = "Static outer field";

    class InnerClass {
        void accessMembers() {
            System.out.println(outerField);
            System.out.println(staticOuterField);
        }
    }

    static class StaticNestedClass {
        void accessMembers(OuterClass outer) {
            // Compiler error: Cannot make a static reference to the non-static
            //     field outerField
            // System.out.println(outerField);
            System.out.println(outer.outerField);
            System.out.println(staticOuterField);
        }
    }

    public static void main(String[] args) {
        System.out.println("Inner class:");
        System.out.println("------------");
        OuterClass outerObject = new OuterClass();
        OuterClass.InnerClass innerObject = outerObject.new InnerClass();
        innerObject.accessMembers();

        System.out.println("\nStatic nested class:");
        System.out.println("--------------------");
        StaticNestedClass staticNestedObject = new StaticNestedClass();        
        staticNestedObject.accessMembers(outerObject);

        System.out.println("\nTop-level class:");
        System.out.println("--------------------");
        TopLevelClass topLevelObject = new TopLevelClass();        
        topLevelObject.accessMembers(outerObject);                
    }
}
```

### TopLevelClass.java

```java

public class TopLevelClass {

    void accessMembers(OuterClass outer) {     
        // Compiler error: Cannot make a static reference to the non-static
        //     field OuterClass.outerField
        // System.out.println(OuterClass.outerField);
        System.out.println(outer.outerField);
        System.out.println(OuterClass.staticOuterField);
    }  
}
```

此示例打印以下输出：

```java
Inner class:
------------
Outer field
Static outer field

Static nested class:
--------------------
Outer field
Static outer field

Top-level class:
--------------------
Outer field
Static outer field
```

请注意，静态嵌套类与其外部类的实例成员交互方式与任何其他顶层类相同。静态嵌套类`StaticNestedClass`无法直接访问`outerField`，因为它是封闭类`OuterClass`的实例变量。Java 编译器会在突出显示的语句处生成错误：

```java
static class StaticNestedClass {
    void accessMembers(OuterClass outer) {
       // Compiler error: Cannot make a static reference to the non-static
       //     field outerField
       System.out.println(outerField);
    }
}

```

要修复此错误，请通过对象引用访问`outerField`：

```java
System.out.println(outer.outerField);
```

同样，顶层类`TopLevelClass`也无法直接访问`outerField`。

## 遮蔽

如果特定范围（如内部类或方法定义）中的类型声明（如成员变量或参数名）与封闭范围中的另一个声明具有相同的名称，则声明会*遮蔽*封闭范围的声明。您不能仅通过名称引用被遮蔽的声明。以下示例，`ShadowTest`，演示了这一点：

```java

public class ShadowTest {

    public int x = 0;

    class FirstLevel {

        public int x = 1;

        void methodInFirstLevel(int x) {
            System.out.println("x = " + x);
            System.out.println("this.x = " + this.x);
            System.out.println("ShadowTest.this.x = " + ShadowTest.this.x);
        }
    }

    public static void main(String... args) {
        ShadowTest st = new ShadowTest();
        ShadowTest.FirstLevel fl = st.new FirstLevel();
        fl.methodInFirstLevel(23);
    }
}

```

以下是此示例的输出：

```java
x = 23
this.x = 1
ShadowTest.this.x = 0
```

此示例定义了三个名为`x`的变量：类`ShadowTest`的成员变量，内部类`FirstLevel`的成员变量以及方法`methodInFirstLevel`中的参数。方法`methodInFirstLevel`中定义的变量`x`会遮蔽内部类`FirstLevel`的变量。因此，当您在方法`methodInFirstLevel`中使用变量`x`时，它指的是方法参数。要引用内部类`FirstLevel`的成员变量，请使用关键字`this`表示封闭范围：

```java
System.out.println("this.x = " + this.x);
```

通过类名引用封装更大范围的成员变量。例如，以下语句从方法`methodInFirstLevel`中访问类`ShadowTest`的成员变量：

```java
System.out.println("ShadowTest.this.x = " + ShadowTest.this.x);
```

## 序列化

内部类的序列化，包括局部和匿名类，是强烈不建议的。当 Java 编译器编译某些结构（如内部类）时，它会创建*合成结构*；这些是在源代码中没有对应构造的类、方法、字段和其他结构。合成结构使 Java 编译器能够实现新的 Java 语言特性，而无需更改 JVM。然而，合成结构在不同的 Java 编译器实现之间可能会有所不同，这意味着`.class`文件在不同的实现之间也可能会有所不同。因此，如果您序列化一个内部类，然后在不同的 JRE 实现中反序列化它，可能会出现兼容性问题。有关在编译内部类时生成的合成结构的更多信息，请参见隐式和合成参数部分中的获取方法参数名称部分。
