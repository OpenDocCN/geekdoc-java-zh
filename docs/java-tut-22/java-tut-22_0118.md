# 原始类型

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/rawTypes.html`](https://docs.oracle.com/javase/tutorial/java/generics/rawTypes.html)

*原始类型*是没有任何类型参数的泛型类或接口的名称。例如，给定泛型`Box`类：

```java
public class Box<T> {
    public void set(T t) { /* ... */ }
    // ...
}

```

要创建`Box<T>`的参数化类型，您需要为形式类型参数`T`提供实际类型参数：

```java
Box<Integer> intBox = new Box<>();

```

如果省略实际类型参数，则创建`Box<T>`的原始类型：

```java
Box rawBox = new Box();

```

因此，`Box`是泛型类型`Box<T>`的原始类型。但是，非泛型类或接口类型*不*是原始类型。

在旧代码中会出现原始类型，因为在 JDK 5.0 之前，许多 API 类（如`Collections`类）都不是泛型的。使用原始类型时，实际上获得的是泛型之前的行为 —— `Box`会给您`Object`。为了向后兼容，允许将参数化类型分配给其原始类型：

```java
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;               // OK

```

但是，如果将原始类型赋给参数化类型，则会收到警告：

```java
Box rawBox = new Box();           // rawBox is a raw type of Box<T>
Box<Integer> intBox = rawBox;     // warning: unchecked conversion

```

如果使用原始类型调用相应泛型类型中定义的泛型方法，也会收到警告：

```java
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;
rawBox.set(8);  // warning: unchecked invocation to set(T)

```

此警告显示原始类型绕过了泛型类型检查，将不安全代码的捕获推迟到运行时。因此，应避免使用原始类型。

类型擦除部分提供了有关 Java 编译器如何使用原始类型的更多信息。

## 未经检查的错误消息

如前所述，在将旧代码与泛型代码混合使用时，可能会遇到类似以下的警告消息：

```java
Note: Example.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.

```

当使用在原始类型上操作的旧 API 时，可能会出现以下示例中所示的情况：

```java
public class WarningDemo {
    public static void main(String[] args){
        Box<Integer> bi;
        bi = createBox();
    }

    static Box createBox(){
        return new Box();
    }
}

```

“未经检查”一词表示编译器没有足够的类型信息来执行确保类型安全所需的所有类型检查。默认情况下，“未经检查”警告是禁用的，尽管编译器会给出提示。要查看所有“未经检查”警告，请使用`-Xlint:unchecked`重新编译。

使用`-Xlint:unchecked`重新编译前面的示例，会显示以下额外信息：

```java
WarningDemo.java:4: warning: [unchecked] unchecked conversion
found   : Box
required: Box<java.lang.Integer>
        bi = createBox();
                      ^
1 warning

```

要完全禁用未经检查的警告，请使用`-Xlint:-unchecked`标志。`@SuppressWarnings("unchecked")`注解可以抑制未经检查的警告。如果您不熟悉`@SuppressWarnings`语法，请参阅注解。
