# 调用方法

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/methodInvocation.html`](https://docs.oracle.com/javase/tutorial/reflect/member/methodInvocation.html)

反射提供了一种在类上调用方法的方式。通常，只有在非反射代码中无法将类的实例强制转换为所需类型时才需要这样做。方法是使用[`java.lang.reflect.Method.invoke()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#invoke-java.lang.Object-java.lang.Object...-)来调用的。第一个参数是要调用该特定方法的对象实例。（如果方法是`static`，第一个参数应为`null`。）后续参数是方法的参数。如果底层方法抛出异常，它将被[`java.lang.reflect.InvocationTargetException`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html)包装。可以使用异常链接机制的[`InvocationTargetException.getCause()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html#getCause--)方法检索方法的原始异常。

## 查找和调用具有特定声明的方法

考虑一个测试套件，它使用反射来调用给定类中的私有测试方法。``Deet``示例搜索类中以字符串"`test`"开头，具有布尔返回类型和单个[`Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html)参数的`public`方法。然后调用每个匹配的方法。

```java

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.lang.reflect.Type;
import java.util.Locale;
import static java.lang.System.out;
import static java.lang.System.err;

public class Deet<T> {
    private boolean testDeet(Locale l) {
	// getISO3Language() may throw a MissingResourceException
	out.format("Locale = %s, ISO Language Code = %s%n", l.getDisplayName(), l.getISO3Language());
	return true;
    }

    private int testFoo(Locale l) { return 0; }
    private boolean testBar() { return true; }

    public static void main(String... args) {
	if (args.length != 4) {
	    err.format("Usage: java Deet <classname> <langauge> <country> <variant>%n");
	    return;
	}

	try {
	    Class<?> c = Class.forName(args[0]);
	    Object t = c.newInstance();

	    Method[] allMethods = c.getDeclaredMethods();
	    for (Method m : allMethods) {
		String mname = m.getName();
		if (!mname.startsWith("test")
		    || (m.getGenericReturnType() != boolean.class)) {
		    continue;
		}
 		Type[] pType = m.getGenericParameterTypes();
 		if ((pType.length != 1)
		    || Locale.class.isAssignableFrom(pType[0].getClass())) {
 		    continue;
 		}

		out.format("invoking %s()%n", mname);
		try {
		    m.setAccessible(true);
		    Object o = m.invoke(t, new Locale(args[1], args[2], args[3]));
		    out.format("%s() returned %b%n", mname, (Boolean) o);

		// Handle any exceptions thrown by method to be invoked.
		} catch (InvocationTargetException x) {
		    Throwable cause = x.getCause();
		    err.format("invocation of %s failed: %s%n",
			       mname, cause.getMessage());
		}
	    }

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}

```

`Deet`调用[`getDeclaredMethods()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredMethods--)，它将返回类中明确声明的所有方法。此外，使用[`Class.isAssignableFrom()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isAssignableFrom-java.lang.Class-)来确定定位方法的参数是否与所需调用兼容。从技术上讲，代码可以测试以下语句是否为`true`，因为[`Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html)是`final`的：

```java
Locale.class == pType[0].getClass()

```

然而，[`Class.isAssignableFrom()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isAssignableFrom-java.lang.Class-)更通用。

```java
$ *java Deet Deet ja JP JP*
invoking testDeet()
Locale = Japanese (Japan,JP), 
ISO Language Code = jpn
testDeet() returned true

```

```java
$ *java Deet Deet xx XX XX*
invoking testDeet()
invocation of testDeet failed: 
Couldn't find 3-letter language code for xx

```

首先，请注意只有`testDeet()`符合代码强制执行的声明限制。接下来，当`testDeet()`传递无效参数时，它会抛出一个未经检查的[`java.util.MissingResourceException`](https://docs.oracle.com/javase/8/docs/api/java/util/MissingResourceException.html)。在反射中，对于已检查和未检查的异常处理没有区别。它们都被包装在一个[`InvocationTargetException`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html)中。

## 调用具有可变数量参数的方法

[`Method.invoke()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#invoke-java.lang.Object-java.lang.Object...-)可用于向方法传递可变数量的参数。要理解的关键概念是，可变参数的方法被实现为如果可变参数被打包在一个数组中。

``InvokeMain``示例演示了如何调用任何类中的`main()`入口点，并在运行时传递一组参数。

```java

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.Arrays;

public class InvokeMain {
    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    Class[] argTypes = new Class[] { String[].class };
	    Method main = c.getDeclaredMethod("main", argTypes);
  	    String[] mainArgs = Arrays.copyOfRange(args, 1, args.length);
	    System.out.format("invoking %s.main()%n", c.getName());
	    main.invoke(null, (Object)mainArgs);

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	} catch (NoSuchMethodException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	}
    }
}

```

首先，为了找到`main()`方法，代码会搜索一个名为"main"的类，该类有一个参数，参数是一个[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)数组。由于`main()`是`static`的，`null`是传递给[`Method.invoke()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#invoke-java.lang.Object-java.lang.Object...-)的第一个参数。第二个参数是要传递的参数数组。

```java
$ *java InvokeMain Deet Deet ja JP JP*
invoking Deet.main()
invoking testDeet()
Locale = Japanese (Japan,JP), 
ISO Language Code = jpn
testDeet() returned true

```
