# 有界类型参数

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/bounded.html`](https://docs.oracle.com/javase/tutorial/java/generics/bounded.html)

有时候你可能想要限制可以用作参数化类型中类型参数的类型。例如，一个操作数字的方法可能只想接受`Number`或其子类的实例。这就是*有界类型参数*的用途。

要声明一个有界类型参数，列出类型参数的名称，后跟`extends`关键字，后跟其*上界*，在这个例子中是`Number`。请注意，在这个上下文中，`extends`的意思是"扩展"（如类）或"实现"（如接口）。

```java
public class Box<T> {

    private T t;          

    public void set(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }

    public <U extends Number> void inspect(U u){
        System.out.println("T: " + t.getClass().getName());
        System.out.println("U: " + u.getClass().getName());
    }

    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<Integer>();
        integerBox.set(new Integer(10));
        integerBox.inspect("some text"); // error: this is still String!
    }
}

```

通过修改我们的通用方法以包含这个有界类型参数，编译现在会失败，因为我们对`inspect`的调用仍然包括一个`String`：

```java
Box.java:21: <U>inspect(U) in Box<java.lang.Integer> cannot
  be applied to (java.lang.String)
                        integerBox.inspect("10");
                                  ^
1 error

```

除了限制你可以用来实例化泛型类型的类型之外，有界类型参数还允许你调用边界中定义的方法：

```java
public class NaturalNumber<T extends Integer> {

    private T n;

    public NaturalNumber(T n)  { this.n = n; }

    public boolean isEven() {
        return n.intValue() % 2 == 0;
    }

    // ...
}

```

`isEven`方法通过`n`调用了`Integer`类中定义的`intValue`方法。

## 多个边界

前面的例子说明了使用具有单个边界的类型参数，但是类型参数可以有*多个边界*：

```java
<T extends B1 & B2 & B3>

```

具有多个边界的类型变量是边界中列出的所有类型的子类型。如果边界中有一个类，它必须首先指定。例如：

```java
Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }

```

如果边界`A`没有首先指定，你会得到一个编译时错误：

```java
class D <T extends B & A & C> { /* ... */ }  // compile-time error

```
