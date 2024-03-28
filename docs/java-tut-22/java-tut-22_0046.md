# 声明成员变量

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/variables.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/variables.html)

有几种类型的变量：

+   类中的成员变量这些被称为*字段*。

+   方法或代码块中的变量这些被称为*局部变量*。

+   方法声明中的变量这些被称为*参数*。

`Bicycle` 类使用以下代码行来定义其字段：

```java
public int cadence;
public int gear;
public int speed;

```

字段声明由三个组件组成，顺序为：

1.  零个或多个修饰符，比如`public`或`private`。

1.  字段的类型。

1.  字段的名称。

`Bicycle` 的字段名为`cadence`、`gear`和`speed`，都是整数类型（`int`）。`public`关键字将这些字段标识为公共成员，可被任何可以访问该类的对象访问。

## 访问修饰符

使用的第一个（最左边的）修饰符让你控制其他类对成员字段的访问权限。目前，只考虑`public`和`private`。其他访问修饰符将在后面讨论。

+   `public`修饰符该字段可从所有类访问。

+   `private`修饰符该字段只能在其自身类中访问。

符合封装原则，通常会将字段设为私有。这意味着它们只能从`Bicycle`类*直接*访问。然而，我们仍然需要访问这些值。这可以通过添加获取字段值的公共方法*间接*完成：

```java
public class Bicycle {

    private int cadence;
    private int gear;
    private int speed;

    public Bicycle(int startCadence, int startSpeed, int startGear) {
        gear = startGear;
        cadence = startCadence;
        speed = startSpeed;
    }

    public int getCadence() {
        return cadence;
    }

    public void setCadence(int newValue) {
        cadence = newValue;
    }

    public int getGear() {
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

## 类型

所有变量必须有一个类型。你可以使用原始类型如`int`、`float`、`boolean`等。或者你可以使用引用类型，比如字符串、数组或对象。

## 变量名

所有变量，无论是字段、局部变量还是参数，都遵循在语言基础课程中介绍的相同的命名规则和约定，变量命名。

在这节课中，请注意方法和类名使用相同的命名规则和约定，除了

+   类名的第一个字母应大写，而

+   方法名的第一个（或唯一）单词应该是动词。
