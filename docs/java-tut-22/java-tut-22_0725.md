# 使用 CachedRowSetObjects

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/cachedrowset.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/cachedrowset.html)

`CachedRowSet`对象很特殊，它可以在不连接到数据源的情况下运行，也就是说，它是一个*断开连接*的`RowSet`对象。它的名字来源于它将数据存储（缓存）在内存中，这样它可以操作自己的数据而不是数据库中存储的数据。

`CachedRowSet`接口是所有断开连接的`RowSet`对象的超级接口，因此这里展示的所有内容也适用于`WebRowSet`、`JoinRowSet`和`FilteredRowSet`对象。

请注意，尽管`CachedRowSet`对象（以及从中派生的`RowSet`对象）的数据源几乎总是关系数据库，但`CachedRowSet`对象能够从以表格格式存储数据的任何数据源获取数据。例如，平面文件或电子表格可以是数据的来源。当为断开连接的`RowSet`对象实现`RowSetReader`对象以从这样的数据源读取数据时，这一点是正确的。`CachedRowSet`接口有一个从关系数据库读取数据的`RowSetReader`对象，因此在本教程中，数据源始终是数据库。

下面涵盖了以下主题：

+   设置 CachedRowSet 对象

+   填充 CachedRowSet 对象

+   Reader 的作用

+   更新 CachedRowSet 对象

+   更新数据源

+   Writer 的作用

+   通知监听器

+   发送大量数据

## 设置 CachedRowSet 对象

设置`CachedRowSet`对象涉及以下内容：

+   创建 CachedRowSet 对象

+   设置 CachedRowSet 属性

+   设置关键列

### 创建 CachedRowSet 对象

通过使用`RowSetProvider`类创建的`RowSetFactory`实例来创建一个新的`CachedRowSet`对象。

以下示例来自`CachedRowSetSample.java`创建了一个`CachedRowSet`对象：

```java
RowSetFactory factory = RowSetProvider.newFactory();
CachedRowSet crs = factory.createCachedRowSet();

```

对象`crs`的属性具有与创建时`JdbcRowSet`对象相同的默认值。此外，它已被分配默认`SyncProvider`实现`RIOptimisticProvider`的实例。

一个`SyncProvider`对象提供了一个`RowSetReader`对象（一个*读取器*）和一个`RowSetWriter`对象（一个*写入器*），一个断开连接的`RowSet`对象需要这些对象来从数据源读取数据或将数据写回数据源。读取器和写入器的功能将在后面的章节读取器的功能和写入器的功能中解释。需要记住的一点是，读取器和写入器完全在后台工作，因此它们如何工作的解释仅供参考。了解读取器和写入器的一些背景知识应该有助于你理解`CachedRowSet`接口中一些方法在后台做什么。

### 设置 CachedRowSet 属性

通常情况下，属性的默认值都是可以的，但是你可以通过调用适当的 setter 方法来更改属性的值。有一些没有默认值的属性，你必须自己设置。

为了获取数据，一个断开连接的`RowSet`对象必须能够连接到数据源，并且有一些选择要保存的数据的方法。以下属性保存了获取数据库连接所需的信息。

+   `username`: 用户在获取访问权限时向数据库提供的名称

+   `password`: 用户的数据库密码

+   `url`: 用户想要连接的数据库的 JDBC URL

+   `datasourceName`: 用于检索已经注册到 JNDI 命名服务的 DataSource 对象的名称

你必须设置哪些属性取决于你将如何建立连接。首选的方式是使用`DataSource`对象，但是你可能无法将`DataSource`对象注册到 JNDI 命名服务中，这通常由系统管理员完成。因此，代码示例都使用`DriverManager`机制来获取连接，你需要使用`url`属性而不是`datasourceName`属性。

以下代码行设置了`username`、`password`和`url`属性，以便使用`DriverManager`类获取连接。（你可以在你的 JDBC 驱动程序的文档中找到要设置为`url`属性值的 JDBC URL。）

```java
public void setConnectionProperties(
    String username, String password) {
    crs.setUsername(username);
    crs.setPassword(password);
    crs.setUrl("jdbc:mySubprotocol:mySubname");
    // ...

```

另一个你必须设置的属性是`command`属性。数据从`ResultSet`对象读入`RowSet`对象。产生该`ResultSet`对象的查询是`command`属性的值。例如，以下代码行使用一个查询设置了`command`属性，该查询产生一个包含表`MERCH_INVENTORY`中所有数据的`ResultSet`对象：

```java
crs.setCommand("select * from MERCH_INVENTORY");

```

### 设置关键列

