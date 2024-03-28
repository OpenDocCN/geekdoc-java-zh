# 什么是接口？

> 原文：[`docs.oracle.com/javase/tutorial/java/concepts/interface.html`](https://docs.oracle.com/javase/tutorial/java/concepts/interface.html)

正如你已经学到的，对象通过暴露的方法定义它们与外部世界的交互。方法构成了对象与外部世界的*接口*；例如，电视机前面的按钮是你和塑料外壳另一侧的电线之间的接口。你按下“电源”按钮来打开和关闭电视。

在最常见的形式中，一个接口是一组相关方法，这些方法没有具体实现。如果将自行车的行为规定为一个接口，可能如下所示：

```java
interface Bicycle {

    //  wheel revolutions per minute
    void changeCadence(int newValue);

    void changeGear(int newValue);

    void speedUp(int increment);

    void applyBrakes(int decrement);
}

```

要实现这个接口，你的类的名称会改变（例如，变成一个特定品牌的自行车，比如`ACMEBicycle`），并且在类声明中使用`implements`关键字：

```java
class ACMEBicycle implements Bicycle {

    int cadence = 0;
    int speed = 0;
    int gear = 1;

   // The compiler will now require that methods
   // changeCadence, changeGear, speedUp, and applyBrakes
   // all be implemented. Compilation will fail if those
   // methods are missing from this class.

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

实现一个接口允许一个类更加正式地承诺提供的行为。接口形成了类与外部世界之间的契约，这个契约在编译时由编译器强制执行。如果你的类声称实现了一个接口，那么该接口定义的所有方法必须在类的源代码中出现，类才能成功编译。

* * *

**注意：** 要实际编译`ACMEBicycle`类，你需要在实现的接口方法开头添加`public`关键字。你将在后面关于类和对象以及接口和继承的课程中了解这样做的原因。

* * *
