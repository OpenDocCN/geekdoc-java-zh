# 使用自定义类型映射

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/sqlcustommapping.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqlcustommapping.html)

**注意**：MySQL 目前不支持用户定义类型。MySQL 和 Java DB 目前不支持结构化类型或`DISTINCT` SQL 数据类型。没有可用的 JDBC 教程示例来演示本节中描述的功能。

随着生意蒸蒸日上，The Coffee Break 的老板经常添加新店铺并对数据库进行更改。老板决定为结构化类型`ADDRESS`使用自定义映射。这使老板可以对映射`ADDRESS`类型的 Java 类进行更改。Java 类将为`ADDRESS`的每个属性都有一个字段。类的名称和字段的名称可以是任何有效的 Java 标识符。

下面涵盖了以下主题：

+   实现 SQLData

+   使用连接的类型映射

+   使用自定义类型映射

## 实现 SQLData

实现自定义映射所需的第一步是创建一个实现接口`SQLData`的类。

结构化类型`ADDRESS`的 SQL 定义如下：

```java

  CREATE TYPE ADDRESS
  (
    NUM INTEGER,
    STREET VARCHAR(40),
    CITY VARCHAR(40),
    STATE CHAR(2),
    ZIP CHAR(5)
  );

```

为了自定义映射`ADDRESS`类型而实现`SQLData`接口的类可能如下所示：

```java
public class Address implements SQLData {
    public int num;
    public String street;
    public String city;
    public String state;
    public String zip;
    private String sql_type;

    public String getSQLTypeName() {
        return sql_type;
    }

    public void readSQL(SQLInput stream, String type)
        throws SQLException {
        sql_type = type;
        num = stream.readInt();
        street = stream.readString();
        city = stream.readString();
        state = stream.readString();
        zip = stream.readString();
    }

    public void writeSQL(SQLOutput stream)
        throws SQLException {
        stream.writeInt(num);
        stream.writeString(street);
        stream.writeString(city);
        stream.writeString(state);
        stream.writeString(zip);
    }
}

```

## 使用连接的类型映射

编写一个实现`SQLData`接口的类之后，设置自定义映射的唯一其他事项就是在类型映射中进行条目设置。例如，这意味着输入`ADDRESS`类型的完全限定 SQL 名称和`Address`类的`Class`对象。类型映射是`java.util.Map`接口的一个实例，与每个新创建的连接相关联，因此您可以使用它。假设`con`是活动连接，以下代码片段向与`con`关联的类型映射添加了一个 UDT `ADDRESS`的条目。

```java
java.util.Map map = con.getTypeMap();
map.put("SchemaName.ADDRESS", Class.forName("Address"));
con.setTypeMap(map);

```

每当调用`getObject`方法检索`ADDRESS`类型的实例时，驱动程序将检查与连接关联的类型映射，并看到它有一个`ADDRESS`的条目。驱动程序将注意到`Address`类的`Class`对象，创建其实例，并在后台执行许多其他操作以将`ADDRESS`映射到`Address`。您只需生成映射的类，然后在类型映射中进行条目设置，让驱动程序知道有一个自定义映射。驱动程序将完成其余所有工作。

存储具有自定义映射的结构化类型时情况类似。当调用`setObject`方法时，驱动程序将检查要设置的值是否是实现了`SQLData`接口的类的实例。如果是（表示存在自定义映射），驱动程序将使用自定义映射将值转换为其 SQL 对应项，然后返回给数据库。再次强调，驱动程序在后台执行自定义映射；你只需向`setObject`方法提供具有自定义映射的参数即可。稍后在本节中将看到一个示例。

查看使用标准映射（`Struct`对象）和使用 Java 编程语言中的自定义映射（类）之间的区别。下面的代码片段展示了标准映射到`Struct`对象的映射，这是在连接的类型映射中没有条目时驱动程序使用的映射。

```java
ResultSet rs = stmt.executeQuery(
    "SELECT LOCATION " +
    "WHERE STORE_NO = 100003");
rs.next();
Struct address = (Struct)rs.getObject("LOCATION");

```

变量`address`包含以下属性值：`4344`、`"First_Street"`、`"Verona"`、`"CA"`、`"94545"`。

下面的代码片段展示了当连接的类型映射中存在结构化类型`ADDRESS`的条目时会发生什么。请记住，列`LOCATION`存储类型为`ADDRESS`的值。

```java
ResultSet rs = stmt.executeQuery(
    "SELECT LOCATION " +
    "WHERE STORE_NO = 100003");
rs.next();
Address store_3 = (Address)rs.getObject("LOCATION");

```

变量`store_3`现在是`Address`类的实例，每个属性值都是`Address`的一个字段的当前值。请注意，在将`getObject`方法检索的对象转换为`Address`对象并将其分配给`store_3`之前，必须记得进行转换。还要注意，`store_3`必须是一个`Address`对象。

将使用`Struct`对象与使用`Address`类的实例进行比较。假设商店搬到了邻近城镇的更好位置，因此您必须更新数据库。使用自定义映射，重置`store_3`的字段，如下面的代码片段所示：

```java
ResultSet rs = stmt.executeQuery(
    "SELECT LOCATION " +
    "WHERE STORE_NO = 100003");
rs.next();
Address store_3 = (Address)rs.getObject("LOCATION");
store_3.num = 1800;
store_3.street = "Artsy_Alley";
store_3.city = "Arden";
store_3.state = "CA";
store_3.zip = "94546";
PreparedStatement pstmt = con.prepareStatement(
    "UPDATE STORES " +
    "SET LOCATION = ? " +
    "WHERE STORE_NO = 100003");
pstmt.setObject(1, store_3);
pstmt.executeUpdate();

```

列`LOCATION`中的值是`ADDRESS`类型的实例。驱动程序检查连接的类型映射，看到将`ADDRESS`与类`Address`关联的条目，因此使用`Address`中指示的自定义映射。当代码使用变量`*store_3*`作为第二个参数调用`setObject`方法时，驱动程序检查并看到`*store_3*`表示`Address`类的实例，该类实现了结构化类型`ADDRESS`的`SQLData`接口，并再次自动使用自定义映射。

没有`ADDRESS`的自定义映射，更新将更像这样：

```java
PreparedStatement pstmt = con.prepareStatement(
    "UPDATE STORES " +
    "SET LOCATION.NUM = 1800, " +
    "LOCATION.STREET = 'Artsy_Alley', " + 
    "LOCATION.CITY = 'Arden', " +
    "LOCATION.STATE = 'CA', " +
    "LOCATION.ZIP = '94546' " +
    "WHERE STORE_NO = 100003");
pstmt.executeUpdate;

```

## 使用您自己的类型映射

到目前为止，您仅使用了与连接关联的类型映射进行自定义映射。通常情况下，大多数程序员只会使用这种类型映射。但是，也可以创建一个类型映射并将其传递给某些方法，以便驱动程序将使用该类型映射而不是与连接关联的类型映射。这允许为同一用户定义类型（UDT）使用两种不同的映射。实际上，可以为相同的 UDT 拥有多个自定义映射，只要每个映射都设置为实现`SQLData`接口的类和类型映射中的条目。如果您没有向可以接受类型映射的方法传递类型映射，则驱动程序将默认使用与连接关联的类型映射。

在使用与连接关联的类型映射之外的类型映射的情况非常少见。例如，如果几个程序员共同开发 JDBC 应用程序并且在使用相同连接，则可能需要为方法提供一个类型映射。如果两个或更多程序员为相同的 SQL UDT 创建了自定义映射，则每个人都需要提供自己的类型映射，从而覆盖连接的类型映射。
