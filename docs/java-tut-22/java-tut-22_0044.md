# 类

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/classes.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/classes.html)

在标题为面向对象编程概念的课程中，介绍了面向对象概念，以自行车类为例，其中赛车、山地车和双人车为子类。以下是一个可能实现`Bicycle`类的示例代码，让你了解类声明的概述。本课程的后续部分将逐步支持和解释类声明。暂时不要担心细节。

```java
public class Bicycle {

    // the Bicycle class has
    // three *fields*
    public int cadence;
    public int gear;
    public int speed;

    // the Bicycle class has
    // one *constructor*
    public Bicycle(int startCadence, int startSpeed, int startGear) {
        gear = startGear;
        cadence = startCadence;
        speed = startSpeed;
    }

    // the Bicycle class has
    // four *methods*
    public void setCadence(int newValue) {
        cadence = newValue;
    }

    public void setGear(int newValue) {
        gear = newValue;
    }

    public void applyBrake(int decrement) {
        speed -= decrement;
    }

    public void speedUp(int increment) {
        speed += increment;
    }

}

```

`MountainBike`类的类声明，作为`Bicycle`的子类，可能如下所示：

```java
public class MountainBike extends Bicycle {

    // the MountainBike subclass has
    // one *field*
    public int seatHeight;

    // the MountainBike subclass has
    // one *constructor*
    public MountainBike(int startHeight, int startCadence,
                        int startSpeed, int startGear) {
        super(startCadence, startSpeed, startGear);
        seatHeight = startHeight;
    }   

    // the MountainBike subclass has
    // one *method*
    public void setHeight(int newValue) {
        seatHeight = newValue;
    }   

}

```

`MountainBike`继承了`Bicycle`的所有字段和方法，并添加了`seatHeight`字段以及一个设置它的方法（山地车的座位可以根据地形要求上下移动）。