如果要对 `crs` 对象进行任何更新并希望将这些更新保存到数据库中，必须设置另一个信息：关键列。关键列本质上与主键相同，因为它们指示唯一标识一行的一个或多个列。不同之处在于，主键设置在数据库中的表上，而关键列设置在特定的 `RowSet` 对象上。以下代码行将 `crs` 的关键列设置为第一列：

```java
int[] keys = {1};
crs.setKeyColumns(keys);

```

表 `MERCH_INVENTORY` 中的第一列是 `ITEM_ID`。它可以作为关键列，因为每个项目标识符都不同，因此唯一标识表 `MERCH_INVENTORY` 中的一行且仅一行。此外，该列在 `MERCH_INVENTORY` 表的定义中被指定为主键。方法 `setKeyColumns` 接受一个数组，以允许可能需要两个或更多列来唯一标识一行。

有趣的一点是，方法 `setKeyColumns` 不设置属性的值。在这种情况下，它为字段 `keyCols` 设置值。关键列在内部使用，因此在设置它们之后，您不再对其进行任何操作。您将在 使用 SyncResolver 对象 部分中看到关键列是如何使用的。

## 填充 CachedRowSet 对象

填充断开连接的 `RowSet` 对象比填充连接的 `RowSet` 对象需要更多的工作。幸运的是，额外的工作是在后台完成的。在完成了设置 `CachedRowSet` 对象 `crs` 的初步工作后，以下代码行填充了 `crs`：

```java
crs.execute();

```

`crs` 中的数据是通过执行命令属性中的查询生成的 `ResultSet` 对象中的数据。

不同的是，`CachedRowSet` 实现的 `execute` 方法比 `JdbcRowSet` 实现做了更多的工作。更正确地说，`CachedRowSet` 对象的读取器，该方法委托其任务的对象，做了更多的工作。

每个断开连接的 `RowSet` 对象都分配了一个 `SyncProvider` 对象，并且这个 `SyncProvider` 对象提供了 `RowSet` 对象的 *读取器*（一个 `RowSetReader` 对象）。当创建 `crs` 对象时，它被用作默认的 `CachedRowSetImpl` 构造函数，除了为属性设置默认值外，还将 `RIOptimisticProvider` 实现的实例分配为默认的 `SyncProvider` 对象。

## 读取器的功能

当应用程序调用`execute`方法时，一个断开连接的`RowSet`对象的读取器在后台工作，将`RowSet`对象填充到数据中。新创建的`CachedRowSet`对象未连接到数据源，因此必须获取与该数据源的连接才能从中获取数据。默认的`SyncProvider`对象（`RIOptimisticProvider`）提供一个读取器，通过使用最近设置的用户名、密码和 JDBC URL 或数据源名称中的值来获取连接。然后读取器执行为命令设置的查询。它读取查询生成的`ResultSet`对象中的数据，将`CachedRowSet`对象填充到该数据中。最后，读取器关闭连接。

## 更新 CachedRowSet 对象

在 Coffee Break 场景中，所有者希望简化操作。所有者决定让仓库员工直接在 PDA（个人数字助理）中输入库存，从而避免让第二个人进行数据输入的容易出错的过程。在这种情况下，`CachedRowSet`对象是理想的，因为它轻量级、可序列化，并且可以在没有与数据源连接的情况下进行更新。

所有者将要求应用程序开发团队为仓库员工用于输入库存数据的 PDA 创建一个 GUI 工具。总部将创建一个填充有显示当前库存的表格的`CachedRowSet`对象，并通过互联网将其发送到 PDA。当仓库员工使用 GUI 工具输入数据时，该工具将每个条目添加到一个数组中，`CachedRowSet`对象将使用该数组在后台执行更新。完成库存后，PDA 将其新数据发送回总部，数据将上传到主服务器。

本节涵盖以下主题：

+   更新列值

+   插入和删除行

### 更新列值

更新`CachedRowSet`对象中的数据与更新`JdbcRowSet`对象中的数据完全相同。例如，来自`CachedRowSetSample.java`的以下代码片段将`ITEM_ID`列具有`12345`物品标识符的行中的`QUAN`列的值增加 1：

```java
        while (crs.next()) {
          System.out.println("Found item " + crs.getInt("ITEM_ID") + ": " +
                             crs.getString("ITEM_NAME"));
          if (crs.getInt("ITEM_ID") == 1235) {
            int currentQuantity = crs.getInt("QUAN") + 1;
            System.out.println("Updating quantity to " + currentQuantity);
            crs.updateInt("QUAN", currentQuantity + 1);
            crs.updateRow();
            // Syncing the row back to the DB
            crs.acceptChanges(con);
          }
        } // End of inner while

```

