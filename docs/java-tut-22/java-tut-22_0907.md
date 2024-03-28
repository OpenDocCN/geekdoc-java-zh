# 检索和解析构造函数修饰符

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/ctorModifiers.html`](https://docs.oracle.com/javase/tutorial/reflect/member/ctorModifiers.html)

由于构造函数在语言中的作用，比方法更少的修饰符是有意义的：

+   访问修饰符：`public`，`protected`和`private`

+   注解

``ConstructorAccess``示例在给定类中搜索具有指定访问修饰符的构造函数。它还显示构造函数是否是合成的（由编译器生成）或具有可变参数。

```java

import java.lang.reflect.Constructor;
import java.lang.reflect.Modifier;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class ConstructorAccess {
    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    Constructor[] allConstructors = c.getDeclaredConstructors();
	    for (Constructor ctor : allConstructors) {
		int searchMod = modifierFromString(args[1]);
		int mods = accessModifiers(ctor.getModifiers());
		if (searchMod == mods) {
		    out.format("%s%n", ctor.toGenericString());
		    out.format("  [ synthetic=%-5b var_args=%-5b ]%n",
			       ctor.isSynthetic(), ctor.isVarArgs());
		}
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static int accessModifiers(int m) {
	return m & (Modifier.PUBLIC | Modifier.PRIVATE | Modifier.PROTECTED);
    }

    private static int modifierFromString(String s) {
	if ("public".equals(s))               return Modifier.PUBLIC;
	else if ("protected".equals(s))       return Modifier.PROTECTED;
	else if ("private".equals(s))         return Modifier.PRIVATE;
	else if ("package-private".equals(s)) return 0;
	else return -1;
    }
}

```

没有明确对应于“包私有”访问权限的[`Modifier`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Modifier.html)常量，因此需要检查所有三个访问修饰符的缺失来识别包私有构造函数。

此输出显示了[`java.io.File`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html)中的私有构造函数：

```java
$ *java ConstructorAccess java.io.File private*
private java.io.File(java.lang.String,int)
  [ synthetic=false var_args=false ]
private java.io.File(java.lang.String,java.io.File)
  [ synthetic=false var_args=false ]

```

合成构造函数很少见；但是``SyntheticConstructor``示例说明了可能发生这种情况的典型情况：

```java

public class SyntheticConstructor {
    private SyntheticConstructor() {}
    class Inner {
	// Compiler will generate a synthetic constructor since
	// SyntheticConstructor() is private.
	Inner() { new SyntheticConstructor(); }
    }
}

```

```java
$ *java ConstructorAccess SyntheticConstructor package-private*
SyntheticConstructor(SyntheticConstructor$1)
  [ synthetic=true  var_args=false ]

```

由于内部类的构造函数引用了封闭类的私有构造函数，编译器必须生成一个包私有构造函数。参数类型`SyntheticConstructor$1`是任意的，取决于编译器的实现。依赖于任何合成或非公共类成员存在的代码可能不具有可移植性。

构造函数实现了[`java.lang.reflect.AnnotatedElement`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html)，提供了用于检索运行时注解的方法，使用[`java.lang.annotation.RetentionPolicy.RUNTIME`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/RetentionPolicy.html#RUNTIME)。有关获取注解的示例，请参见检查类修饰符和类型部分。
