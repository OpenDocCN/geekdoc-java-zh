# 使用 DataSource 对象进行连接

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/sqldatasources.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqldatasources.html)

本节涵盖了`DataSource`对象，这是获取与数据源连接的首选方法。除了其他优点，稍后将解释的优点之外，`DataSource`对象还可以提供连接池和分布式事务。这种功能对企业数据库计算至关重要。特别是，它对企业 JavaBeans（EJB）技术至关重要。

本节向您展示如何使用`DataSource`接口获取连接以及如何使用分布式事务和连接池。这两者在您的 JDBC 应用程序中都涉及非常少的代码更改。

部署执行这些操作的类所需的工作，通常由系统管理员使用工具（如 Apache Tomcat 或 Oracle WebLogic Server）完成，取决于部署的`DataSource`对象的类型。因此，本节大部分内容都致力于展示系统管理员如何设置环境，以便程序员可以使用`DataSource`对象获取连接。

下面的主题包括：

+   使用 DataSource 对象获取连接

+   部署基本的 DataSource 对象

+   部署其他 DataSource 实现

+   获取和使用池化连接

+   部署分布式事务

+   使用连接进行分布式事务

## 使用 DataSource 对象获取连接

在建立连接中，您学习了如何使用`DriverManager`类获取连接。本节将向您展示如何使用`DataSource`对象获取与数据源的连接，这是首选方法。

由实现`DataSource`类的类实例化的对象代表特定的 DBMS 或其他数据源，比如文件。`DataSource`对象代表特定的 DBMS 或其他数据源，比如文件。如果一个公司使用多个数据源，它将为每个数据源部署一个单独的`DataSource`对象。`DataSource`接口由驱动程序供应商实现。它可以以三种不同的方式实现：

+   基本的`DataSource`实现会生成标准的未池化或未用于分布式事务的`Connection`对象。

+   支持连接池的`DataSource`实现会生成参与连接池的`Connection`对象，即可以被回收的连接。

+   支持分布式事务的`DataSource`实现会生成可用于分布式事务的`Connection`对象，即访问两个或多个 DBMS 服务器的事务。

JDBC 驱动程序应至少包含基本的`DataSource`实现。例如，Java DB JDBC 驱动程序包括`org.apache.derby.jdbc.ClientDataSource`的实现，而对于 MySQL，则是`com.mysql.jdbc.jdbc2.optional.MysqlDataSource`。如果您的客户端在 Java 8 紧凑配置文件 2 上运行，则 Java DB JDBC 驱动程序是`org.apache.derby.jdbc.BasicClientDataSource40`。本教程的示例需要紧凑配置文件 3 或更高版本。

支持分布式事务的`DataSource`类通常也实现了对连接池的支持。例如，由 EJB 供应商提供的`DataSource`类几乎总是支持连接池和分布式事务。

假设前面示例中茁壮成长的 The Coffee Break 连锁店的所有者已决定通过互联网进一步扩展销售咖啡。随着预期的大量在线业务，所有者肯定需要连接池。打开和关闭连接涉及大量开销，所有者预计这个在线订购系统将需要大量的查询和更新。通过连接池，一组连接可以一遍又一遍地使用，避免为每次数据库访问创建新连接的开销。此外，所有者现在有第二个包含最近收购的咖啡烘焙公司数据的 DBMS。这意味着所有者希望能够编写使用旧 DBMS 服务器和新 DBMS 服务器的分布式事务。

链店所有者已重新配置计算机系统以服务新的、更大的客户群。所有者已购买最新的 JDBC 驱动程序和与之配套的 EJB 应用服务器，以便使用分布式事务并获得连接池带来的性能提升。许多与最近购买的 EJB 服务器兼容的 JDBC 驱动程序可用。现在，所有者拥有三层架构，中间层有一个新的 EJB 应用服务器和 JDBC 驱动程序，第三层是两个 DBMS 服务器。发出请求的客户端计算机是第一层。

## 部署基本的 DataSource 对象

系统管理员需要部署`DataSource`对象，以便 The Coffee Break 的编程团队可以开始使用它们。部署`DataSource`对象包括三个任务：

1.  创建`DataSource`类的实例

1.  设置其属性

1.  使用使用 Java 命名和目录接口（JNDI）API 的命名服务进行注册

