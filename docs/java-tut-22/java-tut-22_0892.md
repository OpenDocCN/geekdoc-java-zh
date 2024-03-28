# 故障排除

> 原文：[`docs.oracle.com/javase/tutorial/reflect/class/classTrouble.html`](https://docs.oracle.com/javase/tutorial/reflect/class/classTrouble.html)

以下示例展示了在反射类时可能遇到的典型错误。

## 编译器警告："注意：...使用了未经检查或不安全的操作"

当调用方法时，会检查参数值的类型并可能进行转换。`ClassWarning`调用[`getMethod()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethod-java.lang.String-java.lang.Class...-)会导致典型的未经检查的转换警告：

```java

import java.lang.reflect.Method;

public class ClassWarning {
    void m() {
	try {
	    Class c = ClassWarning.class;
	    Method m = c.getMethod("m");  // warning

        // production code should handle this exception more gracefully
	} catch (NoSuchMethodException x) {
    	    x.printStackTrace();
    	}
    }
}

```

```java
$ *javac ClassWarning.java*
Note: ClassWarning.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
$ *javac -Xlint:unchecked ClassWarning.java*
ClassWarning.java:6: warning: [unchecked] unchecked call to getMethod
  (String,Class<?>...) as a member of the raw type Class
Method m = c.getMethod("m");  // warning
                      ^
1 warning

```

许多库方法已经使用了泛型声明，包括[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)中的几个方法。由于`c`声明为*原始*类型（没有类型参数），并且[`getMethod()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethod-java.lang.String-java.lang.Class...-)的相应参数是参数化类型，因此会发生未经检查的转换。编译器需要生成警告。（参见[*Java 语言规范，Java SE 7 版*](https://docs.oracle.com/javase/specs/jls/se7/html/index.html)，章节[未经检查的转换](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.1.9)和[方法调用转换](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.3)。）

有两种可能的解决方案。更可取的是修改`c`的声明以包含适当的通用类型。在这种情况下，声明应为：

```java
Class<?> c = warn.getClass();

```

或者，可以在有问题的语句之前使用预定义的注释[`@SuppressWarnings`](https://docs.oracle.com/javase/8/docs/api/java/lang/SuppressWarnings.html)来明确抑制警告。

```java
Class c = ClassWarning.class;
@SuppressWarnings("unchecked")
Method m = c.getMethod("m");  
// warning gone

```

* * *

**提示：** 作为一个一般原则，不应忽略警告，因为它们可能表明存在错误。应适当使用参数化声明。如果不可能（也许是因为应用程序必须与库供应商的代码交互），则可以使用[`@SuppressWarnings`](https://docs.oracle.com/javase/8/docs/api/java/lang/SuppressWarnings.html)对有问题的行进行注释。

* * *

## 当构造函数不可访问时会出现 InstantiationException

[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)如果尝试创建一个类的新实例且零参数构造函数不可见，则会抛出[`InstantiationException`](https://docs.oracle.com/javase/8/docs/api/java/lang/InstantiationException.html)。``ClassTrouble``示例展示了生成的堆栈跟踪。

```java

class Cls {
    private Cls() {}
}

public class ClassTrouble {
    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName("Cls");
	    c.newInstance();  // InstantiationException

        // production code should handle these exceptions more gracefully
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

```java
$ *java ClassTrouble*
java.lang.IllegalAccessException: Class ClassTrouble can not access a member of
  class Cls with modifiers "private"
        at sun.reflect.Reflection.ensureMemberAccess(Reflection.java:65)
        at java.lang.Class.newInstance0(Class.java:349)
        at java.lang.Class.newInstance(Class.java:308)
        at ClassTrouble.main(ClassTrouble.java:9)

```

[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--)的行为非常类似于`new`关键字，并且会因为与`new`相同的原因而失败。在反射中的典型解决方案是利用[`java.lang.reflect.AccessibleObject`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html)类，该类提供了抑制访问控制检查的能力；然而，这种方法不起作用，因为[`java.lang.Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)不扩展[`AccessibleObject`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html)。唯一的解决方案是修改代码以使用[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-)，该方法确实扩展了[`AccessibleObject`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html)。

* * *

**提示：** 一般来说，最好使用[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-)，原因在创建新类实例部分的成员课程中有描述。

* * *

使用[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-)可能会出现潜在问题的其他示例，可以在构造函数故障排除部分的成员课程中找到。
