# 检查类的修饰符和类型

> 原文：[`docs.oracle.com/javase/tutorial/reflect/class/classModifiers.html`](https://docs.oracle.com/javase/tutorial/reflect/class/classModifiers.html)

一个类可以用一个或多个修饰符声明，这些修饰符会影响其运行时行为：

+   访问修饰符：`public`，`protected`和`private`

+   要求覆盖的修饰符：`abstract`

+   限制为一个实例的修饰符：`static`

+   禁止值修改的修饰符：`final`

+   强制严格浮点行为的修饰符：`strictfp`

+   注解

并非所有修饰符都允许用于所有类，例如接口不能是`final`，枚举不能是`abstract`。[`java.lang.reflect.Modifier`](https://docs.oracle.com/8/docs/api/java/lang/reflect/Modifier.html)包含了所有可能的修饰符声明。它还包含可用于解码由[`Class.getModifiers()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getModifiers--)返回的修饰符集合的方法。

``ClassDeclarationSpy``示例展示了如何获取类的声明组件，包括修饰符、泛型类型参数、实现的接口和继承路径。由于[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)实现了[`java.lang.reflect.AnnotatedElement`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html)接口，因此也可以查询运行时注解。

```java

import java.lang.annotation.Annotation;
import java.lang.reflect.Modifier;
import java.lang.reflect.Type;
import java.lang.reflect.TypeVariable;
import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;
import static java.lang.System.out;

public class ClassDeclarationSpy {
    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    out.format("Class:%n  %s%n%n", c.getCanonicalName());
	    out.format("Modifiers:%n  %s%n%n",
		       Modifier.toString(c.getModifiers()));

	    out.format("Type Parameters:%n");
	    TypeVariable[] tv = c.getTypeParameters();
	    if (tv.length != 0) {
		out.format("  ");
		for (TypeVariable t : tv)
		    out.format("%s ", t.getName());
		out.format("%n%n");
	    } else {
		out.format("  -- No Type Parameters --%n%n");
	    }

	    out.format("Implemented Interfaces:%n");
	    Type[] intfs = c.getGenericInterfaces();
	    if (intfs.length != 0) {
		for (Type intf : intfs)
		    out.format("  %s%n", intf.toString());
		out.format("%n");
	    } else {
		out.format("  -- No Implemented Interfaces --%n%n");
	    }

	    out.format("Inheritance Path:%n");
	    List<Class> l = new ArrayList<Class>();
	    printAncestor(c, l);
	    if (l.size() != 0) {
		for (Class<?> cl : l)
		    out.format("  %s%n", cl.getCanonicalName());
		out.format("%n");
	    } else {
		out.format("  -- No Super Classes --%n%n");
	    }

	    out.format("Annotations:%n");
	    Annotation[] ann = c.getAnnotations();
	    if (ann.length != 0) {
		for (Annotation a : ann)
		    out.format("  %s%n", a.toString());
		out.format("%n");
	    } else {
		out.format("  -- No Annotations --%n%n");
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static void printAncestor(Class<?> c, List<Class> l) {
	Class<?> ancestor = c.getSuperclass();
 	if (ancestor != null) {
	    l.add(ancestor);
	    printAncestor(ancestor, l);
 	}
    }
}

```

以下是一些输出示例。用户输入用斜体表示。

```java
$ *java ClassDeclarationSpy java.util.concurrent.ConcurrentNavigableMap*
Class:
  java.util.concurrent.ConcurrentNavigableMap

Modifiers:
  public abstract interface

Type Parameters:
  K V

Implemented Interfaces:
  java.util.concurrent.ConcurrentMap<K, V>
  java.util.NavigableMap<K, V>

Inheritance Path:
  -- No Super Classes --

Annotations:
  -- No Annotations --

```

这是源代码中[`java.util.concurrent.ConcurrentNavigableMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentNavigableMap.html)的实际声明：

```java
public interface ConcurrentNavigableMap<K,V>
    extends ConcurrentMap<K,V>, NavigableMap<K,V>

```

请注意，由于这是一个接口，它隐式地是`abstract`的。编译器为每个接口添加这个修饰符。此外，此声明包含两个泛型类型参数，`K`和`V`。示例代码仅打印这些参数的名称，但可以使用[`java.lang.reflect.TypeVariable`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/TypeVariable.html)中的方法检索有关它们的其他信息。接口也可以像上面显示的那样实现其他接口。

```java
$ *java ClassDeclarationSpy "[Ljava.lang.String;"*
Class:
  java.lang.String[]

Modifiers:
  public abstract final

Type Parameters:
  -- No Type Parameters --

Implemented Interfaces:
  interface java.lang.Cloneable
  interface java.io.Serializable

Inheritance Path:
  java.lang.Object

Annotations:
  -- No Annotations --

```

由于数组是运行时对象，所有类型信息都由 Java 虚拟机定义。特别是，数组实现了[`Cloneable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html)和[`java.io.Serializable`](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html)，它们的直接超类始终是[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)。

```java
$ *java ClassDeclarationSpy java.io.InterruptedIOException*
Class:
  java.io.InterruptedIOException

Modifiers:
  public

Type Parameters:
  -- No Type Parameters --

Implemented Interfaces:
  -- No Implemented Interfaces --

Inheritance Path:
  java.io.IOException
  java.lang.Exception
  java.lang.Throwable
  java.lang.Object

Annotations:
  -- No Annotations --

```

从继承路径可以推断出[`java.io.InterruptedIOException`](https://docs.oracle.com/javase/8/docs/api/java/io/InterruptedIOException.html)是一个受检异常，因为[`RuntimeException`](https://docs.oracle.com/javase/8/docs/api/java/lang/RuntimeException.html)不存在。

```java
$ *java ClassDeclarationSpy java.security.Identity*
Class:
  java.security.Identity

Modifiers:
  public abstract

Type Parameters:
  -- No Type Parameters --

Implemented Interfaces:
  interface java.security.Principal
  interface java.io.Serializable

Inheritance Path:
  java.lang.Object

Annotations:
  @java.lang.Deprecated()

```

此输出显示[`java.security.Identity`](https://docs.oracle.com/javase/8/docs/api/java/security/Identity.html)，一个已弃用的 API，具有注解[`java.lang.Deprecated`](https://docs.oracle.com/javase/8/docs/api/java/lang/Deprecated.html)。这可能被反射代码用于检测已弃用的 API。

* * *

**注意：** 并非所有注解都可以通过反射获得。只有那些具有[`java.lang.annotation.RetentionPolicy`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/RetentionPolicy.html)为[`RUNTIME`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/RetentionPolicy.html#RUNTIME)的注解是可访问的。在语言中预定义的三个注解[`@Deprecated`](https://docs.oracle.com/javase/8/docs/api/java/lang/Deprecated.html)，[`@Override`](https://docs.oracle.com/javase/8/docs/api/java/lang/Override.html)，和[`@SuppressWarnings`](https://docs.oracle.com/javase/8/docs/api/java/lang/SuppressWarnings.html)中，只有[`@Deprecated`](https://docs.oracle.com/javase/8/docs/api/java/lang/Deprecated.html)在运行时可用。

* * *
