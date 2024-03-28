# 识别数组类型

> 原文：[`docs.oracle.com/javase/tutorial/reflect/special/arrayComponents.html`](https://docs.oracle.com/javase/tutorial/reflect/special/arrayComponents.html)

可以通过调用[`Class.isArray()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isArray--)来识别数组类型。要获取一个[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)，请使用本教程中检索类对象部分描述的方法之一。

``ArrayFind``示例标识了命名类中的数组类型字段，并报告了每个字段的组件类型。

```java

import java.lang.reflect.Field;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class ArrayFind {
    public static void main(String... args) {
	boolean found = false;
 	try {
	    Class<?> cls = Class.forName(args[0]);
	    Field[] flds = cls.getDeclaredFields();
	    for (Field f : flds) {
 		Class<?> c = f.getType();
		if (c.isArray()) {
		    found = true;
		    out.format("%s%n"
                               + "           Field: %s%n"
			       + "            Type: %s%n"
			       + "  Component Type: %s%n",
			       f, f.getName(), c, c.getComponentType());
		}
	    }
	    if (!found) {
		out.format("No array fields%n");
	    }

        // production code should handle this exception more gracefully
 	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

`Class.get*Type()`返回值的语法在[`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--)中有描述。类型名称开头的'`[`'字符的数量表示数组的维度（即嵌套的深度）。

输出示例如下。用户输入用斜体表示。一个原始类型为`byte`的数组：

```java
$*java ArrayFind java.nio.ByteBuffer*
final byte[] java.nio.ByteBuffer.hb
           Field: hb
            Type: class [B
  Component Type: byte

```

一个引用类型为[`StackTraceElement`](https://docs.oracle.com/javase/8/docs/api/java/lang/StackTraceElement.html)的数组：

```java
$ *java ArrayFind java.lang.Throwable*
private java.lang.StackTraceElement[] java.lang.Throwable.stackTrace
           Field: stackTrace
            Type: class [Ljava.lang.StackTraceElement;
  Component Type: class java.lang.StackTraceElement

```

`predefined`是一个引用类型的一维数组[`java.awt.Cursor`](https://docs.oracle.com/javase/8/docs/api/java/awt/Cursor.html)，而`cursorProperties`是一个引用类型的二维数组[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)：

```java
$ *java ArrayFind java.awt.Cursor*
protected static java.awt.Cursor[] java.awt.Cursor.predefined
           Field: predefined
            Type: class [Ljava.awt.Cursor;
  Component Type: class java.awt.Cursor
static final java.lang.String[][] java.awt.Cursor.cursorProperties
           Field: cursorProperties
            Type: class [[Ljava.lang.String;
  Component Type: class [Ljava.lang.String;

```