### 插入和删除行

就像更新列值一样，在`CachedRowSet`对象中插入和删除行的代码与`JdbcRowSet`对象相同。

来自`CachedRowSetSample.java`的以下摘录将新行插入到`CachedRowSet`对象`crs`中：

```java
crs.moveToInsertRow();
crs.updateInt("ITEM_ID", newItemId);
crs.updateString("ITEM_NAME", "TableCloth");
crs.updateInt("SUP_ID", 927);
crs.updateInt("QUAN", 14);
Calendar timeStamp;
timeStamp = new GregorianCalendar();
timeStamp.set(2006, 4, 1);
crs.updateTimestamp(
    "DATE_VAL",
    new Timestamp(timeStamp.getTimeInMillis()));
crs.insertRow();
crs.moveToCurrentRow();

```

如果总部决定停止储存某个特定物品，可能会直接删除咖啡本身的行。然而，在这种情况下，使用 PDA 的仓库员工也有能力删除它。以下代码片段找到`ITEM_ID`列中值为`12345`的行，并从`CachedRowSet` `crs`中删除它：

```java
while (crs.next()) {
    if (crs.getInt("ITEM_ID") == 12345) {
        crs.deleteRow();
        break;
    }
}

```

## 更新数据源

对`JdbcRowSet`对象进行更改与对`CachedRowSet`对象进行更改之间存在重大差异。因为`JdbcRowSet`对象连接到其数据源，`updateRow`、`insertRow`和`deleteRow`方法可以更新`JdbcRowSet`对象和数据源。然而，在断开连接的`RowSet`对象的情况下，这些方法会更新`CachedRowSet`对象内存中存储的数据，但无法影响数据源。断开连接的`RowSet`对象必须调用`acceptChanges`方法才能将其更改保存到数据源。在库存场景中，总部的应用程序将调用`acceptChanges`方法以更新数据库中`QUAN`列的新值。

```java
crs.acceptChanges();

```

## 写入器的功能

与`execute`方法类似，`acceptChanges`方法会在后台完成其工作。`execute`方法将其工作委托给`RowSet`对象的读取器，而`acceptChanges`方法将其任务委托给`RowSet`对象的写入器。在后台，写入器会打开与数据库的连接，使用`RowSet`对象所做的更改更新数据库，然后关闭连接。

### 使用默认实现

难点在于可能会出现*冲突*。冲突是指另一方已经更新了数据库中与`RowSet`对象中更新的值对应的值的情况。数据库中应该保留哪个值？当存在冲突时，写入器的处理方式取决于其如何实现，有许多可能性。在一个极端，写入器甚至不检查冲突，只是将所有更改写入数据库。这是`RIXMLProvider`实现的情况，它被`WebRowSet`对象使用。在另一端，写入器通过设置数据库锁来确保没有冲突，防止他人进行更改。

`crs`对象的写入器是默认`SyncProvider`实现提供的一个，名为`RIOptimisticProvider`。`RIOPtimisticProvider`实现得名于其采用的乐观并发模型。该模型假设冲突会很少，甚至没有，因此不设置数据库锁。写入器会检查是否存在冲突，如果没有，则将对`crs`对象所做的更改写入数据库，这些更改变得持久。如果存在冲突，默认情况下不会将新的`RowSet`值写入数据库。

在这种情况下，默认行为非常有效。因为总部的人不太可能更改`COF_INVENTORY`的`QUAN`列中的值，所以不会发生冲突。因此，在仓库中输入到`crs`对象中的值将被写入数据库，从而变得持久，这是期望的结果。

## 使用 SyncResolver 对象

然而，在其他情况下，可能存在冲突。为了适应这些情况，`RIOPtimisticProvider`实现提供了一个选项，让你查看冲突中的值，并决定哪些值应该持久化。这个选项就是使用`SyncResolver`对象。

当写入程序完成查找冲突并找到一个或多个冲突时，它会创建一个包含导致冲突的数据库值的`SyncResolver`对象。接下来，方法`acceptChanges`抛出一个`SyncProviderException`对象，应用程序可以捕获并用于检索`SyncResolver`对象。以下代码行检索`SyncResolver`对象`resolver`：

```java
try {
    crs.acceptChanges();
} catch (SyncProviderException spe) {
    SyncResolver resolver = spe.getSyncResolver();
}

```

对象`resolver`是一个`RowSet`对象，复制了`crs`对象，只包含导致冲突的数据库中的值。所有其他列值都为 null。