首先，考虑最基本的情况，即使用`DataSource`接口的基本实现，即不支持连接池或分布式事务的实现。在这种情况下，只需要部署一个`DataSource`对象。`DataSource`的基本实现产生与`DriverManager`类产生的相同类型的连接。

### 创建 DataSource 类的实例并设置其属性

假设一家只想要一个基本的`DataSource`实现的公司从 JDBC 供应商 DB Access, Inc 购买了一个驱动程序。该驱动程序包括实现`DataSource`接口的类`com.dbaccess.BasicDataSource`。以下代码摘录创建`BasicDataSource`类的实例并设置其属性。部署`BasicDataSource`实例后，程序员可以调用`DataSource.getConnection`方法获取连接到公司数据库`CUSTOMER_ACCOUNTS`。首先，系统管理员使用默认构造函数创建`BasicDataSource`对象`*ds`。然后，系统管理员设置三个属性。请注意，以下代码通常由部署工具执行： 

```java
com.dbaccess.BasicDataSource ds = new com.dbaccess.BasicDataSource();
ds.setServerName("grinder");
ds.setDatabaseName("CUSTOMER_ACCOUNTS");
ds.setDescription("Customer accounts database for billing");

```

变量`*ds*`现在代表安装在服务器上的数据库`CUSTOMER_ACCOUNTS`。`*ds*`对象生成的任何连接都将是到数据库`CUSTOMER_ACCOUNTS`的连接。

### 使用使用 JNDI API 的命名服务注册 DataSource 对象

设置属性后，系统管理员可以将`BasicDataSource`对象注册到 JNDI（Java 命名和目录接口）命名服务中。通常使用的特定命名服务通常由系统属性确定，这里没有显示。以下代码摘录注册`BasicDataSource`对象并将其绑定到逻辑名称`jdbc/billingDB`：

```java
Context ctx = new InitialContext();
ctx.bind("jdbc/billingDB", ds);

```

此代码使用 JNDI API。第一行创建一个`InitialContext`对象，它类似于文件系统中的根目录的起始点。第二行将`BasicDataSource`对象`*ds*`与逻辑名称`jdbc/billingDB`关联或绑定。在下一个代码摘录中，您将向命名服务提供此逻辑名称，它将返回`BasicDataSource`对象。逻辑名称可以是任何字符串。在这种情况下，公司决定使用名称`billingDB`作为`CUSTOMER_ACCOUNTS`数据库的逻辑名称。

在前面的示例中，`jdbc`是初始上下文下的一个子上下文，就像根目录下的目录是子目录一样。名称`jdbc/billingDB`类似于路径名，其中路径中的最后一项类似于文件名。在这种情况下，`billingDB`是赋予`BasicDataSource`对象`*ds*`的逻辑名称。子上下文`jdbc`保留用于绑定到`DataSource`对象的逻辑名称，因此`jdbc`将始终是数据源逻辑名称的第一部分。

### 使用部署的 DataSource 对象

系统管理员部署了基本的`DataSource`实现后，程序员就可以开始使用了。这意味着程序员可以提供绑定到`DataSource`类实例的逻辑数据源名称，JNDI 命名服务将返回该`DataSource`类的实例。然后可以在该`DataSource`对象上调用`getConnection`方法，以获取连接到其表示的数据源的连接。例如，程序员可能会编写以下两行代码来获取一个产生与数据库`CUSTOMER_ACCOUNTS`连接的`DataSource`对象。

```java
Context ctx = new InitialContext();
DataSource ds = (DataSource)ctx.lookup("jdbc/billingDB");

```

第一行代码获取一个初始上下文作为检索`DataSource`对象的起点。当您向`lookup`方法提供逻辑名称`jdbc/billingDB`时，该方法将返回系统管理员在部署时绑定到`jdbc/billingDB`的`DataSource`对象。因为`lookup`方法的返回值是一个 Java `Object`，我们必须将其转换为更具体的`DataSource`类型，然后将其赋给变量`*ds*`。

变量`*ds*`是实现`DataSource`接口的`com.dbaccess.BasicDataSource`类的实例。调用`*ds*.getConnection`方法将产生与`CUSTOMER_ACCOUNTS`数据库的连接。

