# 创建新数组

> 原文：[`docs.oracle.com/javase/tutorial/reflect/special/arrayInstance.html`](https://docs.oracle.com/javase/tutorial/reflect/special/arrayInstance.html)

就像非反射代码一样，反射支持通过[`java.lang.reflect.Array.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#newInstance-java.lang.Class-int-)动态创建任意类型和维度的数组的能力。考虑``ArrayCreator``，一个能够动态创建数组的基本解释器。将解析的语法如下：

```java
fully_qualified_class_name variable_name[] = 
     { val1, val2, val3, ... }

```

假设`fully_qualified_class_name`代表一个具有接受单个[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)参数的构造函数的类。数组的维度由提供的值的数量确定。以下示例将构造一个`fully_qualified_class_name`数组的实例，并用`val1`、`val2`等给定的实例填充其值。（此示例假定熟悉[`Class.getConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructor-java.lang.Class...-)和[`java.lang.reflect.Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-)。有关[`Constructor`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html)的反射 API 的讨论，请参阅本教程的 Creating New Class Instances 部分。）

```java

import java.lang.reflect.Array;
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import java.util.regex.Pattern;
import java.util.regex.Matcher;
import java.util.Arrays;
import static java.lang.System.out;

public class ArrayCreator {
    private static String s = "java.math.BigInteger bi[] = { 123, 234, 345 }";
    private static Pattern p = Pattern.compile("^\\s*(\\S+)\\s*\\w+\\[\\].*\\{\\s*([^}]+)\\s*\\}");

    public static void main(String... args) {
        Matcher m = p.matcher(s);

        if (m.find()) {
            String cName = m.group(1);
            String[] cVals = m.group(2).split("[\\s,]+");
            int n = cVals.length;

            try {
                Class<?> c = Class.forName(cName);
                Object o = Array.newInstance(c, n);
                for (int i = 0; i < n; i++) {
                    String v = cVals[i];
                    Constructor ctor = c.getConstructor(String.class);
                    Object val = ctor.newInstance(v);
                    Array.set(o, i, val);
                }

                Object[] oo = (Object[])o;
                out.format("%s[] = %s%n", cName, Arrays.toString(oo));

            // production code should handle these exceptions more gracefully
            } catch (ClassNotFoundException x) {
                x.printStackTrace();
            } catch (NoSuchMethodException x) {
                x.printStackTrace();
            } catch (IllegalAccessException x) {
                x.printStackTrace();
            } catch (InstantiationException x) {
                x.printStackTrace();
            } catch (InvocationTargetException x) {
                x.printStackTrace();
            }
        }
    }
}

```

```java
$ *java ArrayCreator*
java.math.BigInteger [] = [123, 234, 345]

```

上面的示例展示了可能希望通过反射创建数组的一种情况；即如果组件类型直到运行时才知道。在这种情况下，代码使用[`Class.forName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#forName--)获取所需组件类型的类，然后调用特定的构造函数来初始化数组的每个组件，然后设置相应的数组值。
