# 使用关键字 super

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/super.html`](https://docs.oracle.com/javase/tutorial/java/IandI/super.html)

## 访问超类成员

如果您的方法覆盖了其超类的方法之一，您可以通过关键字`super`调用被覆盖的方法。您还可以使用`super`来引用隐藏字段（尽管不建议隐藏字段）。考虑这个类，`Superclass`：

```java
public class Superclass {

    public void printMethod() {
        System.out.println("Printed in Superclass.");
    }
}

```

这里是一个名为`Subclass`的子类，覆盖了`printMethod()`：

```java
public class Subclass extends Superclass {

    // overrides printMethod in Superclass
    public void printMethod() {
        super.printMethod();
        System.out.println("Printed in Subclass");
    }
    public static void main(String[] args) {
        Subclass s = new Subclass();
        s.printMethod();    
    }
}

```

在`Subclass`中，简单名称`printMethod()`指的是在`Subclass`中声明的那个，它覆盖了`Superclass`中的那个。因此，要引用从`Superclass`继承的`printMethod()`，`Subclass`必须使用一个限定名称，使用`super`如所示。编译和执行`Subclass`将打印以下内容：

```java
Printed in Superclass.
Printed in Subclass

```

## 子类构造函数

以下示例说明了如何使用`super`关键字调用超类的构造函数。回想一下`Bicycle`示例中`MountainBike`是`Bicycle`的子类。这是`MountainBike`（子类）构造函数，它调用超类构造函数，然后添加自己的初始化代码：

```java
public MountainBike(int startHeight, 
                    int startCadence,
                    int startSpeed,
                    int startGear) {
    super(startCadence, startSpeed, startGear);
    seatHeight = startHeight;
}   

```

调用超类构造函数必须是子类构造函数中的第一行。

调用超类构造函数的语法是

```java
super();  

```

或者：

```java
super(parameter list);

```

使用`super()`时，将调用超类的无参数构造函数。使用`super(parameter list)`时，将调用具有匹配参数列表的超类构造函数。

* * *

**注意：**如果构造函数没有显式调用超类的构造函数，Java 编译器会自动插入对超类的无参数构造函数的调用。如果超类没有无参数构造函数，您将会得到一个编译时错误。`Object`确实有这样一个构造函数，所以如果`Object`是唯一的超类，就不会有问题。

* * *

如果子类构造函数显式或隐式地调用其超类的构造函数，您可能会认为会有一整个构造函数链被调用，一直回溯到`Object`的构造函数。事实上，情况确实如此。这被称为*构造函数链*，当存在长串的类继承时，您需要注意这一点。
