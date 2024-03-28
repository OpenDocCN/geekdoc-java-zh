# 获取和设置数组及其组件

> 原文：[`docs.oracle.com/javase/tutorial/reflect/special/arraySetGet.html`](https://docs.oracle.com/javase/tutorial/reflect/special/arraySetGet.html)

就像在非反射代码中一样，可以整体设置或逐个组件设置或检索数组字段。要一次设置整个数组，请使用[`java.lang.reflect.Field.set(Object obj, Object value)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#set-java.lang.Object-java.lang.Object-)。要检索整个数组，请使用[`Field.get(Object)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#get-java.lang.Object-)。可以使用[`java.lang.reflect.Array`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html)中的方法来设置或检索单个组件。

[`Array`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html)提供了形式为`set*Foo*()`和`get*Foo*()`的方法，用于设置和获取任何原始类型的组件。例如，可以使用[`Array.setInt(Object array, int index, int value)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#setInt-java.lang.Objectint-int-)设置`int`数组的组件，并可以使用[`Array.getInt(Object array, int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#getInt-java.lang.Object-int-)检索它。

这些方法支持自动*扩宽*数据类型。因此，[`Array.getShort()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#getShort-java.lang.Object-int-)可以用于设置`int`数组的值，因为一个 16 位的`short`可以被扩宽为 32 位的`int`而不会丢失数据；另一方面，在`int`数组上调用[`Array.setLong()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#setLong-java.lang.Object-int-long-)将导致抛出[`IllegalArgumentException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html)，因为 64 位的`long`不能被缩小为 32 位的`int`而不丢失信息。无论传递的实际值是否能够准确表示为目标数据类型，这都是正确的。[*Java 语言规范，Java SE 7 版*](https://docs.oracle.com/javase/specs/jls/se7/html/index.html)，章节[Widening Primitive Conversion](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.1.2)和[Narrowing Primitive Conversion](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.1.3)包含了对扩宽和缩窄转换的完整讨论。

引用类型数组（包括数组的数组）的组件使用[`Array.set(Object array, int index, int value)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#set-java.lang.Object-int-int-)和[`Array.get(Object array, int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#get-java.lang.Object-int-)进行设置和检索。

## 设置类型为数组的字段

``GrowBufferedReader``示例演示了如何替换类型为数组的字段的值。在这种情况下，代码将[`java.io.BufferedReader`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)的后备数组替换为更大的数组。（这假设原始`BufferedReader`的创建在不可修改的代码中；否则，可以简单地使用接受输入缓冲区大小的替代构造函数[`BufferedReader(java.io.Reader in, int size)`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html#BufferedReader-java.io.Reader-int-)。）

```java

import java.io.BufferedReader;
import java.io.CharArrayReader;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.lang.reflect.Field;
import java.util.Arrays;
import static java.lang.System.out;

public class GrowBufferedReader {
    private static final int srcBufSize = 10 * 1024;
    private static char[] src = new char[srcBufSize];
    static {
	src[srcBufSize - 1] = 'x';
    }
    private static CharArrayReader car = new CharArrayReader(src);

    public static void main(String... args) {
	try {
	    BufferedReader br = new BufferedReader(car);

	    Class<?> c = br.getClass();
	    Field f = c.getDeclaredField("cb");

	    // cb is a private field
	    f.setAccessible(true);
	    char[] cbVal = char[].class.cast(f.get(br));

	    char[] newVal = Arrays.copyOf(cbVal, cbVal.length * 2);
	    if (args.length > 0 && args[0].equals("grow"))
		f.set(br, newVal);

	    for (int i = 0; i < srcBufSize; i++)
		br.read();

	    // see if the new backing array is being used
	    if (newVal[srcBufSize - 1] == src[srcBufSize - 1])
		out.format("Using new backing array, size=%d%n", newVal.length);
	    else
		out.format("Using original backing array, size=%d%n", cbVal.length);

        // production code should handle these exceptions more gracefully
	} catch (FileNotFoundException x) {
	    x.printStackTrace();
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (IOException x) {
	    x.printStackTrace();
	}
    }
}

```

```java
$ *java GrowBufferedReader grow*
Using new backing array, size=16384
$ *java GrowBufferedReader*
Using original backing array, size=8192

```

请注意，上述示例使用了数组实用方法[`java.util.Arrays.copyOf)`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#copyOf-char:A-int-)。[`java.util.Arrays`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html)包含许多在操作数组时方便的方法。

## 访问多维数组的元素

多维数组简单来说就是嵌套数组。二维数组是数组的数组。三维数组是二维数组的数组，依此类推。``CreateMatrix``示例演示了如何使用反射创建和初始化多维数组。

```java

import java.lang.reflect.Array;
import static java.lang.System.out;

public class CreateMatrix {
    public static void main(String... args) {
        Object matrix = Array.newInstance(int.class, 2, 2);
        Object row0 = Array.get(matrix, 0);
        Object row1 = Array.get(matrix, 1);

        Array.setInt(row0, 0, 1);
        Array.setInt(row0, 1, 2);
        Array.setInt(row1, 0, 3);
        Array.setInt(row1, 1, 4);

        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                out.format("matrix[%d][%d] = %d%n", i, j, ((int[][])matrix)[i][j]);
    }
}

```

```java
$ *java CreateMatrix*
matrix[0][0] = 1
matrix[0][1] = 2
matrix[1][0] = 3
matrix[1][1] = 4

```

通过使用以下代码片段也可以获得相同的结果：

```java
Object matrix = Array.newInstance(int.class, 2);
Object row0 = Array.newInstance(int.class, 2);
Object row1 = Array.newInstance(int.class, 2);

Array.setInt(row0, 0, 1);
Array.setInt(row0, 1, 2);
Array.setInt(row1, 0, 3);
Array.setInt(row1, 1, 4);

Array.set(matrix, 0, row0);
Array.set(matrix, 1, row1);

```

可变参数[`Array.newInstance(Class<?> componentType, int... dimensions)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#newInstance-java.lang.Class-int...-)提供了一个方便的方式来创建多维数组，但组件仍然需要使用多维数组是嵌套数组的原则进行初始化。（反射不提供用于此目的的多个索引`get`/`set`方法。）