使用`resolver`对象，你可以迭代其行以定位不为空且因此引起冲突的值。然后你可以定位`crs`对象中相同位置的值并进行比较。以下代码片段检索`resolver`并使用`SyncResolver`方法`nextConflict`来迭代具有冲突值的行。对象`resolver`获取每个冲突值的状态，如果是`UPDATE_ROW_CONFLICT`，表示`crs`在冲突发生时正在尝试更新，则`resolver`对象获取该值的行号。然后代码将`crs`对象的游标移动到相同的行。接下来，代码找到`resolver`对象中包含冲突值的行中的列，该列将是一个不为空的值。从`resolver`和`crs`对象中检索该列中的值后，你可以比较两者并决定哪个值应该持久化。最后，代码使用`setResolvedValue`方法在`crs`对象和数据库中设置该值，如下所示来自`CachedRowSetSample.java`的代码：

```java
    try {
        // ...
        // Syncing the new row back to the database.
        System.out.println("About to add a new row...");
        crs.acceptChanges(con);
        System.out.println("Added a row...");
        this.viewTable(con);
        // ...
    } catch (SyncProviderException spe) {

      SyncResolver resolver = spe.getSyncResolver();

      Object crsValue; // value in the RowSet object
      Object resolverValue; // value in the SyncResolver object
      Object resolvedValue; // value to be persisted

      while (resolver.nextConflict()) {

        if (resolver.getStatus() == SyncResolver.INSERT_ROW_CONFLICT) {
          int row = resolver.getRow();
          crs.absolute(row);

          int colCount = crs.getMetaData().getColumnCount();
          for (int j = 1; j <= colCount; j++) {
            if (resolver.getConflictValue(j) != null) {
              crsValue = crs.getObject(j);
              resolverValue = resolver.getConflictValue(j);

              // Compare crsValue and resolverValue to determine
              // which should be the resolved value (the value to persist)
              //
              // This example chooses the value in the RowSet object,
              // crsValue, to persist.,

              resolvedValue = crsValue;

              resolver.setResolvedValue(j, resolvedValue);
            }
          }
        }
      }
    }

```

## 通知监听器

作为 JavaBeans 组件意味着`RowSet`对象在发生某些事情时可以通知其他组件。例如，如果`RowSet`对象中的数据发生变化，`RowSet`对象可以通知感兴趣的方。这种通知机制的好处在于，作为应用程序员，你只需添加或移除将被通知的组件。

本节涵盖以下主题：

+   设置监听器

+   通知工作原理

### 设置监听器

一个`RowSet`对象的*监听器*是一个实现`RowSetListener`接口的组件，该接口包括以下方法：

+   `cursorMoved`：定义了当`RowSet`对象中的游标移动时，监听器将执行什么操作（如果有的话）。

+   `rowChanged`：定义了监听器在一行中一个或多个列值发生变化、插入了一行或删除了一行时将执行的操作（如果有的话）。

+   `rowSetChanged`：定义了监听器在`RowSet`对象被填充新数据时将执行的操作（如果有的话）。

一个可能想要成为监听器的组件示例是一个将`RowSet`对象中的数据制成图表的`BarGraph`对象。随着数据的变化，`BarGraph`对象可以更新自身以反映新数据。

作为一个应用程序员，利用通知机制的唯一要做的事情就是添加或移除监听器。下面这行代码意味着每当`crs`对象的光标移动，`crs`中的值发生变化，或者整个`crs`获取新数据时，`BarGraph`对象`bar`都会收到通知：

```java
crs.addRowSetListener(bar);

```

你也可以通过移除监听器来停止通知，就像下面这行代码所做的那样：

```java
crs.removeRowSetListener(bar);

```

使用咖啡休息场景，假设总部定期检查数据库以获取在线销售的咖啡最新价格列表。在这种情况下，监听器是咖啡休息网站上的`PriceList`对象`priceList`，它必须实现`RowSetListener`方法`cursorMoved`、`rowChanged`和`rowSetChanged`。`cursorMoved`方法的实现可能是什么都不做，因为光标的位置不会影响`priceList`对象。另一方面，`rowChanged`和`rowSetChanged`方法的实现必须确定所做的更改，并相应地更新`priceList`。

### 通知如何工作

导致任何`RowSet`事件的方法会自动通知所有注册的监听器。例如，任何移动光标的方法也会调用每个监听器的`cursorMoved`方法。同样，`execute`方法会调用所有监听器的`rowSetChanged`方法，而`acceptChanges`会调用所有监听器的`rowChanged`方法。

## 发送大量数据

该方法`CachedRowSetSample.java`演示了如何将数据分成较小的部分发送。
