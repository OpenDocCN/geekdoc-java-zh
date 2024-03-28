# 枚举类型

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/enum.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)

*枚举类型*是一种特殊的数据类型，允许变量成为一组预定义的常量之一。变量必须等于为其预定义的值之一。常见示例包括罗盘方向（NORTH、SOUTH、EAST 和 WEST 的值）和一周的天数。

由于它们是常量，枚举类型字段的名称必须是大写字母。

在 Java 编程语言中，您可以使用`enum`关键字定义枚举类型。例如，您可以指定一个星期几的枚举类型如下：

```java

public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY,
    THURSDAY, FRIDAY, SATURDAY 
}

```

每当需要表示一组固定常量时，都应该使用枚举类型。这包括自然枚举类型，如我们太阳系中的行星和在编译时知道所有可能值的数据集，例如菜单上的选项、命令行标志等。

这里是一些代码，向您展示如何使用上面定义的`Day`枚举：

```java

public class EnumTest {
    Day day;

    public EnumTest(Day day) {
        this.day = day;
    }

    public void tellItLikeItIs() {
        switch (day) {
            case MONDAY:
                System.out.println("Mondays are bad.");
                break;

            case FRIDAY:
                System.out.println("Fridays are better.");
                break;

            case SATURDAY: case SUNDAY:
                System.out.println("Weekends are best.");
                break;

            default:
                System.out.println("Midweek days are so-so.");
                break;
        }
    }

    public static void main(String[] args) {
        EnumTest firstDay = new EnumTest(Day.MONDAY);
        firstDay.tellItLikeItIs();
        EnumTest thirdDay = new EnumTest(Day.WEDNESDAY);
        thirdDay.tellItLikeItIs();
        EnumTest fifthDay = new EnumTest(Day.FRIDAY);
        fifthDay.tellItLikeItIs();
        EnumTest sixthDay = new EnumTest(Day.SATURDAY);
        sixthDay.tellItLikeItIs();
        EnumTest seventhDay = new EnumTest(Day.SUNDAY);
        seventhDay.tellItLikeItIs();
    }
}

```

输出为：

```java
Mondays are bad.
Midweek days are so-so.
Fridays are better.
Weekends are best.
Weekends are best.

```

Java 编程语言的枚举类型比其他语言中的对应类型更强大。`enum`声明定义了一个*类*（称为*枚举类型*）。枚举类体可以包括方法和其他字段。编译器在创建枚举时会自动添加一些特殊方法。例如，它们具有一个静态`values`方法，返回一个包含枚举值的数组，按照它们声明的顺序排列。此方法通常与 for-each 结构结合使用，以遍历枚举类型的值。例如，下面`Planet`类示例中的代码遍历太阳系中的所有行星。

```java
for (Planet p : Planet.values()) {
    System.out.printf("Your weight on %s is %f%n",
                      p, p.surfaceWeight(mass));
}

```

* * *

**注意：** *所有*枚举隐式扩展`java.lang.Enum`。因为一个类只能扩展一个父类（参见声明类），Java 语言不支持状态的多重继承（参见状态、实现和类型的多重继承），因此枚举不能扩展其他任何内容。

* * *

在下面的示例中，`Planet`是一个表示太阳系行星的枚举类型。它们定义了常量质量和半径属性。

每个枚举常量都声明了质量和半径参数的值。这些值在创建常量时传递给构造函数。Java 要求常量在任何字段或方法之前定义。此外，当存在字段和方法时，枚举常量列表必须以分号结尾。

* * *

**注意：** 枚举类型的构造函数必须是包私有或私有访问。它会自动创建在枚举体开头定义的常量。您不能自己调用枚举构造函数。

* * *

除了其属性和构造函数外，`Planet` 还有一些方法，可以让你获取每个行星上物体的表面重力和重量。以下是一个示例程序，它接受你在地球上的体重（以任何单位）并计算并打印出你在所有行星上的体重（以相同单位）：

```java

public enum Planet {
    MERCURY (3.303e+23, 2.4397e6),
    VENUS   (4.869e+24, 6.0518e6),
    EARTH   (5.976e+24, 6.37814e6),
    MARS    (6.421e+23, 3.3972e6),
    JUPITER (1.9e+27,   7.1492e7),
    SATURN  (5.688e+26, 6.0268e7),
    URANUS  (8.686e+25, 2.5559e7),
    NEPTUNE (1.024e+26, 2.4746e7);

    private final double mass;   // in kilograms
    private final double radius; // in meters
    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }
    private double mass() { return mass; }
    private double radius() { return radius; }

    // universal gravitational constant  (m3 kg-1 s-2)
    public static final double G = 6.67300E-11;

    double surfaceGravity() {
        return G * mass / (radius * radius);
    }
    double surfaceWeight(double otherMass) {
        return otherMass * surfaceGravity();
    }
    public static void main(String[] args) {
        if (args.length != 1) {
            System.err.println("Usage: java Planet <earth_weight>");
            System.exit(-1);
        }
        double earthWeight = Double.parseDouble(args[0]);
        double mass = earthWeight/EARTH.surfaceGravity();
        for (Planet p : Planet.values())
           System.out.printf("Your weight on %s is %f%n",
                             p, p.surfaceWeight(mass));
    }
}

```

如果你在命令行中运行 `Planet.class` 并带上参数 175，你会得到以下输出：

```java
$ java Planet 175
Your weight on MERCURY is 66.107583
Your weight on VENUS is 158.374842
Your weight on EARTH is 175.000000
Your weight on MARS is 66.279007
Your weight on JUPITER is 442.847567
Your weight on SATURN is 186.552719
Your weight on URANUS is 158.397260
Your weight on NEPTUNE is 199.207413

```
