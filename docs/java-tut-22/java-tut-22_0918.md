# 使用枚举类型获取和设置字段

> 原文：[`docs.oracle.com/javase/tutorial/reflect/special/enumSetGet.html`](https://docs.oracle.com/javase/tutorial/reflect/special/enumSetGet.html)

存储枚举的字段与任何其他引用类型一样设置和检索，使用[`Field.set()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#set-java.lang.Object-java.lang.Object-)和[`Field.get()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#get-java.lang.Object-)。有关访问字段的更多信息，请参阅本教程的 Fields 部分。

考虑一个需要在服务器应用程序中动态修改跟踪级别的应用程序，通常在运行时不允许此更改。假设服务器对象的实例可用。``SetTrace``示例展示了代码如何将枚举的[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)表示转换为枚举类型，并检索和设置存储枚举的字段的值。

```java

import java.lang.reflect.Field;
import static java.lang.System.out;

enum TraceLevel { OFF, LOW, MEDIUM, HIGH, DEBUG }

class MyServer {
    private TraceLevel level = TraceLevel.OFF;
}

public class SetTrace {
    public static void main(String... args) {
	TraceLevel newLevel = TraceLevel.valueOf(args[0]);

	try {
	    MyServer svr = new MyServer();
	    Class<?> c = svr.getClass();
	    Field f = c.getDeclaredField("level");
	    f.setAccessible(true);
	    TraceLevel oldLevel = (TraceLevel)f.get(svr);
	    out.format("Original trace level:  %s%n", oldLevel);

	    if (oldLevel != newLevel) {
 		f.set(svr, newLevel);
		out.format("    New  trace level:  %s%n", f.get(svr));
	    }

        // production code should handle these exceptions more gracefully
	} catch (IllegalArgumentException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	}
    }
}

```

由于枚举常量是单例，可以使用`==`和`!=`运算符来比较相同类型的枚举常量。

```java
$ *java SetTrace OFF*
Original trace level:  OFF
$ *java SetTrace DEBUG*
Original trace level:  OFF
    New  trace level:  DEBUG

```
