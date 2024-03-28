# 故障排除

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/methodTrouble.html`](https://docs.oracle.com/javase/tutorial/reflect/member/methodTrouble.html)

本节包含开发人员在使用反射定位、调用或获取方法时可能遇到的问题示例。

## 由于类型擦除导致的 NoSuchMethodException

``MethodTrouble``示例说明了当代码在类中搜索特定方法时未考虑类型擦除时会发生什么。

```java

import java.lang.reflect.Method;

public class MethodTrouble<T>  {
    public void lookup(T t) {}
    public void find(Integer i) {}

    public static void main(String... args) {
	try {
	    String mName = args[0];
	    Class cArg = Class.forName(args[1]);
	    Class<?> c = (new MethodTrouble<Integer>()).getClass();
	    Method m = c.getMethod(mName, cArg);
	    System.out.format("Found:%n  %s%n", m.toGenericString());

        // production code should handle these exceptions more gracefully
	} catch (NoSuchMethodException x) {
	    x.printStackTrace();
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

```java
$ *java MethodTrouble lookup java.lang.Integer*
java.lang.NoSuchMethodException: MethodTrouble.lookup(java.lang.Integer)
        at java.lang.Class.getMethod(Class.java:1605)
        at MethodTrouble.main(MethodTrouble.java:12)

```

```java
$ *java MethodTrouble lookup java.lang.Object*
Found:
  public void MethodTrouble.lookup(T)

```

当方法声明具有泛型参数类型时，编译器将使用其上界替换泛型类型，在本例中，`T`的上界为[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)。因此，当代码搜索`lookup(Integer)`时，尽管`MethodTrouble`的实例是如下创建的，但不会找到方法：

```java
Class<?> c = (new MethodTrouble<Integer>()).getClass();

```

搜索`lookup(Object)`成功，如预期。

```java
$ *java MethodTrouble find java.lang.Integer*
Found:
  public void MethodTrouble.find(java.lang.Integer)
$ *java MethodTrouble find java.lang.Object*
java.lang.NoSuchMethodException: MethodTrouble.find(java.lang.Object)
        at java.lang.Class.getMethod(Class.java:1605)
        at MethodTrouble.main(MethodTrouble.java:12)

```

在这种情况下，`find()`没有泛型参数，因此[`getMethod()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethod-java.lang.String-java.lang.Class...-)搜索的参数类型必须完全匹配。

* * *

**提示：** 在搜索方法时，始终传递参数化类型的上界。

* * *

## 调用方法时的 IllegalAccessException

如果尝试调用`private`或其他不可访问方法，则会抛出[`IllegalAccessException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalAccessException.html)。

``MethodTroubleAgain``示例展示了尝试在另一个类中调用私有方法导致的典型堆栈跟踪。

```java

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

class AnotherClass {
    private void m() {}
}

public class MethodTroubleAgain {
    public static void main(String... args) {
	AnotherClass ac = new AnotherClass();
	try {
	    Class<?> c = ac.getClass();
 	    Method m = c.getDeclaredMethod("m");
//  	    m.setAccessible(true);      // solution
 	    Object o = m.invoke(ac);    // IllegalAccessException

        // production code should handle these exceptions more gracefully
	} catch (NoSuchMethodException x) {
	    x.printStackTrace();
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}

```

异常抛出的堆栈跟踪如下。

```java
$ *java MethodTroubleAgain*
java.lang.IllegalAccessException: Class MethodTroubleAgain can not access a
  member of class AnotherClass with modifiers "private"
        at sun.reflect.Reflection.ensureMemberAccess(Reflection.java:65)
        at java.lang.reflect.Method.invoke(Method.java:588)
        at MethodTroubleAgain.main(MethodTroubleAgain.java:15)

```

* * *

**提示：** 存在访问限制，阻止对通常无法通过直接调用访问的方法进行反射调用。（包括但不限于在另一个类中的`private`方法和在另一个私有类中的公共方法。）但是，`Method`被声明为扩展[`AccessibleObject`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html)，通过[`AccessibleObject.setAccessible()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html#setAccessible-boolean-)提供了抑制此检查的能力。如果成功，则随后对该方法对象的调用不会因此问题而失败。

* * *

## 从 Method.invoke()中抛出的 IllegalArgumentException

[`Method.invoke()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#invoke-java.lang.Object-java.lang.Object...-)已经被改造成为可变参数方法。这是一个巨大的便利，但可能会导致意外行为。``MethodTroubleToo``示例展示了[`Method.invoke()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#invoke-java.lang.Object-java.lang.Object...-)可能产生混乱结果的各种方式。

```java

import java.lang.reflect.Method;

public class MethodTroubleToo {
    public void ping() { System.out.format("PONG!%n"); }

    public static void main(String... args) {
	try {
	    MethodTroubleToo mtt = new MethodTroubleToo();
	    Method m = MethodTroubleToo.class.getMethod("ping");

 	    switch(Integer.parseInt(args[0])) {
	    case 0:
  		m.invoke(mtt);                 // works
		break;
	    case 1:
 		m.invoke(mtt, null);           // works (expect compiler warning)
		break;
	    case 2:
		Object arg2 = null;
		m.invoke(mtt, arg2);           // IllegalArgumentException
		break;
	    case 3:
		m.invoke(mtt, new Object[0]);  // works
		break;
	    case 4:
		Object arg4 = new Object[0];
		m.invoke(mtt, arg4);           // IllegalArgumentException
		break;
	    default:
		System.out.format("Test not found%n");
	    }

        // production code should handle these exceptions more gracefully
	} catch (Exception x) {
	    x.printStackTrace();
	}
    }
}

```

```java
$ *java MethodTroubleToo 0*
PONG!

```

由于[`Method.invoke()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#invoke-java.lang.Object-java.lang.Object...-)的所有参数都是可选的，除了第一个，当要调用的方法没有参数时，可以省略它们。

```java
$ *java MethodTroubleToo 1*
PONG!

```

在这种情况下，代码生成这个编译器警告，因为`null`是模棱两可的。

```java
$ *javac MethodTroubleToo.java*
MethodTroubleToo.java:16: warning: non-varargs call of varargs method with
  inexact argument type for last parameter;
 		m.invoke(mtt, null);           // works (expect compiler warning)
 		              ^
  cast to Object for a varargs call
  cast to Object[] for a non-varargs call and to suppress this warning
1 warning

```

不可能确定`null`代表空参数数组还是第一个参数为`null`。

```java
$ *java MethodTroubleToo 2*
java.lang.IllegalArgumentException: wrong number of arguments
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke
          (NativeMethodAccessorImpl.java:39)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke
          (DelegatingMethodAccessorImpl.java:25)
        at java.lang.reflect.Method.invoke(Method.java:597)
        at MethodTroubleToo.main(MethodTroubleToo.java:21)

```

尽管参数为`null`，但这会失败，因为类型是[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)，而`ping()`不需要任何参数。

```java
$ *java MethodTroubleToo 3*
PONG!

```

这能够成功是因为`new Object[0]`创建了一个空数组，对于可变参数方法来说，这等同于不传递任何可选参数。

```java
$ *java MethodTroubleToo 4*
java.lang.IllegalArgumentException: wrong number of arguments
        at sun.reflect.NativeMethodAccessorImpl.invoke0
          (Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke
          (NativeMethodAccessorImpl.java:39)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke
          (DelegatingMethodAccessorImpl.java:25)
        at java.lang.reflect.Method.invoke(Method.java:597)
        at MethodTroubleToo.main(MethodTroubleToo.java:28)

```

与前面的例子不同，如果空数组存储在一个[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)中，那么它将被视为一个[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)。这与案例 2 失败的原因相同，`ping()`不期望有参数。

* * *

**提示：**当声明一个方法`foo(Object... o)`时，编译器会将传递给`foo()`的所有参数放入一个[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)类型的数组中。`foo()`的实现与声明为`foo(Object[] o)`时相同。理解这一点可能有助于避免上面所示问题的类型。

* * *

## 调用方法失败时的 InvocationTargetException

[`InvocationTargetException`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html)包装了调用方法对象时产生的所有异常（已检查和未检查）。``MethodTroubleReturns``示例展示了如何检索被调用方法抛出的原始异常。

```java

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class MethodTroubleReturns {
    private void drinkMe(int liters) {
	if (liters < 0)
	    throw new IllegalArgumentException("I can't drink a negative amount of liquid");
    }

    public static void main(String... args) {
	try {
	    MethodTroubleReturns mtr  = new MethodTroubleReturns();
 	    Class<?> c = mtr.getClass();
   	    Method m = c.getDeclaredMethod("drinkMe", int.class);
	    m.invoke(mtr, -1);

        // production code should handle these exceptions more gracefully
	} catch (InvocationTargetException x) {
	    Throwable cause = x.getCause();
	    System.err.format("drinkMe() failed: %s%n", cause.getMessage());
	} catch (Exception x) {
	    x.printStackTrace();
	}
    }
}

```

```java
$ *java MethodTroubleReturns*
drinkMe() failed: I can't drink a negative amount of liquid

```

* * *

**提示：**如果抛出[`InvocationTargetException`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html)，则方法已被调用。诊断问题的方法与直接调用方法并通过[`getCause()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html#getCause--)检索到的异常相同。此异常并不表示反射包或其使用存在问题。

* * *
