# 获取方法参数的名称

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/methodparameterreflection.html`](https://docs.oracle.com/javase/tutorial/reflect/member/methodparameterreflection.html)

您可以使用方法[`java.lang.reflect.Executable.getParameters`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Executable.html#getParameters--)获取任何方法或构造函数的形式参数的名称。（类[`Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html)和[`Constructor`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Executable.html)扩展了类[`Executable`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Executable.html)，因此继承了方法`Executable.getParameters`。）但是，默认情况下，`.class`文件不会存储形式参数名称。这是因为许多生成和消费类文件的工具可能不希望`.class`文件包含参数名称的更大的静态和动态占用空间。特别是，这些工具将不得不处理更大的`.class`文件，并且 Java 虚拟机（JVM）将使用更多内存。此外，一些参数名称，如`secret`或`password`，可能会暴露有关安全敏感方法的信息。

要在特定的`.class`文件中存储形式参数名称，并使反射 API 能够检索形式参数名称，请使用`javac`编译器的`-parameters`选项编译源文件。

``MethodParameterSpy``示例演示了如何检索给定类的所有构造函数和方法的形式参数的名称。该示例还打印有关每个参数的其他信息。

以下命令打印类``ExampleMethods``的构造函数和方法的形式参数名称。**注意**：记得使用`-parameters`编译器选项编译示例`ExampleMethods`：

```java
*java MethodParameterSpy ExampleMethods*
```

此命令打印以下内容：

```java
Number of constructors: 1

Constructor #1
public ExampleMethods()

Number of declared constructors: 1

Declared constructor #1
public ExampleMethods()

Number of methods: 4

Method #1
public boolean ExampleMethods.simpleMethod(java.lang.String,int)
             Return type: boolean
     Generic return type: boolean
         Parameter class: class java.lang.String
          Parameter name: stringParam
               Modifiers: 0
            Is implicit?: false
        Is name present?: true
           Is synthetic?: false
         Parameter class: int
          Parameter name: intParam
               Modifiers: 0
            Is implicit?: false
        Is name present?: true
           Is synthetic?: false

Method #2
public int ExampleMethods.varArgsMethod(java.lang.String...)
             Return type: int
     Generic return type: int
         Parameter class: class [Ljava.lang.String;
          Parameter name: manyStrings
               Modifiers: 0
            Is implicit?: false
        Is name present?: true
           Is synthetic?: false

Method #3
public boolean ExampleMethods.methodWithList(java.util.List<java.lang.String>)
             Return type: boolean
     Generic return type: boolean
         Parameter class: interface java.util.List
          Parameter name: listParam
               Modifiers: 0
            Is implicit?: false
        Is name present?: true
           Is synthetic?: false

Method #4
public <T> void ExampleMethods.genericMethod(T[],java.util.Collection<T>)
             Return type: void
     Generic return type: void
         Parameter class: class [Ljava.lang.Object;
          Parameter name: a
               Modifiers: 0
            Is implicit?: false
        Is name present?: true
           Is synthetic?: false
         Parameter class: interface java.util.Collection
          Parameter name: c
               Modifiers: 0
            Is implicit?: false
        Is name present?: true
           Is synthetic?: false
```

`MethodParameterSpy`示例使用[`Parameter`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html)类中的以下方法：

