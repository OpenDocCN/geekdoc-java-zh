# 检索和解析方法修饰符

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/methodModifiers.html`](https://docs.oracle.com/javase/tutorial/reflect/member/methodModifiers.html)

方法声明中可能包含的几个修饰符：

+   访问修饰符：`public`、`protected`和`private`

+   限制为一个实例的修饰符：`static`

+   禁止值修改的修饰符：`final`

+   要求覆盖的修饰符：`abstract`

+   防止重入的修饰符：`synchronized`

+   表示在另一种编程语言中实现的修饰符：`native`

+   强制严格浮点行为的修饰符：`strictfp`

+   注解

``MethodModifierSpy``示例列出了具有给定名称的方法的修饰符。它还显示方法是否是合成的（编译器生成的）、可变参数的，或者是桥接方法（编译器生成的以支持泛型接口）。

```java

import java.lang.reflect.Method;
import java.lang.reflect.Modifier;
import static java.lang.System.out;

public class MethodModifierSpy {

    private static int count;
    private static synchronized void inc() { count++; }
    private static synchronized int cnt() { return count; }

    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    Method[] allMethods = c.getDeclaredMethods();
	    for (Method m : allMethods) {
		if (!m.getName().equals(args[1])) {
		    continue;
		}
		out.format("%s%n", m.toGenericString());
		out.format("  Modifiers:  %s%n",
			   Modifier.toString(m.getModifiers()));
		out.format("  [ synthetic=%-5b var_args=%-5b bridge=%-5b ]%n",
			   m.isSynthetic(), m.isVarArgs(), m.isBridge());
		inc();
	    }
	    out.format("%d matching overload%s found%n", cnt(),
		       (cnt() == 1 ? "" : "s"));

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

``MethodModifierSpy``产生的输出示例如下。

```java
$ *java MethodModifierSpy java.lang.Object wait*
public final void java.lang.Object.wait() throws java.lang.InterruptedException
  Modifiers:  public final
  [ synthetic=false var_args=false bridge=false ]
public final void java.lang.Object.wait(long,int)
  throws java.lang.InterruptedException
  Modifiers:  public final
  [ synthetic=false var_args=false bridge=false ]
public final native void java.lang.Object.wait(long)
  throws java.lang.InterruptedException
  Modifiers:  public final native
  [ synthetic=false var_args=false bridge=false ]
3 matching overloads found

```

```java
$ *java MethodModifierSpy java.lang.StrictMath toRadians*
public static double java.lang.StrictMath.toRadians(double)
  Modifiers:  public static strictfp
  [ synthetic=false var_args=false bridge=false ]
1 matching overload found

```

```java
$ *java MethodModifierSpy MethodModifierSpy inc*
private synchronized void MethodModifierSpy.inc()
  Modifiers: private synchronized
  [ synthetic=false var_args=false bridge=false ]
1 matching overload found

```

```java
$ *java MethodModifierSpy java.lang.Class getConstructor*
public java.lang.reflect.Constructor<T> java.lang.Class.getConstructor
  (java.lang.Class<T>[]) throws java.lang.NoSuchMethodException,
  java.lang.SecurityException
  Modifiers: public transient
  [ synthetic=false var_args=true bridge=false ]
1 matching overload found

```

```java
$ *java MethodModifierSpy java.lang.String compareTo*
public int java.lang.String.compareTo(java.lang.String)
  Modifiers: public
  [ synthetic=false var_args=false bridge=false ]
public int java.lang.String.compareTo(java.lang.Object)
  Modifiers: public volatile
  [ synthetic=true  var_args=false bridge=true  ]
2 matching overloads found

```

请注意，[`Method.isVarArgs()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#isVarArgs--)对于[`Class.getConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructor-java.lang.Class...-)返回`true`。这表明方法声明如下：

```java
public Constructor<T> getConstructor(Class<?>... parameterTypes)

```

不要这样：

```java
public Constructor<T> getConstructor(Class<?> [] parameterTypes)

```

请注意，[`String.compareTo()`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#compareTo-java.lang.String-)的输出包含两种方法。在`String.java`中声明的方法：

```java
public int compareTo(String anotherString);

```

和第二个*合成*或编译器生成的*桥接*方法。这是因为[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)实现了参数化接口[`Comparable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)。在类型擦除期间，继承方法[`Comparable.compareTo()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html#compareTo-T-)的参数类型从`java.lang.Object`更改为`java.lang.String`。由于`Comparable`和`String`中的`compareTo`方法的参数类型在擦除后不再匹配，因此无法进行覆盖。在所有其他情况下，这将产生编译时错误，因为接口未实现。桥接方法的添加避免了这个问题。

[`Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html)实现了[`java.lang.reflect.AnnotatedElement`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html)。因此，任何具有[`java.lang.annotation.RetentionPolicy.RUNTIME`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/RetentionPolicy.html#RUNTIME)的运行时注解都可以被检索。有关获取注解的示例，请参见检查类修饰符和类型部分。
