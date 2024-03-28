# 定义不可变对象的策略

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html)

以下规则定义了创建不可变对象的简单策略。并非所有被记录为“不可变”的类都遵循这些规则。这并不一定意味着这些类的创建者粗心大意  他们可能有充分的理由相信他们的类的实例在构造后永远不会改变。然而，这种策略需要复杂的分析，不适合初学者。

1.  不提供“setter”方法  修改字段或字段引用的对象的方法。

1.  使所有字段都是`final`和`private`。

1.  不允许子类重写方法。这样做的最简单方法是将类声明为`final`。更复杂的方法是将构造函数设为`private`，并在工厂方法中构造实例。

1.  如果实例字段包括对可变对象的引用，请不要允许更改这些对象：

    +   不要提供修改可变对象的方法。

    +   不共享对可变对象的引用。永远不要存储传递给构造函数的外部可变对象的引用；如果必要，创建副本，并存储对副本的引用。类似地，在必要时创建内部可变对象的副本，以避免在方法中返回原始对象。

将这种策略应用于`SynchronizedRGB`会产生以下步骤：

1.  这个类中有两个 setter 方法。第一个`set`方法任意地转换对象，并且在类的不可变版本中没有位置。第二个`invert`方法可以通过创建一个新对象来适应，而不是修改现有对象。

1.  所有字段已经是`private`；它们进一步被标记为`final`。

1.  类本身被声明为`final`。

1.  只有一个字段引用一个对象，而该对象本身是不可变的。因此，不需要防止改变“包含”可变对象状态的保护措施。

在这些更改之后，我们有了``ImmutableRGB``：

```java

final public class ImmutableRGB {

    // Values must be between 0 and 255.
    final private int red;
    final private int green;
    final private int blue;
    final private String name;

    private void check(int red,
                       int green,
                       int blue) {
        if (red < 0 || red > 255
            || green < 0 || green > 255
            || blue < 0 || blue > 255) {
            throw new IllegalArgumentException();
        }
    }

    public ImmutableRGB(int red,
                        int green,
                        int blue,
                        String name) {
        check(red, green, blue);
        this.red = red;
        this.green = green;
        this.blue = blue;
        this.name = name;
    }

    public int getRGB() {
        return ((red << 16) | (green << 8) | blue);
    }

    public String getName() {
        return name;
    }

    public ImmutableRGB invert() {
        return new ImmutableRGB(255 - red,
                       255 - green,
                       255 - blue,
                       "Inverse of " + name);
    }
}

```
