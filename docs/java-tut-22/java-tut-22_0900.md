# 获取方法类型信息

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/methodType.html`](https://docs.oracle.com/javase/tutorial/reflect/member/methodType.html)

方法声明包括名称、修饰符、参数、返回类型和可抛出异常列表。[`java.lang.reflect.Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html)类提供了获取这些信息的方法。

``MethodSpy``示例演示了如何枚举给定类中声明的所有方法，并检索给定名称的所有方法的返回、参数和异常类型。

```java

import java.lang.reflect.Method;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class MethodSpy {
    private static final String  fmt = "%24s: %s%n";

    // for the morbidly curious
    <E extends RuntimeException> void genericThrow() throws E {}

    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    Method[] allMethods = c.getDeclaredMethods();
	    for (Method m : allMethods) {
		if (!m.getName().equals(args[1])) {
		    continue;
		}
		out.format("%s%n", m.toGenericString());

		out.format(fmt, "ReturnType", m.getReturnType());
		out.format(fmt, "GenericReturnType", m.getGenericReturnType());

		Class<?>[] pType  = m.getParameterTypes();
		Type[] gpType = m.getGenericParameterTypes();
		for (int i = 0; i < pType.length; i++) {
		    out.format(fmt,"ParameterType", pType[i]);
		    out.format(fmt,"GenericParameterType", gpType[i]);
		}

		Class<?>[] xType  = m.getExceptionTypes();
		Type[] gxType = m.getGenericExceptionTypes();
		for (int i = 0; i < xType.length; i++) {
		    out.format(fmt,"ExceptionType", xType[i]);
		    out.format(fmt,"GenericExceptionType", gxType[i]);
		}
	    }

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

这是[`Class.getConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructor-java.lang.Class...-)的输出，这是一个具有参数化类型和可变数量参数的方法的示例。

```java
$ *java MethodSpy java.lang.Class getConstructor*
public java.lang.reflect.Constructor<T> java.lang.Class.getConstructor
  (java.lang.Class<?>[]) throws java.lang.NoSuchMethodException,
  java.lang.SecurityException
              ReturnType: class java.lang.reflect.Constructor
       GenericReturnType: java.lang.reflect.Constructor<T>
           ParameterType: class [Ljava.lang.Class;
    GenericParameterType: java.lang.Class<?>[]
           ExceptionType: class java.lang.NoSuchMethodException
    GenericExceptionType: class java.lang.NoSuchMethodException
           ExceptionType: class java.lang.SecurityException
    GenericExceptionType: class java.lang.SecurityException

```

这是源代码中方法的实际声明：

```java
public Constructor<T> getConstructor(Class<?>... parameterTypes)

```

首先注意返回和参数类型是通用的。如果存在类文件中的签名属性，[`Method.getGenericReturnType()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#getGenericReturnType--)将会查看它。如果属性不可用，则会回退到未更改的[`Method.getReturnType()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#getReturnType--)，这是在引入泛型之前没有更改的。其他以反射中某个值*Foo*为名称的`getGeneric*Foo*()`方法实现方式类似。

接下来，请注意最后（也是唯一的）参数`parameterType`是可变参数（具有可变数量的参数）类型为`java.lang.Class`。它被表示为`java.lang.Class`类型的单维数组。这可以通过调用[`Method.isVarArgs()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#isVarArgs--)来区分明确为`java.lang.Class`数组的参数。`Method.get*Types()`返回值的语法在[`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--)中描述。

以下示例说明了具有通用返回类型的方法。

```java
$ *java MethodSpy java.lang.Class cast*
public T java.lang.Class.cast(java.lang.Object)
              ReturnType: class java.lang.Object
       GenericReturnType: T
           ParameterType: class java.lang.Object
    GenericParameterType: class java.lang.Object

```

方法[`Class.cast()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#cast-java.lang.Object-)的通用返回类型报告为`java.lang.Object`，因为泛型是通过*类型擦除*实现的，在编译期间删除了有关泛型类型的所有信息。`T`的擦除由[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)的声明定义：

```java
public final class Class<T> implements ...

```

因此，`T`被类型变量的上界替换，即`java.lang.Object`。

最后一个示例说明了具有多个重载的方法的输出。

```java
$ *java MethodSpy java.io.PrintStream format*
public java.io.PrintStream java.io.PrintStream.format
  (java.util.Locale,java.lang.String,java.lang.Object[])
              ReturnType: class java.io.PrintStream
       GenericReturnType: class java.io.PrintStream
           ParameterType: class java.util.Locale
    GenericParameterType: class java.util.Locale
           ParameterType: class java.lang.String
    GenericParameterType: class java.lang.String
           ParameterType: class [Ljava.lang.Object;
    GenericParameterType: class [Ljava.lang.Object;
public java.io.PrintStream java.io.PrintStream.format
  (java.lang.String,java.lang.Object[])
              ReturnType: class java.io.PrintStream
       GenericReturnType: class java.io.PrintStream
           ParameterType: class java.lang.String
    GenericParameterType: class java.lang.String
           ParameterType: class [Ljava.lang.Object;
    GenericParameterType: class [Ljava.lang.Object;

```

如果发现同一方法名的多个重载，它们都会被[`Class.getDeclaredMethods()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredMethods--)返回。由于`format()`有两个重载（一个带有[`Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html)，一个没有），`MethodSpy`都会显示出来。

* * *

**注意：** [`Method.getGenericExceptionTypes()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#getGenericExceptionTypes--)的存在是因为实际上可以声明一个带有泛型异常类型的方法。然而，这很少被使用，因为无法捕获泛型异常类型。

* * *
