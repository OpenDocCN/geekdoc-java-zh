# 超出基本算术

> 原文：[`docs.oracle.com/javase/tutorial/java/data/beyondmath.html`](https://docs.oracle.com/javase/tutorial/java/data/beyondmath.html)

Java 编程语言支持基本算术运算，使用算术运算符：+，-，*，/和%。`java.lang`包中的[`Math`](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html)类提供了更高级数学计算的方法和常量。

`Math`类中的方法都是静态的，因此您可以直接从类中调用它们，如下所示：

```java
Math.cos(angle);

```

* * *

**注意：** 使用`static import`语言特性，您不必在每个数学函数前面写`Math`：

```java
import static java.lang.Math.*;

```

这使您可以通过简单名称调用`Math`类的方法。例如：

```java
cos(angle);

```

* * *

## 常量和基本方法

`Math`类包括两个常量：

+   `Math.E`，即自然对数的底数，以及

+   `Math.PI`，即圆周与直径的比值。

`Math`类还包括 40 多个静态方法。以下表格列出了一些基本方法。

基本数学方法

| 方法 | 描述 |
| --- | --- |

| `double abs(double d) float abs(float f)

int abs(int i)

`long abs(long lng)` | 返回参数的绝对值。 |

| `double ceil(double d)` | 返回大于或等于参数的最小整数。返回为 double 类型。 |
| --- | --- |
| `double floor(double d)` | 返回小于或等于参数的最大整数。返回为 double 类型。 |
| `double rint(double d)` | 返回与参数最接近的整数值。返回为 double 类型。 |
| `long round(double d) int round(float f)` | 返回最接近参数的 long 或 int，如方法的返回类型所示。 |

| `double min(double arg1, double arg2) float min(float arg1, float arg2)

int min(int arg1, int arg2)

`long min(long arg1, long arg2)` | 返回两个参数中较小的值。 |

| `double max(double arg1, double arg2) float max(float arg1, float arg2)

int max(int arg1, int arg2)

`long max(long arg1, long arg2)` | 返回两个参数中较大的值。 |

以下程序，`BasicMathDemo`，演示了如何使用其中一些方法：

```java

public class BasicMathDemo {
    public static void main(String[] args) {
        double a = -191.635;
        double b = 43.74;
        int c = 16, d = 45;

        System.out.printf("The absolute value " + "of %.3f is %.3f%n", 
                          a, Math.abs(a));

        System.out.printf("The ceiling of " + "%.2f is %.0f%n", 
                          b, Math.ceil(b));

        System.out.printf("The floor of " + "%.2f is %.0f%n", 
                          b, Math.floor(b));

        System.out.printf("The rint of %.2f " + "is %.0f%n", 
                          b, Math.rint(b));

        System.out.printf("The max of %d and " + "%d is %d%n",
                          c, d, Math.max(c, d));

        System.out.printf("The min of of %d " + "and %d is %d%n",
                          c, d, Math.min(c, d));
    }
}

```

这个程序的输出如下：

```java
The absolute value of -191.635 is 191.635
The ceiling of 43.74 is 44
The floor of 43.74 is 43
The rint of 43.74 is 44
The max of 16 and 45 is 45
The min of 16 and 45 is 16

```

## 指数和对数方法

下表列出了`Math`类的指数和对数方法。

指数和对数方法

| 方法 | 描述 |
| --- | --- |
| `double exp(double d)` | 返回自然对数的底数 e 的参数次幂。 |
| `double log(double d)` | 返回参数的自然对数。 |
| `double pow(double base, double exponent)` | 返回第一个参数的值的第二个参数的幂。 |
| `double sqrt(double d)` | 返回参数的平方根。 |

下面的程序，`指数演示`，显示了`e`的值，然后对任意选择的数字调用前面表中列出的每个方法：

```java

public class ExponentialDemo {
    public static void main(String[] args) {
        double x = 11.635;
        double y = 2.76;

        System.out.printf("The value of " + "e is %.4f%n",
                          Math.E);

        System.out.printf("exp(%.3f) " + "is %.3f%n",
                          x, Math.exp(x));

        System.out.printf("log(%.3f) is " + "%.3f%n",
                          x, Math.log(x));

        System.out.printf("pow(%.3f, %.3f) " + "is %.3f%n",
                          x, y, Math.pow(x, y));

        System.out.printf("sqrt(%.3f) is " + "%.3f%n",
                          x, Math.sqrt(x));
    }
}

```

当你运行`指数演示`时，你将看到以下输出：

```java
The value of e is 2.7183
exp(11.635) is 112983.831
log(11.635) is 2.454
pow(11.635, 2.760) is 874.008
sqrt(11.635) is 3.411

```

## 三角函数方法

`Math`类还提供了一系列三角函数，总结在下表中。传递给这些方法的值是以弧度表示的角度。你可以使用`toRadians`方法将角度转换为弧度。

三角函数方法

| 方法 | 描述 |
| --- | --- |
| `double sin(double d)` | 返回指定双精度值的正弦值。 |
| `double cos(double d)` | 返回指定双精度值的余弦值。 |
| `double tan(double d)` | 返回指定双精度值的正切值。 |
| `double asin(double d)` | 返回指定双精度值的反正弦值。 |
| `double acos(double d)` | 返回指定双精度值的反余弦值。 |
| `double atan(double d)` | 返回指定双精度值的反正切值。 |
| `double atan2(double y, double x)` | 将直角坐标`(x, y)`转换为极坐标`(r, theta)`并返回`theta`。 |
| `double toDegrees(double d) double toRadians(double d)` | 将参数转换为度或弧度。 |

这里有一个程序，`三角函数演示`，使用每个方法来计算 45 度角的各种三角函数值：

```java

public class TrigonometricDemo {
    public static void main(String[] args) {
        double degrees = 45.0;
        double radians = Math.toRadians(degrees);

        System.out.format("The value of pi " + "is %.4f%n",
                           Math.PI);

        System.out.format("The sine of %.1f " + "degrees is %.4f%n",
                          degrees, Math.sin(radians));

        System.out.format("The cosine of %.1f " + "degrees is %.4f%n",
                          degrees, Math.cos(radians));

        System.out.format("The tangent of %.1f " + "degrees is %.4f%n",
                          degrees, Math.tan(radians));

        System.out.format("The arcsine of %.4f " + "is %.4f degrees %n", 
                          Math.sin(radians), 
                          Math.toDegrees(Math.asin(Math.sin(radians))));

        System.out.format("The arccosine of %.4f " + "is %.4f degrees %n", 
                          Math.cos(radians),  
                          Math.toDegrees(Math.acos(Math.cos(radians))));

        System.out.format("The arctangent of %.4f " + "is %.4f degrees %n", 
                          Math.tan(radians), 
                          Math.toDegrees(Math.atan(Math.tan(radians))));
    }
}

```

该程序的输出如下：

```java
The value of pi is 3.1416
The sine of 45.0 degrees is 0.7071
The cosine of 45.0 degrees is 0.7071
The tangent of 45.0 degrees is 1.0000
The arcsine of 0.7071 is 45.0000 degrees
The arccosine of 0.7071 is 45.0000 degrees
The arctangent of 1.0000 is 45.0000 degrees

```

## 随机数

`random()`方法返回一个在 0.0 和 1.0 之间伪随机选择的数字。范围包括 0.0 但不包括 1.0。换句话说：`0.0 <= Math.random() < 1.0`。要获得不同范围的数字，可以对随机方法返回的值进行算术运算。例如，要生成 0 到 9 之间的整数，你可以这样写：

```java
int number = (int)(Math.random() * 10);

```

通过将值乘以 10，可能值的范围变为`0.0 <= number < 10.0`。

使用`Math.random`在需要生成单个随机数时效果很好。如果需要生成一系列随机数，应该创建一个`java.util.Random`的实例，并在该对象上调用方法来生成数字。
