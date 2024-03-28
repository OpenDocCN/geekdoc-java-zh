# 获取字段类型

> 原文：[`docs.oracle.com/javase/tutorial/reflect/member/fieldTypes.html`](https://docs.oracle.com/javase/tutorial/reflect/member/fieldTypes.html)

字段可以是原始类型或引用类型。有八种原始类型：`boolean`、`byte`、`short`、`int`、`long`、`char`、`float` 和 `double`。引用类型是任何直接或间接是 [`java.lang.Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) 的子类，包括接口、数组和枚举类型。

``FieldSpy`` 示例根据完全限定的二进制类名和字段名打印字段的类型和泛型类型。

```java

import java.lang.reflect.Field;
import java.util.List;

public class FieldSpy<T> {
    public boolean[][] b = {{ false, false }, { true, true } };
    public String name  = "Alice";
    public List<Integer> list;
    public T val;

    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    Field f = c.getField(args[1]);
	    System.out.format("Type: %s%n", f.getType());
	    System.out.format("GenericType: %s%n", f.getGenericType());

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	}
    }
}

```

获取此类中三个公共字段（`b`、`name` 和参数化类型 `list`）的类型的示例输出如下。用户输入以斜体表示。

```java
$ *java FieldSpy FieldSpy b*
Type: class [[Z
GenericType: class [[Z
$ *java FieldSpy FieldSpy name*
Type: class java.lang.String
GenericType: class java.lang.String
$ *java FieldSpy FieldSpy list*
Type: interface java.util.List
GenericType: java.util.List<java.lang.Integer>
$ *java FieldSpy FieldSpy val*
Type: class java.lang.Object
GenericType: T

```

字段 `b` 的类型是布尔值的二维数组。类型名称的语法在 [`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--) 中有描述。

字段 `val` 的类型报告为 `java.lang.Object`，因为泛型是通过*类型擦除*实现的，在编译期间删除了关于泛型类型的所有信息。因此，`T` 被替换为类型变量的上界，在本例中为 `java.lang.Object`。

[`Field.getGenericType()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#getGenericType--) 如果存在，将查阅类文件中的 Signature 属性。如果该属性不可用，则退而求其次使用 [`Field.getType()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#getType--)，这个方法在引入泛型之后并没有改变。反射中其他以 `getGeneric*Foo*` 命名的方法，对于某个 *Foo* 值的实现方式类似。