```java
Connection con = ds.getConnection("fernanda","brewed");

```

`getConnection`方法仅需要用户名和密码，因为变量`*ds*`在其属性中具有与`CUSTOMER_ACCOUNTS`数据库建立连接所需的其余信息，如数据库名称和位置。

### DataSource 对象的优势

由于其属性，`DataSource`对象是获取连接的比`DriverManager`类更好的选择。程序员不再需要在其应用程序中硬编码驱动程序名称或 JDBC URL，这使它们更具可移植性。此外，`DataSource`属性使代码维护更加简单。如果有任何更改，系统管理员可以更新数据源属性，而不必担心更改每个连接到数据源的应用程序。例如，如果数据源被移动到不同的服务器，系统管理员只需将`serverName`属性设置为新服务器名称。

除了可移植性和易维护性之外，使用`DataSource`对象获取连接还可以提供其他优势。当`DataSource`接口被实现以与`ConnectionPoolDataSource`实现一起工作时，由该`DataSource`类的实例产生的所有连接将自动成为池化连接。同样，当`DataSource`实现被实现以与`XADataSource`类一起工作时，它产生的所有连接将自动成为可用于分布式事务的连接。下一节将展示如何部署这些类型的`DataSource`实现。

## 部署其他 DataSource 实现

系统管理员或其他从事该职能的人可以部署一个`DataSource`对象，以便它产生的连接是连接池连接。为此，他或她首先部署一个`ConnectionPoolDataSource`对象，然后部署一个实现与其配合工作的`DataSource`对象。设置`ConnectionPoolDataSource`对象的属性，以便它表示将生成连接的数据源。在`ConnectionPoolDataSource`对象已经注册到 JNDI 命名服务后，部署`DataSource`对象。通常只需设置`DataSource`对象的两个属性：`description`和`dataSourceName`。给定给`dataSourceName`属性的值是先前部署的标识`ConnectionPoolDataSource`对象的逻辑名称，该对象包含用于建立连接所需的属性。

使用`ConnectionPoolDataSource`和`DataSource`对象部署后，您可以在`DataSource`对象上调用`DataSource.getConnection`方法并获得一个连接池连接。此连接将连接到`ConnectionPoolDataSource`对象属性中指定的数据源。

以下示例描述了如何为 The Coffee Break 的系统管理员部署一个实现提供连接池连接的`DataSource`对象。系统管理员通常会使用部署工具，因此本节中显示的代码片段是部署工具将执行的代码。

为了获得更好的性能，The Coffee Break 公司从 DB Access, Inc.购买了一个 JDBC 驱动程序，其中包括实现`ConnectionPoolDataSource`接口的`com.dbaccess.ConnectionPoolDS`类。系统管理员创建此类的实例，设置其属性，并将其注册到 JNDI 命名服务中。The Coffee Break 从其 EJB 服务器供应商 Application Logic, Inc.购买了其`DataSource`类`com.applogic.PooledDataSource`。`com.applogic.PooledDataSource`类通过使用`ConnectionPoolDataSource`类`com.dbaccess.ConnectionPoolDS`提供的底层支持来实现连接池。

必须首先部署`ConnectionPoolDataSource`对象。以下代码创建了`com.dbaccess.ConnectionPoolDS`的实例并设置其属性：

```java
com.dbaccess.ConnectionPoolDS cpds = new com.dbaccess.ConnectionPoolDS();
cpds.setServerName("creamer");
cpds.setDatabaseName("COFFEEBREAK");
cpds.setPortNumber(9040);
cpds.setDescription("Connection pooling for " + "COFFEEBREAK DBMS");

```

在部署`ConnectionPoolDataSource`对象后，系统管理员部署`DataSource`对象。以下代码将`com.dbaccess.ConnectionPoolDS`对象`*cpds*`注册到 JNDI 命名服务中。请注意，与`*cpds*`变量关联的逻辑名称在`jdbc`的子上下文下添加了`pool`，类似于在分层文件系统中的另一个子目录下添加子目录。`com.dbaccess.ConnectionPoolDS`类的任何实例的逻辑名称始终以`jdbc/pool`开头。Oracle 建议将所有`ConnectionPoolDataSource`对象放在`jdbc/pool`子上下文下：

