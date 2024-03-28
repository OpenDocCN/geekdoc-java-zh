# 故障排除

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/ctorTrouble.html`](https://docs.oracle.com/javase/tutorial/reflect/member/ctorTrouble.html)

开发人员在尝试通过反射调用构造函数时，有时会遇到以下问题。

## 由于缺少零参数构造函数而导致的 InstantiationException

`ConstructorTrouble` 示例说明了当代码尝试使用[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)创建类的新实例时，且没有可访问的零参数构造函数时会发生什么：

```java

public class ConstructorTrouble {
    private ConstructorTrouble(int i) {}

    public static void main(String... args){
	try {
	    Class<?> c = Class.forName("ConstructorTrouble");
	    Object o = c.newInstance();  // InstantiationException

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

```java
$ *java ConstructorTrouble*
java.lang.InstantiationException: ConstructorTrouble
        at java.lang.Class.newInstance0(Class.java:340)
        at java.lang.Class.newInstance(Class.java:308)
        at ConstructorTrouble.main(ConstructorTrouble.java:7)

```

* * *

**提示：**[`InstantiationException`](https://docs.oracle.com/javase/8/docs/api/java/lang/InstantiationException.html)可能发生的原因有很多。在这种情况下，问题在于具有`int`参数的构造函数的存在阻止了编译器生成默认（或零参数）构造函数，并且代码中没有显式的零参数构造函数。请记住，[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)的行为非常类似于`new`关键字，只要`new`失败，它就会失败。

* * *

## Class.newInstance() 抛出意外异常

`ConstructorTroubleToo` 示例展示了在[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)中出现的无法解决的问题。即，它传播构造函数抛出的任何异常（已检查或未检查）。

```java

import java.lang.reflect.InvocationTargetException;
import static java.lang.System.err;

public class ConstructorTroubleToo {
    public ConstructorTroubleToo() {
 	throw new RuntimeException("exception in constructor");
    }

    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName("ConstructorTroubleToo");
	    // Method propagetes any exception thrown by the constructor
	    // (including checked exceptions).
	    if (args.length > 0 && args[0].equals("class")) {
		Object o = c.newInstance();
	    } else {
		Object o = c.getConstructor().newInstance();
	    }

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (NoSuchMethodException x) {
	    x.printStackTrace();
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	    err.format("%n%nCaught exception: %s%n", x.getCause());
	}
    }
}

```

```java
$ *java ConstructorTroubleToo class*
Exception in thread "main" java.lang.RuntimeException: exception in constructor
        at ConstructorTroubleToo.<init>(ConstructorTroubleToo.java:6)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance
          (NativeConstructorAccessorImpl.java:39)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance
          (DelegatingConstructorAccessorImpl.java:27)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:513)
        at java.lang.Class.newInstance0(Class.java:355)
        at java.lang.Class.newInstance(Class.java:308)
        at ConstructorTroubleToo.main(ConstructorTroubleToo.java:15)

```

这种情况是反射独有的。通常情况下，不可能编写忽略已检查异常的代码，因为这样的代码不会编译。可以通过使用[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-)而不是[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)来包装构造函数抛出的任何异常。

```java
$ *java ConstructorTroubleToo*
java.lang.reflect.InvocationTargetException
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance
          (NativeConstructorAccessorImpl.java:39)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance
          (DelegatingConstructorAccessorImpl.java:27)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:513)
        at ConstructorTroubleToo.main(ConstructorTroubleToo.java:17)
Caused by: java.lang.RuntimeException: exception in constructor
        at ConstructorTroubleToo.<init>(ConstructorTroubleToo.java:6)
        ... 5 more

Caught exception: java.lang.RuntimeException: exception in constructor

```

如果抛出[`InvocationTargetException`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html)，则表示方法已被调用。对问题的诊断与直接调用构造函数并抛出异常，然后通过[`InvocationTargetException.getCause()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html#getCause--)检索到的异常相同。此异常并不表示反射包或其使用存在问题。

* * *

**提示：**最好使用[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-)而不是[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)，因为前者的 API 允许检查和处理构造函数抛出的任意异常。

* * *

## 定位或调用正确构造函数的问题

`ConstructorTroubleAgain`类展示了代码错误可能无法定位或调用预期构造函数的各种方式。

```java

import java.lang.reflect.InvocationTargetException;
import static java.lang.System.out;

public class ConstructorTroubleAgain {
    public ConstructorTroubleAgain() {}

    public ConstructorTroubleAgain(Integer i) {}

    public ConstructorTroubleAgain(Object o) {
	out.format("Constructor passed Object%n");
    }

    public ConstructorTroubleAgain(String s) {
	out.format("Constructor passed String%n");
    }

    public static void main(String... args){
	String argType = (args.length == 0 ? "" : args[0]);
	try {
	    Class<?> c = Class.forName("ConstructorTroubleAgain");
	    if ("".equals(argType)) {
		// IllegalArgumentException: wrong number of arguments
		Object o = c.getConstructor().newInstance("foo");
	    } else if ("int".equals(argType)) {
		// NoSuchMethodException - looking for int, have Integer
		Object o = c.getConstructor(int.class);
	    } else if ("Object".equals(argType)) {
		// newInstance() does not perform method resolution
		Object o = c.getConstructor(Object.class).newInstance("foo");
	    } else {
		assert false;
	    }

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	} catch (NoSuchMethodException x) {
	    x.printStackTrace();
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}

```

```java
$ *java ConstructorTroubleAgain*
Exception in thread "main" java.lang.IllegalArgumentException: wrong number of
  arguments
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance
          (NativeConstructorAccessorImpl.java:39)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance
          (DelegatingConstructorAccessorImpl.java:27)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:513)
        at ConstructorTroubleAgain.main(ConstructorTroubleAgain.java:23)

```

抛出[`IllegalArgumentException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html)是因为请求零参数构造函数并尝试传递参数。如果构造函数传递了错误类型的参数，也会抛出相同的异常。

```java
$ *java ConstructorTroubleAgain int*
java.lang.NoSuchMethodException: ConstructorTroubleAgain.<init>(int)
        at java.lang.Class.getConstructor0(Class.java:2706)
        at java.lang.Class.getConstructor(Class.java:1657)
        at ConstructorTroubleAgain.main(ConstructorTroubleAgain.java:26)

```

如果开发人员错误地认为反射会自动装箱或拆箱类型，则可能会出现此异常。装箱（将原始类型转换为引用类型）仅在编译期间发生。在反射中没有机会进行此操作，因此在定位构造函数时必须使用特定类型。

```java
$ *java ConstructorTroubleAgain Object*
Constructor passed Object

```

在这里，可能期望调用接受[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)参数的构造函数，因为使用了更具体的`String`类型调用了`newInstance()`。然而，为时已晚！找到的构造函数已经是接受[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)参数的构造函数。`newInstance()`不会尝试进行方法解析；它只是在现有构造函数对象上操作。

* * *

**提示：** `new`和[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-)之间的一个重要区别是，`new`执行方法参数类型检查、装箱和方法解析。在反射中，这些都不会发生，必须做出明确选择。

* * *

## 尝试调用不可访问构造函数时出现 IllegalAccessException

如果尝试调用私有或其他不可访问的构造函数，则可能会抛出[`IllegalAccessException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalAccessException.html)。`ConstructorTroubleAccess`示例展示了产生的堆栈跟踪。

```java

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

class Deny {
    private Deny() {
	System.out.format("Deny constructor%n");
    }
}

public class ConstructorTroubleAccess {
    public static void main(String... args) {
	try {
	    Constructor c = Deny.class.getDeclaredConstructor();
//  	    c.setAccessible(true);   // solution
	    c.newInstance();

        // production code should handle these exceptions more gracefully
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	} catch (NoSuchMethodException x) {
	    x.printStackTrace();
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}

```

```java
$ *java ConstructorTroubleAccess*
java.lang.IllegalAccessException: Class ConstructorTroubleAccess can not access
  a member of class Deny with modifiers "private"
        at sun.reflect.Reflection.ensureMemberAccess(Reflection.java:65)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:505)
        at ConstructorTroubleAccess.main(ConstructorTroubleAccess.java:15)

```

* * *

**提示：** 存在访问限制，阻止通过直接调用无法访问的构造函数进行反射调用。（这包括但不限于在单独类中的私有构造函数和在单独私有类中的公共构造函数。）但是，`Constructor`被声明为扩展[`AccessibleObject`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html)，它提供了通过[`AccessibleObject.setAccessible()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html#setAccessible-boolean-)来抑制此检查的能力。

* * *
