# 故障排除

> 原文：[`docs.oracle.com/javase/tutorial/reflect/special/arrayTrouble.html`](https://docs.oracle.com/javase/tutorial/reflect/special/arrayTrouble.html)

以下示例展示了在操作数组时可能发生的典型错误。

## 由于不可转换的类型导致`IllegalArgumentException`

``ArrayTroubleAgain``示例将生成一个[`IllegalArgumentException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html)。调用`Array.setInt()`来设置一个`Integer`类型的组件，其值为基本类型`int`。在非反射等效的`ary[0] = 1`中，编译器会将值`1`转换（或*装箱*）为引用类型`new Integer(1)`，以便其类型检查接受该语句。在使用反射时，类型检查仅在运行时发生，因此没有机会将值装箱。

```java

import java.lang.reflect.Array;
import static java.lang.System.err;

public class ArrayTroubleAgain {
    public static void main(String... args) {
	Integer[] ary = new Integer[2];
	try {
	    Array.setInt(ary, 0, 1);  // IllegalArgumentException

        // production code should handle these exceptions more gracefully
	} catch (IllegalArgumentException x) {
	    err.format("Unable to box%n");
	} catch (ArrayIndexOutOfBoundsException x) {
	    x.printStackTrace();
	}
    }
}

```

```java
$ *java ArrayTroubleAgain*
Unable to box

```

要消除此异常，有问题的行应该被以下调用替换[`Array.set(Object array, int index, Object value)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#set-java.lang.Object-int-java.lang.Object-)：

```java
Array.set(ary, 0, new Integer(1));

```

* * *

**提示：** 当使用反射设置或获取数组组件时，编译器无法执行装箱。它只能转换与[`Class.isAssignableFrom()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isAssignableFrom-java.lang.Class-)规范描述的相关类型。该示例预计会失败，因为`isAssignableFrom()`在此测试中将返回`false`，可以用程序验证特定转换是否可能：

```java
Integer.class.isAssignableFrom(int.class) == false 

```

同样，在反射中从基本类型到引用类型的自动转换也是不可能的。

```java
int.class.isAssignableFrom(Integer.class) == false

```

* * *

## 对空数组的`ArrayIndexOutOfBoundsException`

``ArrayTrouble``示例说明了如果尝试访问长度为零的数组元素将会发生的错误：

```java

import java.lang.reflect.Array;
import static java.lang.System.out;

public class ArrayTrouble {
    public static void main(String... args) {
        Object o = Array.newInstance(int.class, 0);
        int[] i = (int[])o;
        int[] j = new int[0];
        out.format("i.length = %d, j.length = %d, args.length = %d%n",
                   i.length, j.length, args.length);
        Array.getInt(o, 0);  // ArrayIndexOutOfBoundsException
    }
}

```

```java
$ *java ArrayTrouble*
i.length = 0, j.length = 0, args.length = 0
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException
        at java.lang.reflect.Array.getInt(Native Method)
        at ArrayTrouble.main(ArrayTrouble.java:11)

```

* * *

**提示：** 可以有没有元素的数组（空数组）。在常见代码中只有少数情况下会看到它们，但它们可能会在反射中无意中出现。当然，无法设置/获取空数组的值，因为会抛出[`ArrayIndexOutOfBoundsException`](https://docs.oracle.com/javase/8/docs/api/java/lang/ArrayIndexOutOfBoundsException.html)。

* * *

## 如果尝试缩小范围会导致`IllegalArgumentException`

``ArrayTroubleToo``示例包含的代码会失败，因为它尝试执行一个可能会丢失数据的操作：

```java

import java.lang.reflect.Array;
import static java.lang.System.out;

public class ArrayTroubleToo {
    public static void main(String... args) {
        Object o = new int[2];
        Array.setShort(o, 0, (short)2);  // widening, succeeds
        Array.setLong(o, 1, 2L);         // narrowing, fails
    }
}

```

```java
$ *java ArrayTroubleToo*
Exception in thread "main" java.lang.IllegalArgumentException: argument type
  mismatch
        at java.lang.reflect.Array.setLong(Native Method)
        at ArrayTroubleToo.main(ArrayTroubleToo.java:9)

```

* * *

**提示：** `Array.set*()` 和 `Array.get*()` 方法将执行自动扩展转换，但如果尝试进行缩小转换，则会抛出 [`IllegalArgumentException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html)。有关扩展和缩小转换的完整讨论，请参阅[*Java 语言规范，Java SE 7 版*](https://docs.oracle.com/javase/specs/jls/se7/html/index.html)，分别查看[Widening Primitive Conversion](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.1.2)和[Narrowing Primitive Conversion](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.1.3)部分。

* * *
