# 多态性

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html`](https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html)

*多态性*的词典定义指的是生物学中的一个原则，即一个生物体或物种可以具有许多不同的形式或阶段。这个原则也可以应用于面向对象编程和像 Java 语言这样的语言中。类的子类可以定义自己独特的行为，同时共享父类的一些功能。

多态性可以通过对`Bicycle`类进行微小修改来演示。例如，可以向类中添加一个`printDescription`方法，该方法显示实例中当前存储的所有数据。

```java
public void printDescription(){
    System.out.println("\nBike is " + "in gear " + this.gear
        + " with a cadence of " + this.cadence +
        " and travelling at a speed of " + this.speed + ". ");
}

```

要在 Java 语言中演示多态特性，请使用`MountainBike`和`RoadBike`类扩展`Bicycle`类。对于`MountainBike`，添加一个`suspension`字段，这是一个`String`值，指示自行车是否有前减震器，`Front`。或者，自行车有前后减震器，`Dual`。

这是更新后的类：

```java
public class MountainBike extends Bicycle {
    private String suspension;

    public MountainBike(
               int startCadence,
               int startSpeed,
               int startGear,
               String suspensionType){
        super(startCadence,
              startSpeed,
              startGear);
        this.setSuspension(suspensionType);
    }

    public String getSuspension(){
      return this.suspension;
    }

    public void setSuspension(String suspensionType) {
        this.suspension = suspensionType;
    }

    public void printDescription() {
        super.printDescription();
        System.out.println("The " + "MountainBike has a" +
            getSuspension() + " suspension.");
    }
} 

```

请注意覆盖的`printDescription`方法。除了之前提供的信息外，输出还包括有关悬架的额外数据。

接下来，创建`RoadBike`类。因为公路或赛车自行车有细轮胎，所以添加一个属性来跟踪轮胎宽度。这是`RoadBike`类：

```java
public class RoadBike extends Bicycle{
    // In millimeters (mm)
    private int tireWidth;

    public RoadBike(int startCadence,
                    int startSpeed,
                    int startGear,
                    int newTireWidth){
        super(startCadence,
              startSpeed,
              startGear);
        this.setTireWidth(newTireWidth);
    }

    public int getTireWidth(){
      return this.tireWidth;
    }

    public void setTireWidth(int newTireWidth){
        this.tireWidth = newTireWidth;
    }

    public void printDescription(){
        super.printDescription();
        System.out.println("The RoadBike" + " has " + getTireWidth() +
            " MM tires.");
    }
}

```

请注意，`printDescription`方法再次被覆盖。这次，显示了有关轮胎宽度的信息。

总结一下，有三个类：`Bicycle`，`MountainBike`和`RoadBike`。这两个子类覆盖了`printDescription`方法并打印了独特的信息。

这是一个创建三个`Bicycle`变量的测试程序。每个变量分配给三个自行车类中的一个。然后打印每个变量。

```java
public class TestBikes {
  public static void main(String[] args){
    Bicycle bike01, bike02, bike03;

    bike01 = new Bicycle(20, 10, 1);
    bike02 = new MountainBike(20, 10, 5, "Dual");
    bike03 = new RoadBike(40, 20, 8, 23);

    bike01.printDescription();
    bike02.printDescription();
    bike03.printDescription();
  }
}

```

以下是测试程序的输出：

```java
Bike is in gear 1 with a cadence of 20 and travelling at a speed of 10\. 

Bike is in gear 5 with a cadence of 20 and travelling at a speed of 10\. 
The MountainBike has a Dual suspension.

Bike is in gear 8 with a cadence of 40 and travelling at a speed of 20\. 
The RoadBike has 23 MM tires.

```

Java 虚拟机（JVM）调用与每个变量引用的对象相对应的方法。它不调用变量类型定义的方法。这种行为称为*虚拟方法调用*，展示了 Java 语言中重要的多态性特性的一个方面。
