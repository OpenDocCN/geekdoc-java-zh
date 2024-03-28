# 查找构造函数

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/ctorLocation.html`](https://docs.oracle.com/javase/tutorial/reflect/member/ctorLocation.html)

构造函数声明包括名称、修饰符、参数和可抛出异常列表。[`java.lang.reflect.Constructor`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html)类提供了获取这些信息的方法。

``ConstructorSift``示例演示了如何搜索一个类的声明构造函数中具有给定类型参数的构造函数。

```java

import java.lang.reflect.Constructor;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class ConstructorSift {
    public static void main(String... args) {
	try {
	    Class<?> cArg = Class.forName(args[1]);

	    Class<?> c = Class.forName(args[0]);
	    Constructor[] allConstructors = c.getDeclaredConstructors();
	    for (Constructor ctor : allConstructors) {
		Class<?>[] pType  = ctor.getParameterTypes();
		for (int i = 0; i < pType.length; i++) {
		    if (pType[i].equals(cArg)) {
			out.format("%s%n", ctor.toGenericString());

			Type[] gpType = ctor.getGenericParameterTypes();
			for (int j = 0; j < gpType.length; j++) {
			    char ch = (pType[j].equals(cArg) ? '*' : ' ');
			    out.format("%7c%s[%d]: %s%n", ch,
				       "GenericParameterType", j, gpType[j]);
			}
			break;
		    }
		}
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

[`Method.getGenericParameterTypes()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#getGenericParameterTypes--)将在类文件中的签名属性中查找（如果存在）。如果属性不可用，则会回退到[`Method.getParameterType()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#getParameterType--)，这个方法在引入泛型之前并未更改。其他以`getGeneric*Foo*()`命名的反射方法也是类似实现的。`Method.get*Types()`返回值的语法在[`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--)中有描述。

这里是所有在[`java.util.Formatter`](https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html)中具有[`Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html)参数的构造函数的输出。

```java
$ *java ConstructorSift java.util.Formatter java.util.Locale*
public
java.util.Formatter(java.io.OutputStream,java.lang.String,java.util.Locale)
throws java.io.UnsupportedEncodingException
       GenericParameterType[0]: class java.io.OutputStream
       GenericParameterType[1]: class java.lang.String
      *GenericParameterType[2]: class java.util.Locale
public java.util.Formatter(java.lang.String,java.lang.String,java.util.Locale)
throws java.io.FileNotFoundException,java.io.UnsupportedEncodingException
       GenericParameterType[0]: class java.lang.String
       GenericParameterType[1]: class java.lang.String
      *GenericParameterType[2]: class java.util.Locale
public java.util.Formatter(java.lang.Appendable,java.util.Locale)
       GenericParameterType[0]: interface java.lang.Appendable
      *GenericParameterType[1]: class java.util.Locale
public java.util.Formatter(java.util.Locale)
      *GenericParameterType[0]: class java.util.Locale
public java.util.Formatter(java.io.File,java.lang.String,java.util.Locale)
throws java.io.FileNotFoundException,java.io.UnsupportedEncodingException
       GenericParameterType[0]: class java.io.File
       GenericParameterType[1]: class java.lang.String
      *GenericParameterType[2]: class java.util.Locale

```

下一个示例输出演示了如何在[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)中搜索类型为`char[]`的参数。

```java
$ *java ConstructorSift java.lang.String "[C"*
java.lang.String(int,int,char[])
       GenericParameterType[0]: int
       GenericParameterType[1]: int
      *GenericParameterType[2]: class [C
public java.lang.String(char[],int,int)
      *GenericParameterType[0]: class [C
       GenericParameterType[1]: int
       GenericParameterType[2]: int
public java.lang.String(char[])
      *GenericParameterType[0]: class [C

```

表达接受[`Class.forName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#forName-java.lang.String-)的引用和基本类型数组的语法在[`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--)中有描述。请注意，第一个列出的构造函数是`包私有`的，而不是`public`的。它被返回是因为示例代码使用了[`Class.getDeclaredConstructors()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructors--)而不是[`Class.getConstructors()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructors--)，后者只返回`public`构造函数。

这个示例表明，搜索可变参数的参数（具有可变数量的参数）需要使用数组语法：

```java
$ *java ConstructorSift java.lang.ProcessBuilder "[Ljava.lang.String;"*
public java.lang.ProcessBuilder(java.lang.String[])
      *GenericParameterType[0]: class [Ljava.lang.String;

```

这是源代码中[`ProcessBuilder`](https://docs.oracle.com/javase/8/docs/api/java/lang/ProcessBuilder.html#ProcessBuilder-java.lang.String...-)构造函数的实际声明：

```java
public ProcessBuilder(String... command)

```

参数表示为类型为`java.lang.String`的单维数组。可以通过调用[`Constructor.isVarArgs()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#isVarArgs--)来区分明确为`java.lang.String`数组的参数。

最后一个示例报告了已声明具有泛型参数类型的构造函数的输出：

```java
$ *java ConstructorSift java.util.HashMap java.util.Map*
public java.util.HashMap(java.util.Map<? extends K, ? extends V>)
      *GenericParameterType[0]: java.util.Map<? extends K, ? extends V>

```

与方法类似，可以以类似的方式检索构造函数的异常类型。有关更多详细信息，请参见``MethodSpy``示例中描述的获取方法类型信息部分。
