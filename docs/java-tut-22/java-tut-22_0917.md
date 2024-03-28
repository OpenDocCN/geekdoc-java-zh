# 检查枚举

> 原文：[`docs.oracle.com/javase/tutorial/reflect/special/enumMembers.html`](https://docs.oracle.com/javase/tutorial/reflect/special/enumMembers.html)

反射提供了三个特定于枚举的 API：

[`Class.isEnum()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isEnum--)

表示此类是否表示枚举类型

[`Class.getEnumConstants()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnumConstants--)

检索由枚举定义的枚举常量列表，按照它们声明的顺序

[`java.lang.reflect.Field.isEnumConstant()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#isEnumConstant--)

表示此字段是否表示枚举类型的元素

有时需要动态检索枚举常量的列表；在非反射代码中，可以通过在枚举上调用隐式声明的静态方法 [`values()`](https://docs.oracle.com/javase/specs/jls/se7/html/jls-8.html) 来实现这一点。 如果枚举类型的实例不可用，则获取可能值列表的唯一方法是调用 [`Class.getEnumConstants()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnumConstants--)，因为无法实例化枚举类型。

给定完全限定名称，``EnumConstants`` 示例显示如何使用 [`Class.getEnumConstants()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnumConstants--) 检索枚举中常量的有序列表。

```java

import java.util.Arrays;
import static java.lang.System.out;

enum Eon { HADEAN, ARCHAEAN, PROTEROZOIC, PHANEROZOIC }

public class EnumConstants {
    public static void main(String... args) {
	try {
	    Class<?> c = (args.length == 0 ? Eon.class : Class.forName(args[0]));
	    out.format("Enum name:  %s%nEnum constants:  %s%n",
		       c.getName(), Arrays.asList(c.getEnumConstants()));
	    if (c == Eon.class)
		out.format("  Eon.values():  %s%n",
			   Arrays.asList(Eon.values()));

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

输出示例如下。 用户输入以斜体显示。

```java
$ *java EnumConstants java.lang.annotation.RetentionPolicy*
Enum name:  java.lang.annotation.RetentionPolicy
Enum constants:  [SOURCE, CLASS, RUNTIME]

```

```java
$ *java EnumConstants java.util.concurrent.TimeUnit*
Enum name:  java.util.concurrent.TimeUnit
Enum constants:  [NANOSECONDS, MICROSECONDS, 
                  MILLISECONDS, SECONDS, 
                  MINUTES, HOURS, DAYS]

```

该示例还表明，通过调用 [`Class.getEnumConstants()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnumConstants--) 返回的值与在枚举类型上调用 `values()` 返回的值相同。

```java
$ *java EnumConstants*
Enum name:  Eon
Enum constants:  [HADEAN, ARCHAEAN, 
                  PROTEROZOIC, PHANEROZOIC]
Eon.values():  [HADEAN, ARCHAEAN, 
                PROTEROZOIC, PHANEROZOIC]

```

由于枚举是类，可以使用本教程中描述的字段、方法和构造函数部分中描述的相同反射 API 获取其他信息。 ``EnumSpy`` 代码示例说明了如何使用这些 API 获取有关枚举声明的其他信息。 该示例使用 [`Class.isEnum()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isEnum--) 来限制要检查的类集。 它还使用 [`Field.isEnumConstant()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#isEnumConstant--) 来区分枚举声明中的枚举常量和其他字段（并非所有字段都是枚举常量）。

```java

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.lang.reflect.Member;
import java.util.List;
import java.util.ArrayList;
import static java.lang.System.out;

public class EnumSpy {
    private static final String fmt = "  %11s:  %s %s%n";

    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    if (!c.isEnum()) {
		out.format("%s is not an enum type%n", c);
		return;
	    }
	    out.format("Class:  %s%n", c);

	    Field[] flds = c.getDeclaredFields();
	    List<Field> cst = new ArrayList<Field>();  // enum constants
	    List<Field> mbr = new ArrayList<Field>();  // member fields
	    for (Field f : flds) {
		if (f.isEnumConstant())
		    cst.add(f);
		else
		    mbr.add(f);
	    }
	    if (!cst.isEmpty())
		print(cst, "Constant");
	    if (!mbr.isEmpty())
		print(mbr, "Field");

	    Constructor[] ctors = c.getDeclaredConstructors();
	    for (Constructor ctor : ctors) {
		out.format(fmt, "Constructor", ctor.toGenericString(),
			   synthetic(ctor));
	    }

	    Method[] mths = c.getDeclaredMethods();
	    for (Method m : mths) {
		out.format(fmt, "Method", m.toGenericString(),
			   synthetic(m));
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static void print(List<Field> lst, String s) {
	for (Field f : lst) {
 	    out.format(fmt, s, f.toGenericString(), synthetic(f));
	}
    }

    private static String synthetic(Member m) {
	return (m.isSynthetic() ? "[ synthetic ]" : "");
    }
}

```

```java
$ *java EnumSpy java.lang.annotation.RetentionPolicy*
Class:  class java.lang.annotation.RetentionPolicy
     Constant:  public static final java.lang.annotation.RetentionPolicy
                  java.lang.annotation.RetentionPolicy.SOURCE 
     Constant:  public static final java.lang.annotation.RetentionPolicy
                  java.lang.annotation.RetentionPolicy.CLASS 
     Constant:  public static final java.lang.annotation.RetentionPolicy 
                  java.lang.annotation.RetentionPolicy.RUNTIME 
        Field:  private static final java.lang.annotation.RetentionPolicy[] 
                  java.lang.annotation.RetentionPolicy. [ synthetic ]
  Constructor:  private java.lang.annotation.RetentionPolicy() 
       Method:  public static java.lang.annotation.RetentionPolicy[]
                  java.lang.annotation.RetentionPolicy.values() 
       Method:  public static java.lang.annotation.RetentionPolicy
                  java.lang.annotation.RetentionPolicy.valueOf(java.lang.String) 

```

输出显示，[`java.lang.annotation.RetentionPolicy`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/RetentionPolicy.html)的声明仅包含三个枚举常量。枚举常量暴露为`public static final`字段。字段、构造函数和方法是由编译器生成的。`$VALUES`字段与`values()`方法的实现有关。

* * *

**注意：**出于各种原因，包括支持枚举类型的演变，枚举常量的声明顺序很重要。[`Class.getFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getFields--)和[`Class.getDeclaredFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredFields--)不能保证返回值的顺序与声明源代码中的顺序匹配。如果应用程序需要排序，请使用[`Class.getEnumConstants()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnumConstants--)。

* * *

对于[`java.util.concurrent.TimeUnit`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/TimeUnit.html)的输出显示，更复杂的枚举是可能的。这个类包括几个方法以及额外声明为`static final`的字段，这些字段不是枚举常量。

```java
$ java EnumSpy java.util.concurrent.TimeUnit
Class:  class java.util.concurrent.TimeUnit
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.NANOSECONDS
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.MICROSECONDS
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.MILLISECONDS
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.SECONDS
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.MINUTES
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.HOURS
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.DAYS
        Field:  static final long java.util.concurrent.TimeUnit.C0
        Field:  static final long java.util.concurrent.TimeUnit.C1
        Field:  static final long java.util.concurrent.TimeUnit.C2
        Field:  static final long java.util.concurrent.TimeUnit.C3
        Field:  static final long java.util.concurrent.TimeUnit.C4
        Field:  static final long java.util.concurrent.TimeUnit.C5
        Field:  static final long java.util.concurrent.TimeUnit.C6
        Field:  static final long java.util.concurrent.TimeUnit.MAX
        Field:  private static final java.util.concurrent.TimeUnit[] 
                  java.util.concurrent.TimeUnit. [ synthetic ]
  Constructor:  private java.util.concurrent.TimeUnit()
  Constructor:  java.util.concurrent.TimeUnit
                  (java.lang.String,int,java.util.concurrent.TimeUnit)
                  [ synthetic ]
       Method:  public static java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.valueOf(java.lang.String)
       Method:  public static java.util.concurrent.TimeUnit[] 
                  java.util.concurrent.TimeUnit.values()
       Method:  public void java.util.concurrent.TimeUnit.sleep(long) 
                  throws java.lang.InterruptedException
       Method:  public long java.util.concurrent.TimeUnit.toNanos(long)
       Method:  public long java.util.concurrent.TimeUnit.convert
                  (long,java.util.concurrent.TimeUnit)
       Method:  abstract int java.util.concurrent.TimeUnit.excessNanos
                  (long,long)
       Method:  public void java.util.concurrent.TimeUnit.timedJoin
                  (java.lang.Thread,long) throws java.lang.InterruptedException
       Method:  public void java.util.concurrent.TimeUnit.timedWait
                  (java.lang.Object,long) throws java.lang.InterruptedException
       Method:  public long java.util.concurrent.TimeUnit.toDays(long)
       Method:  public long java.util.concurrent.TimeUnit.toHours(long)
       Method:  public long java.util.concurrent.TimeUnit.toMicros(long)
       Method:  public long java.util.concurrent.TimeUnit.toMillis(long)
       Method:  public long java.util.concurrent.TimeUnit.toMinutes(long)
       Method:  public long java.util.concurrent.TimeUnit.toSeconds(long)
       Method:  static long java.util.concurrent.TimeUnit.x(long,long,long)

```
