# 检索和解析字段修饰符

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/fieldModifiers.html`](https://docs.oracle.com/javase/tutorial/reflect/member/fieldModifiers.html)

有几个修饰符可能是字段声明的一部分：

+   访问修饰符：`public`、`protected` 和 `private`

+   控制运行时行为的字段特定修饰符：`transient` 和 `volatile`

+   限制为一个实例的修饰符：`static`

+   禁止值修改的修饰符：`final`

+   注解

方法[`Field.getModifiers()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#getModifiers--)可用于返回表示字段的声明修饰符集合的整数。该整数中表示修饰符的位在[`java.lang.reflect.Modifier`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Modifier.html)中定义。

``FieldModifierSpy``示例演示了如何搜索具有给定修饰符的字段。它还通过调用[`Field.isSynthetic()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#isSynthetic--)和[`Field.isEnumCostant()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#isEnumConstant--)确定所定位的字段是合成的（编译器生成的）还是枚举常量。

```java

import java.lang.reflect.Field;
import java.lang.reflect.Modifier;
import static java.lang.System.out;

enum Spy { BLACK , WHITE }

public class FieldModifierSpy {
    volatile int share;
    int instance;
    class Inner {}

    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    int searchMods = 0x0;
	    for (int i = 1; i < args.length; i++) {
		searchMods |= modifierFromString(args[i]);
	    }

	    Field[] flds = c.getDeclaredFields();
	    out.format("Fields in Class '%s' containing modifiers:  %s%n",
		       c.getName(),
		       Modifier.toString(searchMods));
	    boolean found = false;
	    for (Field f : flds) {
		int foundMods = f.getModifiers();
		// Require all of the requested modifiers to be present
		if ((foundMods & searchMods) == searchMods) {
		    out.format("%-8s [ synthetic=%-5b enum_constant=%-5b ]%n",
			       f.getName(), f.isSynthetic(),
			       f.isEnumConstant());
		    found = true;
		}
	    }

	    if (!found) {
		out.format("No matching fields%n");
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static int modifierFromString(String s) {
	int m = 0x0;
	if ("public".equals(s))           m |= Modifier.PUBLIC;
	else if ("protected".equals(s))   m |= Modifier.PROTECTED;
	else if ("private".equals(s))     m |= Modifier.PRIVATE;
	else if ("static".equals(s))      m |= Modifier.STATIC;
	else if ("final".equals(s))       m |= Modifier.FINAL;
	else if ("transient".equals(s))   m |= Modifier.TRANSIENT;
	else if ("volatile".equals(s))    m |= Modifier.VOLATILE;
	return m;
    }
}

```

示例输出如下：

```java
$ *java FieldModifierSpy FieldModifierSpy volatile*
Fields in Class 'FieldModifierSpy' containing modifiers:  volatile
share    [ synthetic=false enum_constant=false ]

$ *java FieldModifierSpy Spy public*
Fields in Class 'Spy' containing modifiers:  public
BLACK    [ synthetic=false enum_constant=true  ]
WHITE    [ synthetic=false enum_constant=true  ]

$ *java FieldModifierSpy FieldModifierSpy\$Inner final*
Fields in Class 'FieldModifierSpy$Inner' containing modifiers:  final
this$0   [ synthetic=true  enum_constant=false ]

$ *java FieldModifierSpy Spy private static final*
Fields in Class 'Spy' containing modifiers:  private static final
$VALUES  [ synthetic=true  enum_constant=false ]

```

请注意，有些字段即使在原始代码中未声明也会被报告。这是因为编译器会生成一些*合成字段*，这些字段在运行时是必需的。为了测试一个字段是否是合成的，示例调用[`Field.isSynthetic()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#isSynthetic--)。合成字段的集合是依赖于编译器的；然而，常用的字段包括用于内部类（即非静态成员类）引用最外层封闭类的`this$0`和用于枚举实现隐式定义的静态方法`values()`的`$VALUES`。合成类成员的名称未指定，可能在所有编译器实现或版本中不同。这些和其他合成字段将包含在[`Class.getDeclaredFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredFields--)返回的数组中，但不会被[`Class.getField()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getField-java.lang.String-)识别，因为合成成员通常不是`public`。

因为[`Field`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html)实现了接口[`java.lang.reflect.AnnotatedElement`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html)，所以可以使用[`java.lang.annotation.RetentionPolicy.RUNTIME`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/RetentionPolicy.html#RUNTIME)来检索任何运行时注解。有关获取注解的示例，请参见检查类修饰符和类型部分。
