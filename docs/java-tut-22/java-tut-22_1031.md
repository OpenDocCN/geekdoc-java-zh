# 操作相对独立名称（RDN）

> 原文：[`docs.oracle.com/javase/tutorial/jndi/newstuff/rdn.html`](https://docs.oracle.com/javase/tutorial/jndi/newstuff/rdn.html)

类[`javax.naming.ldap.Rdn`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html)表示根据[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt)指定的相对独立名称（RDN）。RDN 表示 DN 的组件，如操作 LdapName 课程中所解释的那样。RDN 由类型和值对组成。RDN 的示例包括：

+   `OU=Sun`

+   `OU=Sales+CN=J.Smith`。

    上面的示例显示了多值 RDN 的表示。

[`Rdn`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html)类提供了访问 RDN 的名称/值对、获取其字符串表示、检索[`Attributes`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/Attributes.html)视图、比较和确定 RDN 的相等性以及转义和取消转义 RDN 值部分的方法。

`Rdn`类是不可变的。

## 构造 Rdn

如果是单个名称/值对的 RDN，可以使用指定的名称和值对构造 Rdn。对于多值 RDN，应创建包含所有名称/值对的属性集，并使用以`Attributes`为参数的构造函数。还可以根据[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt)中指定的字符串表示创建 Rdn。最后，可以使用其复制构造函数克隆 Rdn。这里是一个使用不同类型构造函数创建 RDN 的`示例`。

```java
        Rdn rdn1 = new Rdn("ou= Juicy\\, Fruit");
        System.out.println("rdn1:" + rdn1.toString());

        Rdn rdn2 = new Rdn(rdn1);
        System.out.println("rdn2:" + rdn2.toString());

        Attributes attrs = new BasicAttributes();
        attrs.put("ou", "Juicy, Fruit");
        attrs.put("cn", "Mango");
        Rdn rdn3 = new Rdn(attrs);
        System.out.println("rdn3:" + rdn3.toString());

        Rdn rdn4 = new Rdn("ou", "Juicy, Fruit");
        System.out.println("rdn4:" + rdn4.toString());

```

## 访问 RDN 的类型/值对

可以使用以下方法获取 RDN 的类型/值：

[`getType()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#getType--)

[`getValue()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#getValue--)

[`toAttributes()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#toAttributes--)

对于由单个类型/值对组成的 RDN，[`getType()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#getType--)方法返回类型，[`getValue()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#getValue--)方法返回 RDN 的值。方法[`toAttributes()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#toAttributes--)返回类型/值对的属性视图。下面的`示例`打印了 RDN 的类型/值对。

```java
        Attributes attrs = new BasicAttributes();
        attrs.put("o", "Yellow");
        attrs.put("cn", "Mango");

        // create a binary value for the RDN
        byte[] mangoJuice = new byte[6];
        for (int i = 0; i < mangoJuice.length; i++) {
            mangoJuice[i] = (byte) i;
        }
        attrs.put("ou", mangoJuice);
        Rdn rdn = new Rdn(attrs);

        System.out.println();
        System.out.println("size:" + rdn.size());
        System.out.println("getType(): " + rdn.getType());
        System.out.println("getValue(): " + rdn.getValue());

        // test toAttributes
        System.out.println();
        System.out.println("toAttributes(): " + rdn.toAttributes());

```

## 获取字符串表示

为了按照[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt)中指定的语法格式化 RDN 的字符串表示，可以使用：

[`toString()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#toString--)

当您使用接受`String`参数的 Rdn 构造函数时，您提供了 RDN 的字符串表示，并获得了一个 Rdn 实例。要做相反的操作，即获取 Rdn 实例的字符串表示，您可以使用 toString()。toString()的结果可以馈送回 Rdn 构造函数，以产生与原始 Rdn 实例相等的 Rdn 实例。

这里有一个`示例`：

```java
Rdn rdn = new Rdn("cn=Juicy\\,Fruit");
String str = rdn.toString();
System.out.println(str);
Rdn rdn2 = new Rdn(str);
System.out.println(rdn.equals(rdn2));    // true

```

## 比较 RDNs

下面的方法使 RDN 的比较成为可能：

[`equals(Object Rdn)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#equals-Object-)

[`compareTo(Object Rdn)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#compareTo-Object-)

您可以使用 compareTo()对 Rdn 实例列表进行排序。equals()让您确定两个 Rdns 在语法上是否相等。如果两个 Rdns 都具有相同（大小写匹配）的类型/值对，则它们是相等的。多值 RDN 中组件的顺序不重要。

这里有一个`示例`：

```java
        Rdn one = new Rdn("ou=Sales+cn=Bob");
        Rdn two = new Rdn("cn=Bob+ou=Sales");
        Rdn three = new Rdn("ou=Sales+cn=Bob+c=US");
        Rdn four = new Rdn("cn=lowercase");
        Rdn five = new Rdn("cn=LowerCASE");
        System.out.println(one.equals(two));    // true
        System.out.println(two.equals(three));  // false
        System.out.println(one.equals(three));  // false
        System.out.println(four.equals(five));  // true

```

## 转义和反转义特殊字符

Rdn 类的最佳用途之一是处理包含特殊字符的 DN。它会自动处理特殊字符的转义和反转义。像 '\'（反斜杠）、','（逗号）、+（加号）等字符在[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt)中有特定的语义。您可以在 RFC2253 中找到所有特殊字符的列表。当这些字符作为 DN 中的文字时，必须用 '\'（反斜杠）进行转义。

例如，考虑一个 RDN：`cn=Juicy, Fruit`，出现在 Juicy 和 Fruit 之间的逗号，是一个需要用 '\'（反斜杠）转义的特殊字符。经过语法格式化后的 RDN 如下所示：`cn=Juicy\, Fruit` 然而， '\'（反斜杠）字符本身是 Java 语言字符串语法中的一个特殊字符，需要再次用 '\'（反斜杠）转义。Java 语言字符串格式和[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt)都使用 '\'（反斜杠）来转义特殊字符。因此，Java 格式化的 RDN 字符串如下所示：`cn=Juicy\\, Fruit` 请注意，上述提到的格式化规则仅适用于 Rdn 的值组件。`Rdn`类提供了两个`static`方法来处理 RDN 值的自动转义和反转义：

[`escapeValue()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#escapeValue--)

[`unescapeValue()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#unescapeValue--)

下面的`示例`显示了如何获取 DN 的字符串表示，而无需处理[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt)中定义的处理特殊字符的语法。

```java
    // DN with ',' (comma)
    String unformatted = "Juicy, Fruit";
    String formatted = Rdn.escapeValue(unformatted);
    LdapName dn = new LdapName("cn=" + formatted);
    System.out.println("dn:" + dn);

    unformatted = "true+false";
    formatted = Rdn.escapeValue(unformatted); 
    dn = new LdapName("cn=" + formatted);
    System.out.println("dn:" + dn);

    // DN with a binary value as one of its attribute values
    byte[] bytes = new byte[] {1, 2, 3, 4};
    formatted = Rdn.escapeValue(bytes);
    System.out.println("Orig val: " + bytes + "Escaped val: " + formatted);

```

同样地，使用静态的`unescapeValue()`方法，可以从格式化值中获取原始字符串。`这里`是一个检索原始值的示例。

```java
    // DN with ',' (comma)
    String unformatted = "Juicy, Fruit";
    String formatted = Rdn.escapeValue(unformatted);
    System.out.println("Formatted:" + formatted);
    Object original = Rdn.unescapeValue(formatted);
    System.out.println("Original:" +  original);  

    // DN with a '+' (plus)
    unformatted = "true+false";
    formatted = Rdn.escapeValue(unformatted); 
    System.out.println("Formatted:" + formatted);
    original = Rdn.unescapeValue(formatted);
    System.out.println("Original:" +  original);  

    // DN with a binary value as one of its attribute values
    byte[] bytes = new byte[] {1, 2, 3, 4};
    formatted = Rdn.escapeValue(bytes);
    System.out.println("Formatted:" + formatted);
    original = Rdn.unescapeValue(formatted);
    System.out.println("Original:" +  original);  

```
