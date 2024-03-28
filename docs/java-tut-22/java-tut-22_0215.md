# 一个同步类的示例

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/syncrgb.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/syncrgb.html)

这个类，``SynchronizedRGB``，定义了代表颜色的对象。每个对象将颜色表示为三个代表主要颜色值的整数和一个给出颜色名称的字符串。

```java

public class SynchronizedRGB {

    // Values must be between 0 and 255.
    private int red;
    private int green;
    private int blue;
    private String name;

    private void check(int red,
                       int green,
                       int blue) {
        if (red < 0 || red > 255
            || green < 0 || green > 255
            || blue < 0 || blue > 255) {
            throw new IllegalArgumentException();
        }
    }

    public SynchronizedRGB(int red,
                           int green,
                           int blue,
                           String name) {
        check(red, green, blue);
        this.red = red;
        this.green = green;
        this.blue = blue;
        this.name = name;
    }

    public void set(int red,
                    int green,
                    int blue,
                    String name) {
        check(red, green, blue);
        synchronized (this) {
            this.red = red;
            this.green = green;
            this.blue = blue;
            this.name = name;
        }
    }

    public synchronized int getRGB() {
        return ((red << 16) | (green << 8) | blue);
    }

    public synchronized String getName() {
        return name;
    }

    public synchronized void invert() {
        red = 255 - red;
        green = 255 - green;
        blue = 255 - blue;
        name = "Inverse of " + name;
    }
}

```

必须小心使用`SynchronizedRGB`，以避免出现不一致的状态。例如，假设一个线程执行以下代码：

```java
SynchronizedRGB color =
    new SynchronizedRGB(0, 0, 0, "Pitch Black");
...
int myColorInt = color.getRGB();      //Statement 1
String myColorName = color.getName(); //Statement 2

```

如果另一个线程在语句 1 之后但在语句 2 之前调用`color.set`，`myColorInt`的值将不匹配`myColorName`的值。为了避免这种结果，这两个语句必须绑定在一起：

```java
synchronized (color) {
    int myColorInt = color.getRGB();
    String myColorName = color.getName();
} 

```

这种不一致性只对可变对象有效  对于不可变版本的`SynchronizedRGB`不会有问题。
