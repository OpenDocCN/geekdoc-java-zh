# 创建新的类实例

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/ctorInstance.html`](https://docs.oracle.com/javase/tutorial/reflect/member/ctorInstance.html)

创建类实例的两种反射方法：[`java.lang.reflect.Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) 和 [`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)。前者更受青睐，因此在这些示例中使用，原因如下：

+   [`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)只能调用零参数构造函数，而[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-)可以调用任何构造函数，无论参数个数如何。

+   无论构造函数抛出的是已检查异常还是未检查异常，[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)都会抛出该异常。[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-)总是用[`InvocationTargetException`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html)包装抛出的异常。

+   [`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)要求构造函数可见；[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-)在某些情况下可以调用`private`构造函数。

有时可能希望从仅在构造后设置的对象中检索内部状态。考虑一个场景，需要获取[`java.io.Console`](https://docs.oracle.com/javase/8/docs/api/java/io/Console.html)使用的内部字符集。（`Console`字符集存储在私有字段中，并且不一定与[`java.nio.charset.Charset.defaultCharset()`](https://docs.oracle.com/javase/8/docs/api/java/nio/charset/Charset.html#defaultCharset--)返回的 Java 虚拟机默认字符集相同）。``ConsoleCharset``示例展示了如何实现这一点：

```java

import java.io.Console;
import java.nio.charset.Charset;
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import static java.lang.System.out;

public class ConsoleCharset {
    public static void main(String... args) {
	Constructor[] ctors = Console.class.getDeclaredConstructors();
	Constructor ctor = null;
	for (int i = 0; i < ctors.length; i++) {
	    ctor = ctors[i];
	    if (ctor.getGenericParameterTypes().length == 0)
		break;
	}

	try {
	    ctor.setAccessible(true);
 	    Console c = (Console)ctor.newInstance();
	    Field f = c.getClass().getDeclaredField("cs");
	    f.setAccessible(true);
	    out.format("Console charset         :  %s%n", f.get(c));
	    out.format("Charset.defaultCharset():  %s%n",
		       Charset.defaultCharset());

        // production code should handle these exceptions more gracefully
	} catch (InstantiationException x) {
	    x.printStackTrace();
 	} catch (InvocationTargetException x) {
 	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	}
    }
}

```

* * *

**注意：**

如果构造函数没有参数且已经可访问，则[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)才会成功。否则，需要像上面的示例一样使用[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-)。

* * *

UNIX 系统的示例输出：

```java
$ *java ConsoleCharset*
Console charset          :  ISO-8859-1
Charset.defaultCharset() :  ISO-8859-1

```

Windows 系统的示例输出：

```java
C:\> *java ConsoleCharset*
Console charset          :  IBM437
Charset.defaultCharset() :  windows-1252

```

另一个常见的 [`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) 应用是调用需要参数的构造函数。``RestoreAliases`` 示例找到一个特定的单参数构造函数并调用它：

```java

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;
import static java.lang.System.out;

class EmailAliases {
    private Set<String> aliases;
    private EmailAliases(HashMap<String, String> h) {
	aliases = h.keySet();
    }

    public void printKeys() {
	out.format("Mail keys:%n");
	for (String k : aliases)
	    out.format("  %s%n", k);
    }
}

public class RestoreAliases {

    private static Map<String, String> defaultAliases = new HashMap<String, String>();
    static {
	defaultAliases.put("Duke", "duke@i-love-java");
	defaultAliases.put("Fang", "fang@evil-jealous-twin");
    }

    public static void main(String... args) {
	try {
	    Constructor ctor = EmailAliases.class.getDeclaredConstructor(HashMap.class);
	    ctor.setAccessible(true);
	    EmailAliases email = (EmailAliases)ctor.newInstance(defaultAliases);
	    email.printKeys();

        // production code should handle these exceptions more gracefully
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	} catch (NoSuchMethodException x) {
	    x.printStackTrace();
	}
    }
}

```

这个示例使用 [`Class.getDeclaredConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructor-java.lang.Class...-) 来找到一个参数类型为 [`java.util.HashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html) 的构造函数。请注意，只需传递 `HashMap.class` 就足够了，因为任何 `get*Constructor()` 方法的参数只需要类来确定类型。由于 [类型擦除](https://docs.oracle.com/javase/specs/jls/se7/html/jls-4.html#jls-4.6)，以下表达式求值为 `true`：

```java
HashMap.class == defaultAliases.getClass()

```

然后，示例使用这个构造函数使用 [`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) 创建类的新实例。

```java
$ *java RestoreAliases*
Mail keys:
  Duke
  Fang

```
