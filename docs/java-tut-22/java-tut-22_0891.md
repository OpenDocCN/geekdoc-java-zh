# 发现类成员

> 原文：[`docs.oracle.com/javase/tutorial/reflect/class/classMembers.html`](https://docs.oracle.com/javase/tutorial/reflect/class/classMembers.html)

[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) 提供了两类方法来访问字段、方法和构造函数：列举这些成员的方法和搜索特定成员的方法。此外，还有用于访问直接在类上声明的成员的方法，以及搜索超接口和超类以查找继承成员的方法。以下表格总结了所有定位成员的方法及其特性。

定位字段的类方法

| [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) API | 成员列表？ | 继承成员？ | 私有成员？ |
| --- | --- | --- | --- |
| [`getDeclaredField()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredField-java.lang.String-) | 否 | 否 | 是 |
| [`getField()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getField-java.lang.String-) | 否 | 是 | 否 |
| [`getDeclaredFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredFields--) | 是 | 否 | 是 |
| [`getFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getFields--) | 是 | 是 | 否 |

定位方法的类方法

| [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) API | 成员列表？ | 继承成员？ | 私有成员？ |
| --- | --- | --- | --- |
| [`getDeclaredMethod()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredMethod-java.lang.String-java.lang.Class...-) | 否 | 否 | 是 |
| [`getMethod()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethod-java.lang.String-java.lang.Class...-) | 否 | 是 | 否 |
| [`getDeclaredMethods()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredMethods--) | 是 | 否 | 是 |
| [`getMethods()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethods--) | 是 | 是 | 否 |

定位构造函数的类方法

| [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) API | 成员列表？ | 继承成员？ | 私有成员？ |
| --- | --- | --- | --- |
| [`getDeclaredConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructor-java.lang.Class...-) | 否 | N/A¹ | 是 |
| [`getConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructor-java.lang.Class...-) | 否 | N/A¹ | 否 |
| [`getDeclaredConstructors()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructors--) | 是 | N/A¹ | 是 |
| [`getConstructors()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructors--) | 是 | N/A¹ | 否 |

¹ 构造函数不会被继承。

给定一个类名和感兴趣的成员指示，``ClassSpy``示例使用`get*s()`方法来确定所有公共元素的列表，包括任何继承的元素。

```java

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.lang.reflect.Member;
import static java.lang.System.out;

enum ClassMember { CONSTRUCTOR, FIELD, METHOD, CLASS, ALL }

public class ClassSpy {
    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    out.format("Class:%n  %s%n%n", c.getCanonicalName());

	    Package p = c.getPackage();
	    out.format("Package:%n  %s%n%n",
		       (p != null ? p.getName() : "-- No Package --"));

	    for (int i = 1; i < args.length; i++) {
		switch (ClassMember.valueOf(args[i])) {
		case CONSTRUCTOR:
		    printMembers(c.getConstructors(), "Constructor");
		    break;
		case FIELD:
		    printMembers(c.getFields(), "Fields");
		    break;
		case METHOD:
		    printMembers(c.getMethods(), "Methods");
		    break;
		case CLASS:
		    printClasses(c);
		    break;
		case ALL:
		    printMembers(c.getConstructors(), "Constuctors");
		    printMembers(c.getFields(), "Fields");
		    printMembers(c.getMethods(), "Methods");
		    printClasses(c);
		    break;
		default:
		    assert false;
		}
	    }

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static void printMembers(Member[] mbrs, String s) {
	out.format("%s:%n", s);
	for (Member mbr : mbrs) {
	    if (mbr instanceof Field)
		out.format("  %s%n", ((Field)mbr).toGenericString());
	    else if (mbr instanceof Constructor)
		out.format("  %s%n", ((Constructor)mbr).toGenericString());
	    else if (mbr instanceof Method)
		out.format("  %s%n", ((Method)mbr).toGenericString());
	}
	if (mbrs.length == 0)
	    out.format("  -- No %s --%n", s);
	out.format("%n");
    }

    private static void printClasses(Class<?> c) {
	out.format("Classes:%n");
	Class<?>[] clss = c.getClasses();
	for (Class<?> cls : clss)
	    out.format("  %s%n", cls.getCanonicalName());
	if (clss.length == 0)
	    out.format("  -- No member interfaces, classes, or enums --%n");
	out.format("%n");
    }
}

```

这个例子相对紧凑；然而，`printMembers()`方法略显笨拙，因为[`java.lang.reflect.Member`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Member.html)接口自反射最早的实现以来就存在，当引入泛型时，它无法被修改以包含更有用的`getGenericString()`方法。唯一的替代方法是像所示那样进行测试和转换，用`printConstructors()`、`printFields()`和`printMethods()`替换此方法，或者满足于相对简洁的[`Member.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Member.html#getName--)的结果。

输出示例及其解释如下。用户输入用斜体表示。

```java
$ *java ClassSpy java.lang.ClassCastException CONSTRUCTOR*
Class:
  java.lang.ClassCastException

Package:
  java.lang

Constructor:
  public java.lang.ClassCastException()
  public java.lang.ClassCastException(java.lang.String)

```

由于构造函数不会被继承，因此在直接超类[`RuntimeException`](https://docs.oracle.com/javase/8/docs/api/java/lang/RuntimeException.html)和其他超类中定义的异常链接机制构造函数（具有[`Throwable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Throwable.html)参数）将不会被找到。

```java
$ *java ClassSpy java.nio.channels.ReadableByteChannel METHOD*
Class:
  java.nio.channels.ReadableByteChannel

Package:
  java.nio.channels

Methods:
  public abstract int java.nio.channels.ReadableByteChannel.read
    (java.nio.ByteBuffer) throws java.io.IOException
  public abstract void java.nio.channels.Channel.close() throws
    java.io.IOException
  public abstract boolean java.nio.channels.Channel.isOpen()

```

接口[`java.nio.channels.ReadableByteChannel`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/ReadableByteChannel.html)定义了[`read()`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/ReadableByteChannel.html#read-java.nio.ByteBuffer-)。其余方法是从超级接口继承的。可以通过将`get*s()`替换为`getDeclared*s()`来轻松修改此代码，仅列出实际在类中声明的方法。

```java
$ *java ClassSpy ClassMember FIELD METHOD*
Class:
  ClassMember

Package:
  -- No Package --

Fields:
  public static final ClassMember ClassMember.CONSTRUCTOR
  public static final ClassMember ClassMember.FIELD
  public static final ClassMember ClassMember.METHOD
  public static final ClassMember ClassMember.CLASS
  public static final ClassMember ClassMember.ALL

Methods:
  public static ClassMember ClassMember.valueOf(java.lang.String)
  public static ClassMember[] ClassMember.values()
  public final int java.lang.Enum.hashCode()
  public final int java.lang.Enum.compareTo(E)
  public int java.lang.Enum.compareTo(java.lang.Object)
  public final java.lang.String java.lang.Enum.name()
  public final boolean java.lang.Enum.equals(java.lang.Object)
  public java.lang.String java.lang.Enum.toString()
  public static <T> T java.lang.Enum.valueOf
    (java.lang.Class<T>,java.lang.String)
  public final java.lang.Class<E> java.lang.Enum.getDeclaringClass()
  public final int java.lang.Enum.ordinal()
  public final native java.lang.Class<?> java.lang.Object.getClass()
  public final native void java.lang.Object.wait(long) throws
    java.lang.InterruptedException
  public final void java.lang.Object.wait(long,int) throws
    java.lang.InterruptedException
  public final void java.lang.Object.wait() hrows java.lang.InterruptedException
  public final native void java.lang.Object.notify()
  public final native void java.lang.Object.notifyAll()

```

在这些结果的字段部分，枚举常量被列出。虽然这些在技术上是字段，但将它们与其他字段区分开可能是有用的。这个例子可以修改为使用[`java.lang.reflect.Field.isEnumConstant()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#isEnumConstant--)来实现这一目的。在本教程的后续部分检查枚举中的``EnumSpy``示例包含了一个可能的实现。

在输出的方法部分中，观察到方法名称包含声明类的名称。因此，`toString()`方法是由[`Enum`](https://docs.oracle.com/javase/8/docs/api/java/lang/Enum.html#toString--)实现的，而不是从[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)继承的。可以通过使用[`Field.getDeclaringClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#getDeclaringClass--)来修改代码，使这一点更明显。以下片段展示了潜在解决方案的一部分。

```java
if (mbr instanceof Field) {
    Field f = (Field)mbr;
    out.format("  %s%n", f.toGenericString());
    out.format("  -- declared in: %s%n", f.getDeclaringClass());
}

```
