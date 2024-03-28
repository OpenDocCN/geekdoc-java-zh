# 接口

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/createinterface.html`](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)

在软件工程中有许多情况下，不同组的程序员需要达成一致的“合同”，明确规定他们的软件如何交互。每个组都应该能够编写他们的代码，而不需要了解其他组的代码是如何编写的。一般来说，*接口*就是这样的合同。

例如，想象一个未来社会，在这个社会中，由计算机控制的机器人汽车在城市街道上运载乘客，没有人类操作员。汽车制造商编写软件（当然是 Java），操作汽车停止、启动、加速、左转等等。另一个工业团体，电子导航仪制造商，制造接收 GPS（全球定位系统）位置数据和交通状况无线传输的计算机系统，并利用这些信息驾驶汽车。

汽车制造商必须发布一个行业标准接口，详细说明可以调用哪些方法来使汽车移动（任何制造商的任何汽车）。导航制造商可以编写调用接口中描述的方法来命令汽车的软件。两个工业团体都不需要知道*对方的软件是如何实现的。事实上，每个团体都认为自己的软件是高度专有的，并保留随时修改的权利，只要它继续遵守已发布的接口。

## Java 中的接口

在 Java 编程语言中，*接口*是一种引用类型，类似于类，只能包含常量、方法签名、默认方法、静态方法和嵌套类型。方法体仅存在于默认方法和静态方法中。接口不能被实例化它们只能被类*实现*或其他接口*扩展*。扩展将在本课程的后面讨论。

定义接口类似于创建新类：

```java
public interface OperateCar {

   // constant declarations, if any

   // method signatures

   // An enum with values RIGHT, LEFT
   int turn(Direction direction,
            double radius,
            double startSpeed,
            double endSpeed);
   int changeLanes(Direction direction,
                   double startSpeed,
                   double endSpeed);
   int signalTurn(Direction direction,
                  boolean signalOn);
   int getRadarFront(double distanceToCar,
                     double speedOfCar);
   int getRadarRear(double distanceToCar,
                    double speedOfCar);
         ......
   // more method signatures
}

```

请注意，方法签名没有大括号，并以分号结尾。

要使用接口，您需要编写一个*实现*接口的类。当一个可实例化的类实现一个接口时，它为接口中声明的每个方法提供一个方法体。例如，

```java
public class OperateBMW760i implements OperateCar {

    // the OperateCar method signatures, with implementation --
    // for example:
    public int signalTurn(Direction direction, boolean signalOn) {
       // code to turn BMW's LEFT turn indicator lights on
       // code to turn BMW's LEFT turn indicator lights off
       // code to turn BMW's RIGHT turn indicator lights on
       // code to turn BMW's RIGHT turn indicator lights off
    }

    // other members, as needed -- for example, helper classes not 
    // visible to clients of the interface
}

```

在上面的机器人汽车示例中，将实现接口的是汽车制造商。雪佛兰的实现肯定与丰田的实现大不相同，但两家制造商都会遵守相同的接口。作为接口的客户，导航制造商将构建使用汽车位置的 GPS 数据、数字街道地图和交通数据来驾驶汽车的系统。在这样做的过程中，导航系统将调用接口方法：转向、变道、刹车、加速等等。

## 接口作为 API

机器人汽车示例展示了一个作为行业标准*应用程序编程接口（API）*使用的接口。API 在商业软件产品中也很常见。通常，一家公司销售一个包含复杂方法的软件包，另一家公司希望在自己的软件产品中使用这些方法。一个例子是数字图像处理方法包，这些方法被销售给制作最终用户图形程序的公司。图像处理公司编写其类来实现一个接口，然后将其公开给客户。图形公司然后使用接口中定义的签名和返回类型调用图像处理方法。虽然图像处理公司的 API 是公开的（给其客户），但其 API 的实现被保持为严格保密的秘密事实上，它可以在以后的某个日期修改实现，只要它继续实现客户依赖的原始接口。
