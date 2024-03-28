# 什么是类？

> 原文：[`docs.oracle.com/javase/tutorial/java/concepts/class.html`](https://docs.oracle.com/javase/tutorial/java/concepts/class.html)

在现实世界中，你经常会发现许多同类的个体对象。可能存在成千上万其他同款同型号的自行车。每辆自行车都是根据相同的蓝图构建的，因此包含相同的组件。在面向对象的术语中，我们说你的自行车是已知为自行车的对象类的*实例*。一个*类*是创建个体对象的蓝图。

以下的`Bicycle`类是自行车的一种可能实现：

```java

class Bicycle {

    int cadence = 0;
    int speed = 0;
    int gear = 1;

    void changeCadence(int newValue) {
         cadence = newValue;
    }

    void changeGear(int newValue) {
         gear = newValue;
    }

    void speedUp(int increment) {
         speed = speed + increment;   
    }

    void applyBrakes(int decrement) {
         speed = speed - decrement;
    }

    void printStates() {
         System.out.println("cadence:" +
             cadence + " speed:" + 
             speed + " gear:" + gear);
    }
}

```

Java 编程语言的语法对你来说可能是新的，但这个类的设计是基于之前讨论的自行车对象的。字段`cadence`、`speed`和`gear`代表对象的状态，而方法（`changeCadence`、`changeGear`、`speedUp`等）定义了它与外部世界的交互。

你可能已经注意到`Bicycle`类中没有包含`main`方法。那是因为它不是一个完整的应用程序；它只是可能在应用程序中*使用*的自行车的蓝图。创建和使用新的`Bicycle`对象的责任属于你的应用程序中的某个其他类。

这里有一个`BicycleDemo`类，它创建了两个独立的`Bicycle`对象并调用它们的方法：

```java

class BicycleDemo {
    public static void main(String[] args) {

        // Create two different 
        // Bicycle objects
        Bicycle bike1 = new Bicycle();
        Bicycle bike2 = new Bicycle();

        // Invoke methods on 
        // those objects
        bike1.changeCadence(50);
        bike1.speedUp(10);
        bike1.changeGear(2);
        bike1.printStates();

        bike2.changeCadence(50);
        bike2.speedUp(10);
        bike2.changeGear(2);
        bike2.changeCadence(40);
        bike2.speedUp(10);
        bike2.changeGear(3);
        bike2.printStates();
    }
}

```

这个测试的输出打印了两辆自行车的结束踏板节奏、速度和档位：

```java
cadence:50 speed:10 gear:2
cadence:40 speed:20 gear:3

```