```java
Context ctx = new InitialContext();
ctx.bind("jdbc/pool/fastCoffeeDB", cpds);

```

接下来，实现与`*cpds*`变量和`com.dbaccess.ConnectionPoolDS`类的其他实例交互的`DataSource`类被部署。下面的代码创建了这个类的一个实例并设置了其属性。请注意，为`com.applogic.PooledDataSource`的这个实例只设置了两个属性。设置`description`属性是因为它总是必需的。另一个设置的属性是`dataSourceName`，为`*cpds*`给出了逻辑 JNDI 名称，它是`com.dbaccess.ConnectionPoolDS`类的一个实例。换句话说，`*cpds*`代表了将为`DataSource`对象实现连接池的`ConnectionPoolDataSource`对象。

下面的代码，可能会被部署工具执行，创建了一个`PooledDataSource`对象，设置了其属性，并将其绑定到逻辑名称`jdbc/fastCoffeeDB`：

```java
com.applogic.PooledDataSource ds = new com.applogic.PooledDataSource();
ds.setDescription("produces pooled connections to COFFEEBREAK");
ds.setDataSourceName("jdbc/pool/fastCoffeeDB");
Context ctx = new InitialContext();
ctx.bind("jdbc/fastCoffeeDB", ds);

```

此时，部署了一个`DataSource`对象，应用程序可以从中获取到数据库`COFFEEBREAK`的连接池连接。

## 获取和使用连接池连接

*连接池*是数据库连接对象的缓存。这些对象代表了可以被应用程序用来连接数据库的物理数据库连接。在运行时，应用程序从连接池请求连接。如果连接池包含可以满足请求的连接，则将连接返回给应用程序。如果找不到连接，则创建一个新连接并返回给应用程序。应用程序使用连接对数据库执行一些工作，然后将对象返回到连接池。连接随后可用于下一个连接请求。

连接池促进了连接对象的重复使用，减少了连接对象被创建的次数。连接池显著提高了数据库密集型应用的性能，因为创建连接对象在时间和资源方面都是昂贵的。

现在这些`DataSource`和`ConnectionPoolDataSource`对象已经部署，程序员可以使用`DataSource`对象获取连接池连接。获取连接池连接的代码与获取非连接池连接的代码类似，如下两行所示：

```java
ctx = new InitialContext();
ds = (DataSource)ctx.lookup("jdbc/fastCoffeeDB");

```

变量`*ds*`代表一个`DataSource`对象，用于向数据库`COFFEEBREAK`生成连接池连接。您只需要检索一次这个`DataSource`对象，因为您可以使用它生成所需数量的连接池连接。在`*ds*`变量上调用`getConnection`方法会自动生成一个连接池连接，因为`*ds*`变量表示的`DataSource`对象被配置为生成连接池连接。

连接池对程序员通常是透明的。在使用连接池连接时，只需要做两件事情：

1.  使用`DataSource`对象而不是`DriverManager`类来获取连接。在下面的代码行中，`*ds*`是一个已实现和部署的`DataSource`对象，它将创建池化连接，`username`和`password`是代表具有访问数据库权限的用户的凭据的变量：

    ```java
    Connection con = ds.getConnection(username, password);

    ```

1.  使用`finally`语句来关闭池化连接。下面的`finally`块将出现在应用于使用池化连接的代码的`try/catch`块之后：

    ```java
    try {
        Connection con = ds.getConnection(username, password);
        // ... code to use the pooled
        // connection con
    } catch (Exception ex {
        // ... code to handle exceptions
    } finally {
        if (con != null) con.close();
    }

    ```

否则，使用池化连接的应用程序与使用常规连接的应用程序相同。当进行连接池化时，应用程序员可能注意到的唯一其他事情是性能更好。

以下示例代码获取一个`DataSource`对象，该对象生成到数据库`COFFEEBREAK`的连接，并使用它来更新表`COFFEES`中的价格：