+   [`getType`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#getType--): 返回标识参数声明类型的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)对象。

+   [`getName`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#getName--): 返回参数的名称。如果参数的名称存在，则此方法返回`.class`文件提供的名称。否则，此方法将合成一个形式为`arg*N*`的名称，其中`*N*`是声明参数的方法描述符中的参数索引。

    例如，假设您编译了类`ExampleMethods`而没有指定`-parameters`编译器选项。示例`MethodParameterSpy`将为方法`ExampleMethods.simpleMethod`打印以下内容：

    ```java
    public boolean ExampleMethods.simpleMethod(java.lang.String,int)
                 Return type: boolean
         Generic return type: boolean
             Parameter class: class java.lang.String
              Parameter name: arg0
                   Modifiers: 0
                Is implicit?: false
            Is name present?: false
               Is synthetic?: false
             Parameter class: int
              Parameter name: arg1
                   Modifiers: 0
                Is implicit?: false
            Is name present?: false
               Is synthetic?: false
    ```

+   [`getModifiers`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#getModifiers--)：返回一个整数，表示形式参数具有的各种特征。如果适用于形式参数，则此值是以下值的总和：

    | 值（十进制） | 值（十六进制） | 描述 |
    | --- | --- | --- |
    | 16 | 0x0010 | 形式参数声明为`final` |
    | 4096 | 0x1000 | 形式参数是合成的。或者，您可以调用方法`isSynthetic`。 |
    | 32768 | 0x8000 | 参数在源代码中是隐式声明的。或者，您可以调用方法`isImplicit` |

+   [`isImplicit`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#isImplicit--)：如果此参数在源代码中是隐式声明的，则返回`true`。有关更多信息，请参阅隐式和合成参数部分。

+   [`isNamePresent`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#isNamePresent--)：如果参数在`.class`文件中具有名称，则返回`true`。

+   [`isSynthetic`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#isSynthetic--)：如果此参数在源代码中既不是隐式声明也不是显式声明，则返回`true`。有关更多信息，请参阅隐式和合成参数部分。

## 隐式和合成参数

如果源代码中未明确编写某些构造，则某些构造将被隐式声明。例如，``ExampleMethods``示例不包含构造函数。它将隐式声明一个默认构造函数。`MethodParameterSpy`示例打印有关`ExampleMethods`隐式声明构造函数的信息：

```java
Number of declared constructors: 1
public ExampleMethods()
```

请考虑来自``MethodParameterExamples``的以下摘录：

```java
public class MethodParameterExamples {
    public class InnerClass { }
}
```

类`InnerClass`是一个非静态嵌套类或内部类。内部类也会隐式声明一个构造函数。但是，此构造函数将包含一个参数。当 Java 编译器编译`InnerClass`时，它会创建一个代表以下代码的`.class`文件：

```java
public class MethodParameterExamples {
    public class InnerClass {
        final MethodParameterExamples parent;
        InnerClass(final MethodParameterExamples this$0) {
            parent = this$0; 
        }
    }
}
```

`InnerClass`构造函数包含一个参数，其类型是包含`InnerClass`的类，即`MethodParameterExamples`。因此，示例`MethodParameterExamples`打印如下内容：

```java
public MethodParameterExamples$InnerClass(MethodParameterExamples)
         Parameter class: class MethodParameterExamples
          Parameter name: this$0
               Modifiers: 32784
            Is implicit?: true
        Is name present?: true
           Is synthetic?: false
```

因为类`InnerClass`的构造函数是隐式声明的，所以它的参数也是隐式的。

**注意**：

+   Java 编译器为内部类的构造函数创建一个形式参数，以便编译器能够从创建表达式传递一个引用（表示立即封闭实例）到成员类的构造函数。

+   值 32784 表示`InnerClass`构造函数的参数既是 final（16）又是隐式的（32768）。

+   Java 编程语言允许在变量名中使用美元符号（`$`）；然而，按照惯例，在变量名中不使用美元符号。

Java 编译器生成的构造如果不对应于源代码中显式或隐式声明的构造，则标记为*合成的*，除非它们是类初始化方法。合成的构造是编译器生成的在不同实现之间变化的工件。考虑以下摘录来自``MethodParameterExamples``：

```java
public class MethodParameterExamples {
    enum Colors {
        RED, WHITE;
    }
}
```

当 Java 编译器遇到`enum`构造时，它会创建几个与`.class`文件结构兼容且提供`enum`构造所期望功能的方法。例如，Java 编译器会为代表以下代码的`enum`构造`Colors`创建一个`.class`文件：

```java
final class Colors extends java.lang.Enum<Colors> {
    public final static Colors RED = new Colors("RED", 0);
    public final static Colors BLUE = new Colors("WHITE", 1);

    private final static values = new Colors[]{ RED, BLUE };

    private Colors(String name, int ordinal) {
        super(name, ordinal);
    }

    public static Colors[] values(){
        return values;
    }

    public static Colors valueOf(String name){
        return (Colors)java.lang.Enum.valueOf(Colors.class, name);
    }
}
```

Java 编译器为这个`enum`构造创建了三个构造函数和方法：`Colors(String name, int ordinal)`、`Colors[] values()`和`Colors valueOf(String name)`。方法`values`和`valueOf`是隐式声明的。因此，它们的形式参数名称也是隐式声明的。

`enum`构造函数`Colors(String name, int ordinal)`是一个默认构造函数，它是隐式声明的。然而，这个构造函数的形式参数（`name`和`ordinal`）*并没有*隐式声明。因为这些形式参数既没有显式声明也没有隐式声明，它们是合成的。（`enum`构造函数的默认构造函数的形式参数不是隐式声明的，因为不同的编译器可能对这个构造函数的形式参数形式有不同的规定；另一个 Java 编译器可能为其指定不同的形式参数。当编译器编译使用`enum`常量的表达式时，它们仅依赖于`enum`构造的公共静态字段，这些字段是隐式声明的，而不依赖于它们的构造函数或这些常量是如何初始化的。）

因此，示例`MethodParameterExample`关于`enum`构造`Colors`打印如下内容：

```java
enum Colors:

Number of constructors: 0

Number of declared constructors: 1

Declared constructor #1
private MethodParameterExamples$Colors()
         Parameter class: class java.lang.String
          Parameter name: $enum$name
               Modifiers: 4096
            Is implicit?: false
        Is name present?: true
           Is synthetic?: true
         Parameter class: int
          Parameter name: $enum$ordinal
               Modifiers: 4096
            Is implicit?: false
        Is name present?: true
           Is synthetic?: true

Number of methods: 2

Method #1
public static MethodParameterExamples$Colors[]
    MethodParameterExamples$Colors.values()
             Return type: class [LMethodParameterExamples$Colors;
     Generic return type: class [LMethodParameterExamples$Colors;

Method #2
public static MethodParameterExamples$Colors
    MethodParameterExamples$Colors.valueOf(java.lang.String)
             Return type: class MethodParameterExamples$Colors
     Generic return type: class MethodParameterExamples$Colors
         Parameter class: class java.lang.String
          Parameter name: name
               Modifiers: 32768
            Is implicit?: true
        Is name present?: true
           Is synthetic?: false
```

有关隐式声明的构造的更多信息，请参考[Java 语言规范](https://docs.oracle.com/javase/specs/)，包括在反射 API 中出现为隐式的参数。
