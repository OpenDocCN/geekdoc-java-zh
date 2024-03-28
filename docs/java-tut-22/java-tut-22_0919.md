# 故障排除

> 原文：[`docs.oracle.com/javase/tutorial/reflect/special/enumTrouble.html`](https://docs.oracle.com/javase/tutorial/reflect/special/enumTrouble.html)

以下示例展示了在使用枚举类型时可能遇到的问题。

## 尝试实例化枚举类型时出现 IllegalArgumentException

正如前面提到的，实例化枚举类型是被禁止的。``EnumTrouble``示例尝试这样做。

```java

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import static java.lang.System.out;

enum Charge {
    POSITIVE, NEGATIVE, NEUTRAL;
    Charge() {
	out.format("under construction%n");
    }
}

public class EnumTrouble {

    public static void main(String... args) {
	try {
	    Class<?> c = Charge.class;

 	    Constructor[] ctors = c.getDeclaredConstructors();
 	    for (Constructor ctor : ctors) {
		out.format("Constructor: %s%n",  ctor.toGenericString());
 		ctor.setAccessible(true);
 		ctor.newInstance();
 	    }

        // production code should handle these exceptions more gracefully
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	}
    }
}

```

```java
$ *java EnumTrouble*
Constructor: private Charge()
Exception in thread "main" java.lang.IllegalArgumentException: Cannot
  reflectively create enum objects
        at java.lang.reflect.Constructor.newInstance(Constructor.java:511)
        at EnumTrouble.main(EnumTrouble.java:22)

```

* * *

**提示：** 明确实例化枚举是一种编译时错误，因为这将阻止定义的枚举常量保持唯一。这种限制也在反射代码中执行。试图使用默认构造函数实例化类的代码应该首先调用[`Class.isEnum()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isEnum--)来确定该类是否为枚举。

* * *

## 设置具有不兼容枚举类型的字段时出现 IllegalArgumentException

存储枚举的字段应该设置为适当的枚举类型。（实际上，*任何*类型的字段都必须设置为兼容的类型。）``EnumTroubleToo``示例会产生预期的错误。

```java

import java.lang.reflect.Field;

enum E0 { A, B }
enum E1 { A, B }

class ETest {
    private E0 fld = E0.A;
}

public class EnumTroubleToo {
    public static void main(String... args) {
	try {
	    ETest test = new ETest();
	    Field f = test.getClass().getDeclaredField("fld");
	    f.setAccessible(true);
 	    f.set(test, E1.A);  // IllegalArgumentException

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
$ *java EnumTroubleToo*
Exception in thread "main" java.lang.IllegalArgumentException: Can not set E0
  field ETest.fld to E1
        at sun.reflect.UnsafeFieldAccessorImpl.throwSetIllegalArgumentException
          (UnsafeFieldAccessorImpl.java:146)
        at sun.reflect.UnsafeFieldAccessorImpl.throwSetIllegalArgumentException
          (UnsafeFieldAccessorImpl.java:150)
        at sun.reflect.UnsafeObjectFieldAccessorImpl.set
          (UnsafeObjectFieldAccessorImpl.java:63)
        at java.lang.reflect.Field.set(Field.java:657)
        at EnumTroubleToo.main(EnumTroubleToo.java:16)

```

* * *

**提示：** 严格来说，将类型为`X`的字段设置为类型为`Y`的值只有在以下语句成立时才能成功：

```java
X.class.isAssignableFrom(Y.class) == true

```

代码可以修改以执行以下测试，以验证类型是否兼容：

```java
if (f.getType().isAssignableFrom(E0.class))
    // compatible
else
    // expect IllegalArgumentException

```

* * *