```java
import java.sql.*;
import javax.sql.*;
import javax.ejb.*;
import javax.naming.*;

public class ConnectionPoolingBean implements SessionBean {

    // ...

    public void ejbCreate() throws CreateException {
        ctx = new InitialContext();
        ds = (DataSource)ctx.lookup("jdbc/fastCoffeeDB");
    }

    public void updatePrice(float price, String cofName,
                            String username, String password)
        throws SQLException{

        Connection con;
        PreparedStatement pstmt;
        try {
            con = ds.getConnection(username, password);
            con.setAutoCommit(false);
            pstmt = con.prepareStatement("UPDATE COFFEES " +
                        "SET PRICE = ? " +
                        "WHERE COF_NAME = ?");
            pstmt.setFloat(1, price);
            pstmt.setString(2, cofName);
            pstmt.executeUpdate();

            con.commit();
            pstmt.close();

        } finally {
            if (con != null) con.close();
        }
    }

    private DataSource ds = null;
    private Context ctx = null;
}

```

此代码示例中的连接参与连接池化，因为以下条件为真：

+   已部署了一个实现`ConnectionPoolDataSource`的类的实例。

+   已部署了一个实现`DataSource`的类的实例，并为其`dataSourceName`属性设置的值是之前部署的`ConnectionPoolDataSource`对象绑定的逻辑名称。

请注意，尽管这段代码与您之前看到的代码非常相似，但在以下方面有所不同：

+   它导入了`javax.sql`、`javax.ejb`和`javax.naming`包，以及`java.sql`。

    `DataSource`和`ConnectionPoolDataSource`接口位于`javax.sql`包中，JNDI 构造函数`InitialContext`和方法`Context.lookup`属于`javax.naming`包。这个特定的示例代码是以一个使用`javax.ejb`包中的 API 的 EJB 组件的形式呈现的。这个示例的目的是展示您使用池化连接的方式与使用非池化连接的方式相同，因此您不必担心理解 EJB API。

+   它使用`DataSource`对象来获取连接，而不是使用`DriverManager`工具。

+   它使用一个`finally`块来确保连接被关闭。

获取和使用池化连接与获取和使用常规连接类似。当某人作为系统管理员部署了一个`ConnectionPoolDataSource`对象和一个正确的`DataSource`对象时，应用程序将使用该`DataSource`对象来获取池化连接。然而，应用程序应该使用一个`finally`块来关闭池化连接。为简单起见，前面的示例使用了一个`finally`块但没有`catch`块。如果`try`块中的方法抛出异常，它将默认抛出，并且`finally`子句将在任何情况下执行。

## 部署分布式事务

可以部署`DataSource`对象以获取可用于分布式事务的连接。与连接池一样，必须部署两个不同的类实例：一个`XADataSource`对象和一个实现与其一起工作的`DataSource`对象。

假设 The Coffee Break 企业家购买的 EJB 服务器包括`com.applogic.TransactionalDS`类，该类与`com.dbaccess.XATransactionalDS`等`XADataSource`类一起工作。它可以与任何`XADataSource`类一起工作，使 EJB 服务器在 JDBC 驱动程序之间具有可移植性。当部署`DataSource`和`XADataSource`对象时，生成的连接将能够参与分布式事务。在这种情况下，`com.applogic.TransactionalDS`类被实现为生成的连接也是池化连接，这通常是作为 EJB 服务器实现的一部分提供的`DataSource`类的情况。

必须首先部署`XADataSource`对象。以下代码创建`com.dbaccess.XATransactionalDS`的一个实例并设置其属性：

```java
com.dbaccess.XATransactionalDS xads = new com.dbaccess.XATransactionalDS();
xads.setServerName("creamer");
xads.setDatabaseName("COFFEEBREAK");
xads.setPortNumber(9040);
xads.setDescription("Distributed transactions for COFFEEBREAK DBMS");

```

以下代码将`com.dbaccess.XATransactionalDS`对象`*xads*`注册到 JNDI 命名服务。请注意，与`*xads*`关联的逻辑名称在`jdbc`下添加了子上下文`xa`。Oracle 建议`com.dbaccess.XATransactionalDS`类的任何实例的逻辑名称始终以`jdbc/xa`开头。

```java
Context ctx = new InitialContext();
ctx.bind("jdbc/xa/distCoffeeDB", xads);

```

部署实现与`*xads*`和其他`XADataSource`对象交互的`DataSource`对象。请注意，`DataSource`类`com.applogic.TransactionalDS`可以与任何 JDBC 驱动程序供应商的`XADataSource`类一起使用。部署`DataSource`对象涉及创建`com.applogic.TransactionalDS`类的实例并设置其属性。`dataSourceName`属性设置为`jdbc/xa/distCoffeeDB`，这是与`com.dbaccess.XATransactionalDS`关联的逻辑名称。这是实现`DataSource`类的分布式事务能力的`XADataSource`类。以下代码部署`DataSource`类的一个实例：

