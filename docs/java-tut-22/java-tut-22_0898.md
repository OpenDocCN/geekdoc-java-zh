# 故障排除

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/fieldTrouble.html`](https://docs.oracle.com/javase/tutorial/reflect/member/fieldTrouble.html)

开发人员遇到的一些常见问题及其解释和解决方法如下。

## 由于不可转换类型而引发的`IllegalArgumentException`

`FieldTrouble`示例将生成一个[`IllegalArgumentException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html)。调用[`Field.setInt()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#setInt-java.lang.Object-int-) 来设置一个引用类型为`Integer`的字段，其值为原始类型。在非反射等效的`Integer val = 42`中，编译器会将原始类型`42`转换（或*装箱*）为引用类型`new Integer(42)`，以便其类型检查接受该语句。在使用反射时，类型检查仅在运行时发生，因此没有机会对值进行装箱。

```java

import java.lang.reflect.Field;

public class FieldTrouble {
    public Integer val;

    public static void main(String... args) {
	FieldTrouble ft = new FieldTrouble();
	try {
	    Class<?> c = ft.getClass();
	    Field f = c.getDeclaredField("val");
  	    f.setInt(ft, 42);               // IllegalArgumentException

        // production code should handle these exceptions more gracefully
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
 	} catch (IllegalAccessException x) {
 	    x.printStackTrace();
	}
    }
}

```

```java
$ *java FieldTrouble*
Exception in thread "main" java.lang.IllegalArgumentException: Can not set
  java.lang.Object field FieldTrouble.val to (long)42
        at sun.reflect.UnsafeFieldAccessorImpl.throwSetIllegalArgumentException
          (UnsafeFieldAccessorImpl.java:146)
        at sun.reflect.UnsafeFieldAccessorImpl.throwSetIllegalArgumentException
          (UnsafeFieldAccessorImpl.java:174)
        at sun.reflect.UnsafeObjectFieldAccessorImpl.setLong
          (UnsafeObjectFieldAccessorImpl.java:102)
        at java.lang.reflect.Field.setLong(Field.java:831)
        at FieldTrouble.main(FieldTrouble.java:11)

```

要消除此异常，问题行应替换为以下调用[`Field.set(Object obj, Object value)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#set-java.lang.Object-java.lang.Object-)：

```java
f.set(ft, new Integer(43));

```

* * *

**提示：** 当使用反射设置或获取字段时，编译器没有机会执行装箱。它只能转换由[`Class.isAssignableFrom()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isAssignableFrom-java.lang.Class-)规范描述的相关类型。该示例预计会失败，因为在此测试中`isAssignableFrom()`将返回`false`，可用于编程验证特定转换是否可能：

```java
Integer.class.isAssignableFrom(int.class) == false

```

同样，在反射中也不可能自动将原始类型转换为引用类型。

```java
int.class.isAssignableFrom(Integer.class) == false

```

* * *

## 针对非公共字段的`NoSuchFieldException`

机智的读者可能会注意到，如果之前展示的`FieldSpy`示例用于获取非公共字段的信息，它将失败：

```java
$ *java FieldSpy java.lang.String count*
java.lang.NoSuchFieldException: count
        at java.lang.Class.getField(Class.java:1519)
        at FieldSpy.main(FieldSpy.java:12)

```

* * *

**提示：** [`Class.getField()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getField-java.lang.String-) 和 [`Class.getFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getFields--) 方法返回由`Class`对象表示的类、枚举或接口的*公共*成员字段。要检索在`Class`中声明的所有字段（但不是继承的字段），请使用[`Class.getDeclaredFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredFields--) 方法。

* * *

## 修改最终字段时的`IllegalAccessException`

如果尝试获取或设置`private`或其他不可访问字段的值，或者设置`final`字段的值（无论其访问修饰符如何），可能会抛出[`IllegalAccessException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalAccessException.html)。

``FieldTroubleToo``示例展示了尝试设置 final 字段时产生的堆栈跟踪类型。

```java

import java.lang.reflect.Field;

public class FieldTroubleToo {
    public final boolean b = true;

    public static void main(String... args) {
	FieldTroubleToo ft = new FieldTroubleToo();
	try {
	    Class<?> c = ft.getClass();
	    Field f = c.getDeclaredField("b");
// 	    f.setAccessible(true);  // solution
	    f.setBoolean(ft, Boolean.FALSE);   // IllegalAccessException

        // production code should handle these exceptions more gracefully
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	} catch (IllegalArgumentException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}

```

```java
$ *java FieldTroubleToo*
java.lang.IllegalAccessException: Can not set final boolean field
  FieldTroubleToo.b to (boolean)false
        at sun.reflect.UnsafeFieldAccessorImpl.
          throwFinalFieldIllegalAccessException(UnsafeFieldAccessorImpl.java:55)
        at sun.reflect.UnsafeFieldAccessorImpl.
          throwFinalFieldIllegalAccessException(UnsafeFieldAccessorImpl.java:63)
        at sun.reflect.UnsafeQualifiedBooleanFieldAccessorImpl.setBoolean
          (UnsafeQualifiedBooleanFieldAccessorImpl.java:78)
        at java.lang.reflect.Field.setBoolean(Field.java:686)
        at FieldTroubleToo.main(FieldTroubleToo.java:12)

```

* * *

**提示：**存在访问限制，阻止在类初始化后设置`final`字段。但是，`Field`声明为扩展[`AccessibleObject`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html)，从而提供了抑制此检查的能力。

如果[`AccessibleObject.setAccessible()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html#setAccessible-boolean-)成功，那么对该字段值的后续操作将不会因此问题而失败。这可能会产生意想不到的副作用；例如，有时候原始值将继续被应用程序的某些部分使用，即使该值已被修改。只有在安全上下文允许的情况下，[`AccessibleObject.setAccessible()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html#setAccessible-boolean-)才会成功。

* * *
