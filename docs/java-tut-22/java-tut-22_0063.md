# 内部类示例

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/innerclasses.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/innerclasses.html)

要查看内部类的使用，请首先考虑一个数组。在以下示例中，您创建一个数组，填充它的整数值，然后仅按升序输出数组的偶数索引值。

接下来的`DataStructure.java`示例包括：

+   包含构造函数以创建包含连续整数值（0、1、2、3 等）的数组的实例的`DataStructure`外部类，并且包含一个打印具有偶数索引值的数组元素的方法。

+   `EvenIterator`内部类，实现了`DataStructureIterator`接口，该接口扩展了[`Iterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)`<`[`Integer`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html)`>`接口。迭代器用于遍历数据结构，通常具有用于测试最后一个元素、检索当前元素和移动到下一个元素的方法。

+   一个`main`方法，实例化一个`DataStructure`对象（`ds`），然后调用`printEven`方法来打印具有偶数索引值的数组`arrayOfInts`的元素。

```java

public class DataStructure {

    // Create an array
    private final static int SIZE = 15;
    private int[] arrayOfInts = new int[SIZE];

    public DataStructure() {
        // fill the array with ascending integer values
        for (int i = 0; i < SIZE; i++) {
            arrayOfInts[i] = i;
        }
    }

    public void printEven() {

        // Print out values of even indices of the array
        DataStructureIterator iterator = this.new EvenIterator();
        while (iterator.hasNext()) {
            System.out.print(iterator.next() + " ");
        }
        System.out.println();
    }

    interface DataStructureIterator extends java.util.Iterator<Integer> { } 

    // Inner class implements the DataStructureIterator interface,
    // which extends the Iterator<Integer> interface

    private class EvenIterator implements DataStructureIterator {

        // Start stepping through the array from the beginning
        private int nextIndex = 0;

        public boolean hasNext() {

            // Check if the current element is the last in the array
            return (nextIndex <= SIZE - 1);
        }        

        public Integer next() {

            // Record a value of an even index of the array
            Integer retValue = Integer.valueOf(arrayOfInts[nextIndex]);

            // Get the next even element
            nextIndex += 2;
            return retValue;
        }
    }

    public static void main(String s[]) {

        // Fill the array with integer values and print out only
        // values of even indices
        DataStructure ds = new DataStructure();
        ds.printEven();
    }
}

```

输出为：

```java
0 2 4 6 8 10 12 14 

```

请注意，`EvenIterator`类直接引用了`DataStructure`对象的`arrayOfInts`实例变量。

您可以使用内部类来实现辅助类，例如在此示例中所示的类。要处理用户界面事件，您必须知道如何使用内部类，因为事件处理机制广泛使用它们。

## 局部类和匿名类

有两种额外的内部类。您可以在方法体内声明一个内部类。这些类被称为局部类。您还可以在方法体内声明一个没有命名的内部类。这些类被称为匿名类。

## 修饰符

您可以为内部类使用与外部类的其他成员相同的修饰符。例如，您可以使用访问修饰符`private`、`public`和`protected`来限制对内部类的访问，就像您用它们来限制对其他类成员的访问一样。