```java
com.applogic.TransactionalDS ds = new com.applogic.TransactionalDS();
ds.setDescription("Produces distributed transaction " +
                  "connections to COFFEEBREAK");
ds.setDataSourceName("jdbc/xa/distCoffeeDB");
Context ctx = new InitialContext();
ctx.bind("jdbc/distCoffeeDB", ds);

```

现在已经部署了`com.applogic.TransactionalDS`和`com.dbaccess.XATransactionalDS`类的实例，应用程序可以调用`TransactionalDS`类的实例上的`getConnection`方法，以获取连接到`COFFEEBREAK`数据库的连接，该连接可用于分布式事务。

## 使用连接进行分布式事务

要获取可用于分布式事务的连接，必须使用已经正确实现和部署的`DataSource`对象，如部署分布式事务部分所示。使用这样的`DataSource`对象，调用其上的`getConnection`方法。获得连接后，使用它就像使用任何其他连接一样。因为`jdbc/distCoffeesDB`已经与 JNDI 命名服务中的`XADataSource`对象关联，因此以下代码生成一个可用于分布式事务的`Connection`对象：

```java
Context ctx = new InitialContext();
DataSource ds = (DataSource)ctx.lookup("jdbc/distCoffeesDB");
Connection con = ds.getConnection();

```

在连接作为分布式事务的一部分时，对其使用有一些次要但重要的限制。事务管理器控制分布式事务何时开始以及何时提交或回滚；因此，应用程序代码永远不应调用`Connection.commit`或`Connection.rollback`方法。应用程序也不应调用`Connection.setAutoCommit(true)`，这会启用自动提交模式，因为这也会干扰事务管理器对事务边界的控制。这就解释了为什么在分布式事务范围内创建的新连接默认情况下会禁用其自动提交模式。请注意，这些限制仅适用于连接参与分布式事务时；连接不参与分布式事务时没有限制。

对于以下示例，假设已经发货一份咖啡订单，这将触发对驻留在不同 DBMS 服务器上的两个表进行更新。第一个表是一个新的`INVENTORY`表，第二个是`COFFEES`表。因为这些表位于不同的 DBMS 服务器上，涉及它们两个的事务将是一个分布式事务。以下示例中的代码获取连接，更新`COFFEES`表，并关闭连接，是分布式事务的第二部分。

注意，代码并没有显式提交或回滚更新，因为分布式事务的范围由中间层服务器的基础系统基础设施控制。此外，假设用于分布式事务的连接是一个连接池连接，应用程序使用`finally`块来关闭连接。这样可以确保即使抛出异常，也会关闭有效连接，从而确保连接被返回到连接池以进行回收利用。

以下代码示例说明了一个企业 Bean，它是一个实现了可以被客户端计算机调用的方法的类。这个示例的目的是演示分布式事务的应用代码与其他代码没有任何不同，只是它不调用`Connection`方法`commit`、`rollback`或`setAutoCommit(true)`。因此，您不需要担心理解所使用的 EJB API。

```java
import java.sql.*;
import javax.sql.*;
import javax.ejb.*;
import javax.naming.*;

public class DistributedTransactionBean implements SessionBean {

    // ...

    public void ejbCreate() throws CreateException {

        ctx = new InitialContext();
        ds = (DataSource)ctx.lookup("jdbc/distCoffeesDB");
    }

    public void updateTotal(int incr, String cofName, String username,
                            String password)
        throws SQLException {

        Connection con;
        PreparedStatement pstmt;

        try {
            con = ds.getConnection(username, password);
            pstmt = con.prepareStatement("UPDATE COFFEES " +
                        "SET TOTAL = TOTAL + ? " +
                        "WHERE COF_NAME = ?");
            pstmt.setInt(1, incr);
            pstmt.setString(2, cofName);
            pstmt.executeUpdate();
            stmt.close();
        } finally {
            if (con != null) con.close();
        }
    }

    private DataSource ds = null;
    private Context ctx = null;
}

```
