# 操作 LdapName（专有名称）

> 原文：[`docs.oracle.com/javase/tutorial/jndi/newstuff/ldapname.html`](https://docs.oracle.com/javase/tutorial/jndi/newstuff/ldapname.html)

Distinguished Name（DN）在 LDAP 中以字符串表示形式使用。用于表示 DN 的字符串格式在[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt)中指定。DN 由称为相对专有名称（RDN）的组件组成。以下是 DN 的示例：

"CN=John Smith, O=Isode Limited, C=GB"

它由以下 RDN 组成：

+   CN=John Smith

+   O=Isode Limited

+   C=GB

下面的类分别表示 DN 和 RDN。

+   [`javax.naming.ldap.LdapName`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html)

+   [`javax.naming.ldap.Rdn`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html)

LdapName 类实现了[`javax.naming.Name`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Name.html)接口，类似于[`javax.naming.CompoundName`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Name.html)和[`javax.naming.CompositeName`](https://docs.oracle.com/javase/8/docs/api/javax/naming/CompositeName.html)类。

LdapName 和 Rdn 类允许轻松操作 DN 和 RDN。使用这些 API，通过将名称和值配对，轻松构造 RDN。可以使用 RDN 列表构造 DN。同样，可以从它们的字符串表示中检索 DN 和 RDN 的各个组件。

## LdapName

可以使用其在[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt)中定义的字符串表示形式或使用 Rdns 列表创建 LdapName。当使用前一种方式时，指定的字符串将根据 RFC2253 中定义的规则进行解析。如果字符串不是有效的 DN，则会抛出[`InvalidNameException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/InvalidNameException.html)。以下是使用构造函数解析 LDAP 名称并打印其组件的示例。

```java
String name = "cn=Mango,ou=Fruits,o=Food";
try {
    LdapName dn = new LdapName(name);
    System.out.println(dn + " has " + dn.size() + " RDNs: ");
    for (int i = 0; i < dn.size(); i++) {
        System.out.println(dn.get(i));
    }
} catch (InvalidNameException e) {
    System.out.println("Cannot parse name: " + name);
}

```

使用输入`"cn=Mango,ou=Fruits,o=Food"`运行此示例会产生以下结果：

```java
cn=Mango,ou=Fruits,o=Food has 3 RDNs: 
o=Food
ou=Fruits
cn=Mango

```

LdapName 类包含方法，用于将其组件作为 RDN 和字符串访问，修改 LdapName，比较两个 LdapName 是否相等，并获取名称的字符串表示形式。

### 访问 LDAP 名称的名称组件：

以下是您可以使用的方法来访问名称组件作为 RDN 和字符串：

[`getRdn(int posn)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getRdn-int-)

[`get(int posn)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#get-int-)

[`getRdns()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getRdns--)

[`getAll()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getAll--)

[`getPrefix(int posn)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getPrefix-intposn-)

[`getSuffix(int posn)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getSuffix-intposn-)

[`clone()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#clone--)

要检索 LdapName 中特定位置的组件，您可以使用 getRdn()或 get()。前面的构造函数示例展示了其用法的示例。[`getRdns()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getRdns--)返回所有 RDN 的列表，[`getAll()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getAll--)将 LdapName 的所有组件作为枚举返回。

最右边的 RDN 位于索引 0，最左边的 RDN 位于索引 n-1。例如，专有名称："cn=Mango, ou=Fruits, o=Food"按以下顺序编号，范围从 0 到 2：{o=Food, ou=Fruits, cn=Mango}

您还可以将 LdapName 的后缀或前缀作为 LdapName 实例获取。这里有一个`示例`，展示了如何获取 LDAP 名称的后缀和前缀。

```java
LdapName dn = new LdapName("cn=Mango, ou=Fruits, o=Food");
Name suffix = dn.getSuffix(1);  // 1 <= index < cn.size()
Name prefix = dn.getPrefix(1);  // 0 <= index < 1

```

运行此程序时，它会生成以下输出：

```java
cn=Mango ou=Fruits
o=Food 

```

要复制 LdapName，您可以使用 clone()。

### 修改 LDAP 名称

以下是您可以使用的方法来修改 LDAP 名称：

[`add(Rdn rdn)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#add-Rdn-)

[`add(String comp)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#add-String-)

[`add(int posn, String comp)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#add-int-String-)

[`addAll(List suffixRdns)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#addAll-List-)

[`addAll(Name suffix)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#addAll-Namesuffix-)

[`addAll(int posn, List suffixRdns)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#addAll-int-List-)

[`addAll(int posn, Name suffix)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#addAll-int-Name-)

[`remove(int posn)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#remove-int-)

创建 LdapName 实例后，您可以向其添加和删除组件。这里有一个`示例`，演示了如何将一个 LdapName 附加到现有 LdapName 中，在开头和结尾添加组件，以及删除第二个组件。

```java
     LdapName dn = new LdapName("ou=Fruits,o=Food");
     LdapName dn2 = new LdapName("ou=Summer");
     System.out.println(dn.addAll(dn2)); // ou=Summer,ou=Fruits,o=Food
     System.out.println(dn.add(0, "o=Resources")); 
// ou=Summer,ou=Fruits,o=Food,o=Resources
     System.out.println(dn.add("cn=WaterMelon")); 
// cn=WaterMelon,ou=Summer,ou=Fruits,o=Food,o=Resources
     System.out.println(dn.remove(1));  // o=Food
     System.out.println(dn);  
// cn=WaterMelon,ou=Summer,ou=Fruits,o=Resources

```

### 比较 LDAP 名称

以下是您可以使用的方法来比较两个 LDAP 名称：

[`compareTo(Object name)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#compareTo-Object-)

[`equals(Object name)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#equals-Object-)

[`endsWith(List)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#endsWith-List-)

[`endWith(Name name)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#endsWith-Name-)

[`startsWith(List rdns)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#startsWith-iList-)

[`startsWith(Name name)`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#startsWith-Name-)

[`isEmpty()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#isEmpty--)

您可以使用`compareTo()`对 LdapName 实例列表进行排序。`equals()`方法可让您确定两个 LdapNames 是否在语法上相等。如果两个 LdapNames 在相同顺序中具有相同（大小写匹配）的组件，则它们是相等的。

使用`startsWith()`和`endsWith()`，您可以了解一个 LdapName 是否以另一个 LdapName 开头或结尾；也就是说，一个 LdapName 是否是另一个 LdapName 的后缀或前缀。

便利方法`isEmpty()`使您能够确定 LdapName 是否具有零个组件。您还可以使用表达式`size() == 0`执行相同的检查。

这里有一个示例，`CompareLdapNames`，使用了其中一些比较方法。

```java
LdapName one = new LdapName("cn=Vincent Ryan, ou=People, o=JNDITutorial");
LdapName two = new LdapName("cn=Vincent Ryan");
LdapName three = new LdapName("o=JNDITutorial");
LdapName four = new LdapName("");

System.out.println(one.equals(two));        // false
System.out.println(one.startsWith(three));  // true
System.out.println(one.endsWith(two));      // true
System.out.println(one.startsWith(four));   // true
System.out.println(one.endsWith(four));     // true
System.out.println(one.endsWith(three));    // false
System.out.println(one.isEmpty());          // false
System.out.println(four.isEmpty());         // true
System.out.println(four.size() == 0);       // true

```

### 获取字符串表示

下面的方法可以让您获取根据 RFC 2253 中指定的语法格式化的 LDAP 名称的字符串表示：

[`toString()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#toString--)

当您使用 LdapName 构造函数时，您提供 LDAP 名称的字符串表示，并获得一个 LdapName 实例。要执行相反操作，即获取 LdapName 实例的字符串表示，您可以使用`toString()`。`toString()`的结果可以再次传递给构造函数，以生成与原始 LdapName 实例相等的 LdapName 实例。这里有一个示例，`LdapNametoString`：

```java
LdapName dn = new LdapName(name);
String str = dn.toString();
System.out.println(str);
LdapName dn2 = new LdapName(str);
System.out.println(dn.equals(dn2));  // true

```

### 作为上下文方法的参数的 LdapName

Context 方法要求将复合名称或复合名称作为参数传递给其方法。因此，可以直接将 LdapName 传递给上下文方法，如示例中所示，`LookupLdapName`：

```java
// Create the initial context
Context ctx = new InitialContext(env);

// An LDAP name
LdapName dn = new LdapName("ou=People,o=JNDITutorial");

// Perform the lookup using the dn
Object obj = ctx.lookup(dn);

```

类似地，当上下文方法从 list()、listBindings() 或 search() 操作返回结果时，可以通过调用[`getNameInNamespace()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getNameInNamepspace--)检索 DN。可以直接从 DN 构造 LdapName，如示例中所示，`RetrievingLdapName`：

```java
while (answer.hasMore()) {
    SearchResult sr = (SearchResult) answer.next();
    String name = sr.getNameInNamespace();
    System.out.println(name);
    LdapName dn = new LdapName(name);

    // do something with the dn

```
