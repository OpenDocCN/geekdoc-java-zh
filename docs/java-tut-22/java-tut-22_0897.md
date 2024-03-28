# 获取和设置字段值

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/fieldValues.html`](https://docs.oracle.com/javase/tutorial/reflect/member/fieldValues.html)

给定一个类的实例，可以使用反射来设置该类中字段的值。通常只在特殊情况下进行此操作，当通常方式无法设置值时。由于这种访问通常违反了类的设计意图，应该谨慎使用。

``Book``类演示了如何设置长整型、数组和枚举字段类型的值。获取和设置其他基本类型的方法在[`Field`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#method_summary)中有描述。

```java

import java.lang.reflect.Field;
import java.util.Arrays;
import static java.lang.System.out;

enum Tweedle { DEE, DUM }

public class Book {
    public long chapters = 0;
    public String[] characters = { "Alice", "White Rabbit" };
    public Tweedle twin = Tweedle.DEE;

    public static void main(String... args) {
	Book book = new Book();
	String fmt = "%6S:  %-12s = %s%n";

	try {
	    Class<?> c = book.getClass();

	    Field chap = c.getDeclaredField("chapters");
	    out.format(fmt, "before", "chapters", book.chapters);
  	    chap.setLong(book, 12);
	    out.format(fmt, "after", "chapters", chap.getLong(book));

	    Field chars = c.getDeclaredField("characters");
	    out.format(fmt, "before", "characters",
		       Arrays.asList(book.characters));
	    String[] newChars = { "Queen", "King" };
	    chars.set(book, newChars);
	    out.format(fmt, "after", "characters",
		       Arrays.asList(book.characters));

	    Field t = c.getDeclaredField("twin");
	    out.format(fmt, "before", "twin", book.twin);
	    t.set(book, Tweedle.DUM);
	    out.format(fmt, "after", "twin", t.get(book));

        // production code should handle these exceptions more gracefully
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}

```

这是相应的输出：

```java
$ *java Book*
BEFORE:  chapters     = 0
 AFTER:  chapters     = 12
BEFORE:  characters   = [Alice, White Rabbit]
 AFTER:  characters   = [Queen, King]
BEFORE:  twin         = DEE
 AFTER:  twin         = DUM

```

* * *

**注意：** 通过反射设置字段的值会带来一定的性能开销，因为必须执行各种操作，如验证访问权限。从运行时的角度来看，效果是相同的，操作与直接在类代码中更改值一样原子。

使用反射可能导致一些运行时优化丢失。例如，以下代码很可能会被 Java 虚拟机优化：

```java
int x = 1;
x = 2;
x = 3;

```

使用`Field.set*()`的等效代码可能不会。

* * *
