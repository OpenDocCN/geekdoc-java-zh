# 检索类对象

> 原文：[`docs.oracle.com/javase/tutorial/reflect/class/classNew.html`](https://docs.oracle.com/javase/tutorial/reflect/class/classNew.html)

所有反射操作的入口点是[`java.lang.Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。除了[`java.lang.reflect.ReflectPermission`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/ReflectPermission.html)之外，[`java.lang.reflect`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/package-summary.html)中的类都没有公共构造函数。要访问这些类，需要在[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)上调用适当的方法。有几种方法可以获取[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)，具体取决于代码是否可以访问对象、类的名称、类型或现有的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。

## `Object.getClass()`

如果对象的实例可用，则获取其[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)的最简单方法是调用[`Object.getClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--)。当然，这仅适用于所有继承自[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)的引用类型。以下是一些示例。

```java
Class c = "foo".getClass();

```

返回[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。

```java
Class c = System.console().getClass();

```

与虚拟机关联的唯一控制台由`static`方法[`System.console()`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#console--)返回。[`getClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--)返回的值是对应于[`java.io.Console`](https://docs.oracle.com/javase/8/docs/api/java/io/Console.html)的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。

```java
enum E { A, B }
Class c = A.getClass();

```

`A`是枚举`E`的一个实例；因此[`getClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--)返回对应于枚举类型`E`的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。

```java
byte[] bytes = new byte[1024];
Class c = bytes.getClass();

```

由于数组是[`Objects`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)，因此也可以在数组实例上调用[`getClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--)。返回的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)对应于具有组件类型`byte`的数组。

```java
import java.util.HashSet;
import java.util.Set;

Set<String> s = new HashSet<String>();
Class c = s.getClass();

```

在这种情况下，[`java.util.Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)是一个指向类型为[`java.util.HashSet`](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html)的对象的接口。[`getClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--)返回的值是与[`java.util.HashSet`](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html)对应的类。

## .class 语法

如果类型可用但没有实例，则可以通过在类型名称后附加`".class"`来获得一个[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。这也是获取原始类型对应的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)的最简单方式。

```java
boolean b;
Class c = b.getClass();   // compile-time error

Class c = boolean.class;  // correct

```

请注意，语句`boolean.getClass()`会产生编译时错误，因为`boolean`是原始类型，不能被解引用。`.class`语法返回与类型`boolean`对应的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。

```java
Class c = java.io.PrintStream.class;

```

变量`c`将是与类型[`java.io.PrintStream`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html)对应的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。

```java
Class c = int[][][].class;

```

`.class`语法可用于检索与给定类型的多维数组对应的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。

## Class.forName()

如果类的完全限定名称可用，则可以使用静态方法[`Class.forName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#forName-java.lang.String-)获取相应的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。这不能用于原始类型。数组类名称的语法由[`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--)描述。此语法适用于引用和原始类型。

```java
Class c = Class.forName("com.duke.MyLocaleServiceProvider");

```

此语句将根据给定的完全限定名称创建一个类。

```java
Class cDoubleArray = Class.forName("[D");

Class cStringArray = Class.forName("[[Ljava.lang.String;");

```

变量`cDoubleArray`将包含与原始类型`double`的数组对应的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)（即与`double[].class`相同）。变量`cStringArray`将包含与[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)的二维数组对应的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)（即与`String[][].class`相同）。

## 原始类型包装器的 TYPE 字段

`.class` 语法是获取原始类型的 [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) 更方便且更常用的方式；然而还有另一种获取 [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) 的方法。每种原始类型和 `void` 在 [`java.lang`](https://docs.oracle.com/javase/8/docs/api/java/lang/package-summary.html) 中都有一个包装类，用于将原始类型装箱为引用类型。每个包装类都包含一个名为 `TYPE` 的字段，该字段等于被包装的原始类型的 [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。

```java
Class c = Double.TYPE;

```

存在一个类 [`java.lang.Double`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html) 用于包装原始类型 `double`，每当需要一个 [`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) 时。[`Double.TYPE`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html#TYPE) 的值与 `double.class` 相同。

```java
Class c = Void.TYPE;

```

[`Void.TYPE`](https://docs.oracle.com/javase/8/docs/api/java/lang/Void.html#TYPE) 与 `void.class` 相同。

## 返回类的方法

有几个反射 API 返回类，但只有在已经直接或间接地获取了 [`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) 后才能访问这些类。

[`Class.getSuperclass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getSuperclass--)

返回给定类的超类。

```java
Class c = javax.swing.JButton.class.getSuperclass();

```

[`javax.swing.JButton`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JButton.html) 的超类是 [`javax.swing.AbstractButton`](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html)。

[`Class.getClasses()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getClasses--)

返回类的所有公共类、接口和枚举，包括继承的成员。

```java
Class<?>[] c = Character.class.getClasses();

```

[`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html) 包含两个成员类 [`Character.Subset`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.Subset.html) 和 [`Character.UnicodeBlock`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.UnicodeBlock.html)。

[`Class.getDeclaredClasses()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredClasses--)

返回在此类中显式声明的所有类、接口和枚举。

```java
Class<?>[] c = Character.class.getDeclaredClasses();

```

[`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html) 包含两个公共成员类 [`Character.Subset`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.Subset.html) 和 [`Character.UnicodeBlock`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.UnicodeBlock.html) 以及一个私有类 `Character.CharacterCache`。

[`Class.getDeclaringClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaringClass--)。

[`java.lang.reflect.Field.getDeclaringClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#getDeclaringClass--)。

[`java.lang.reflect.Method.getDeclaringClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#getDeclaringClass--)。

[`java.lang.reflect.Constructor.getDeclaringClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#getDeclaringClass--)。

返回声明这些成员的[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)。[匿名类声明](https://docs.oracle.com/javase/specs/jls/se7/html/jls-15.html#jls-15.9.5)不会有声明类，但会有封闭类。

```java
import java.lang.reflect.Field;

Field f = System.class.getField("out");
Class c = f.getDeclaringClass();

```

字段[`out`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#out)在[`System`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html)中声明。

```java
public class MyClass {
    static Object o = new Object() {
        public void m() {} 
    };
    static Class<c> = o.getClass().getEnclosingClass();
}

```

由`o`定义的匿名类的声明类为`null`。

[`Class.getEnclosingClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnclosingClass--)。

返回类的直接封闭类。

```java
Class c = Thread.State.class().getEnclosingClass();

```

枚举[`Thread.State`](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.State.html)的封闭类为[`Thread`](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html)。

```java
public class MyClass {
    static Object o = new Object() { 
        public void m() {} 
    };
    static Class<c> = o.getClass().getEnclosingClass();
}

```

由`o`定义的匿名类由`MyClass`封闭。
