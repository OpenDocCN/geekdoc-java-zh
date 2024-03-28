# 理解类成员

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/classvars.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html)

在本节中，我们讨论了使用`static`关键字来创建属于类而不是类实例的字段和方法。

## 类变量

当从同一类蓝图创建多个对象时，它们各自拥有自己独特的*实例变量*副本。在`Bicycle`类的情况下，实例变量是`cadence`、`gear`和`speed`。每个`Bicycle`对象都有自己的这些变量的值，存储在不同的内存位置。

有时，你希望拥有对所有对象都通用的变量。这可以通过`static`修饰符来实现。在声明中具有`static`修饰符的字段称为*静态字段*或*类变量*。它们与类关联，而不是与任何对象关联。类的每个实例共享一个类变量，它在内存中的一个固定位置。任何对象都可以更改类变量的值，但也可以在不创建类的实例的情况下操作类变量。

例如，假设你想创建多个`Bicycle`对象并为每个分配一个从 1 开始的序列号。这个 ID 号对每个对象是唯一的，因此是一个实例变量。同时，你需要一个字段来跟踪已创建多少`Bicycle`对象，以便知道要分配给下一个对象的 ID。这样的字段与任何单个对象无关，而是与整个类相关。为此，你需要一个类变量`numberOfBicycles`，如下所示：

```java
public class Bicycle {

    private int cadence;
    private int gear;
    private int speed;

    // add an instance variable for the object ID
    private int id;

    // add a class variable for the
    // number of Bicycle objects instantiated
    private static int numberOfBicycles = 0;
        ...
}

```

类变量通过类名本身引用，如

```java
Bicycle.numberOfBicycles

```

这样清楚地表明它们是类变量。

* * *

**注意：** 你也可以通过对象引用来引用静态字段

```java
myBike.numberOfBicycles

```

但这是不被推荐的，因为它没有清楚地表明它们是类变量。

* * *

你可以使用`Bicycle`构造函数来设置`id`实例变量并递增`numberOfBicycles`类变量：

```java
public class Bicycle {

    private int cadence;
    private int gear;
    private int speed;
    private int id;
    private static int numberOfBicycles = 0;

    public Bicycle(int startCadence, int startSpeed, int startGear){
        gear = startGear;
        cadence = startCadence;
        speed = startSpeed;

        // increment number of Bicycles
        // and assign ID number
        id = ++numberOfBicycles;
    }

    // new method to return the ID instance variable
    public int getID() {
        return id;
    }
        ...
}

```

## 类方法

Java 编程语言支持静态方法以及静态变量。具有`static`修饰符的静态方法应该使用类名调用，而无需创建类的实例，如

```java
ClassName.methodName(args)

```

* * *

**注意：** 你也可以通过对象引用来引用静态方法

```java
instanceName.methodName(args)

```

但这是不被推荐的，因为它没有清楚地表明它们是类方法。

* * *

静态方法的一个常见用途是访问静态字段。例如，我们可以向`Bicycle`类添加一个静态方法来访问`numberOfBicycles`静态字段：

```java
public static int getNumberOfBicycles() {
    return numberOfBicycles;
}

```

并非所有实例和类变量和方法的组合都被允许：

+   实例方法可以直接访问实例变量和实例方法。

+   实例方法可以直接访问类变量和类方法。

+   类方法可以直接访问类变量和类方法。

+   类方法**不能**直接访问实例变量或实例方法它们必须使用对象引用。此外，类方法不能使用`this`关键字，因为`this`没有实例可供参考。

## 常量

`static`修饰符与`final`修饰符结合使用，也用于定义常量。`final`修饰符表示此字段的值不能更改。

例如，以下变量声明定义了一个名为`PI`的常量，其值是圆周率的近似值（圆的周长与直径的比值）：

```java
static final double PI = 3.141592653589793;

```

以这种方式定义的常量不能被重新分配，如果程序尝试这样做，将在编译时出现错误。按照惯例，常量值的名称以大写字母拼写。如果名称由多个单词组成，则单词之间用下划线（_）分隔。

* * *

**注意：**如果原始类型或字符串被定义为常量，并且在编译时已知其值，则编译器会在代码中的所有位置用其值替换常量名称。这被称为*编译时常量*。如果常量在外部世界中的值发生变化（例如，如果立法规定 pi 实际上应该是 3.975），则需要重新编译使用此常量的任何类以获取当前值。

* * *

## `Bicycle`类

在本节中进行的所有修改后，`Bicycle`类现在是：

```java
public class Bicycle {

    private int cadence;
    private int gear;
    private int speed;

    private int id;

    private static int numberOfBicycles = 0;

    public Bicycle(int startCadence,
                   int startSpeed,
                   int startGear) {
        gear = startGear;
        cadence = startCadence;
        speed = startSpeed;

        id = ++numberOfBicycles;
    }

    public int getID() {
        return id;
    }

    public static int getNumberOfBicycles() {
        return numberOfBicycles;
    }

    public int getCadence() {
        return cadence;
    }

    public void setCadence(int newValue) {
        cadence = newValue;
    }

    public int getGear(){
        return gear;
    }

    public void setGear(int newValue) {
        gear = newValue;
    }

    public int getSpeed() {
        return speed;
    }

    public void applyBrake(int decrement) {
        speed -= decrement;
    }

    public void speedUp(int increment) {
        speed += increment;
    }
}

```
