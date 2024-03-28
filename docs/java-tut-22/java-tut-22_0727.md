# 使用 FilteredRowSet 对象

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/filteredrowset.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/filteredrowset.html)

`FilteredRowSet`对象允许您减少在`RowSet`对象中可见的行数，以便您只处理与您正在进行的工作相关的数据。您决定要对数据设置什么限制（如何“过滤”数据），并将该过滤器应用到`FilteredRowSet`对象上。换句话说，`FilteredRowSet`对象只显示符合您设置限制的数据行。`JdbcRowSet`对象始终与其数据源连接，可以通过向数据源发送仅选择您想要查看的列和行的查询来进行此过滤。查询的`WHERE`子句定义了过滤条件。`FilteredRowSet`对象提供了一种让断开连接的`RowSet`对象进行此过滤的方法，而无需在数据源上执行查询，从而避免必须连接到数据源并向其发送查询。

例如，假设咖啡休息连锁店已经在美利坚合众国各地发展到数百家店铺，并且所有店铺都列在名为`COFFEE_HOUSES`的表中。业主希望通过一款不需要与数据库系统保持持久连接的咖啡店比较应用程序来衡量仅加利福尼亚州的店铺的成功。这种比较将关注销售商品与销售咖啡饮料的盈利能力以及其他各种成功指标，并将按照咖啡饮料销售额、商品销售额和总销售额对加利福尼亚州的店铺进行排名。由于`COFFEE_HOUSES`表有数百行数据，如果将搜索的数据量减少到仅包含`STORE_ID`列中指示加利福尼亚州的行，这些比较将更快更容易进行。

这正是`FilteredRowSet`对象通过提供以下功能来解决的问题：

+   能够根据设定的条件限制可见的行

+   能够选择哪些数据可见而无需连接到数据源

下面的主题包括：

+   在谓词对象中定义过滤条件

+   创建 FilteredRowSet 对象

+   创建和设置谓词对象

+   使用新的谓词对象设置 FilteredRowSet 对象以进一步过滤数据

+   更新 FilteredRowSet 对象

+   插入或更新行

+   删除所有过滤器以使所有行可见

+   删除行

## 在谓词对象中定义过滤条件

要设置`FilteredRowSet`对象中哪些行可见的条件，您需要定义一个实现`Predicate`接口的类。使用此类创建的对象将初始化为以下内容：

+   值必须落在的范围的高端

+   值必须落在的范围的低端

+   列名或列号是必须落在高低边界设置的值范围内的值所在的列的列名或列号

请注意，值范围是包容的，这意味着边界处的值包括在范围内。例如，如果范围的高端为 100，低端为 50，则 50 的值被视为在范围内。49 不在范围内。同样，100 在范围内，但 101 不在范围内。

符合业主想要比较加利福尼亚店铺的情景，必须编写一个实现`Predicate`接口的过滤位于加利福尼亚的 Coffee Break 咖啡店的类。没有一种正确的方法来做这件事，这意味着在编写实现的方式上有很大的自由度。例如，您可以随意命名类及其成员，并以任何实现方式编写构造函数和三个评估方法，以实现所需的结果。

列出所有咖啡店的表名为`COFFEE_HOUSES`，有数百行。为了使事情更易管理，此示例使用了少得多的行数的表，足以演示如何进行过滤。

列`STORE_ID`中的值是一个`int`值，表示咖啡店所在的州等信息。例如，以 10 开头的值表示该州是加利福尼亚。以 32 开头的`STORE_ID`值表示俄勒冈州，以 33 开头的表示华盛顿州。

以下类，`StateFilter`，实现了`Predicate`接口：

```java
public class StateFilter implements Predicate {

    private int lo;
    private int hi;
    private String colName = null;
    private int colNumber = -1;

    public StateFilter(int lo, int hi, int colNumber) {
        this.lo = lo;
        this.hi = hi;
        this.colNumber = colNumber;
    }

    public StateFilter(int lo, int hi, String colName) {
        this.lo = lo;
        this.hi = hi;
        this.colName = colName;
    }

    public boolean evaluate(Object value, String columnName) {
        boolean evaluation = true;
        if (columnName.equalsIgnoreCase(this.colName)) {
            int columnValue = ((Integer)value).intValue();
            if ((columnValue >= this.lo)
                &&
                (columnValue <= this.hi)) {
                evaluation = true;
            } else {
                evaluation = false;
            }
        }
        return evaluation;
    }

    public boolean evaluate(Object value, int columnNumber) {

        boolean evaluation = true;

        if (this.colNumber == columnNumber) {
            int columnValue = ((Integer)value).intValue();
            if ((columnValue >= this.lo)
                &&
                (columnValue <= this.hi)) {
                evaluation = true;
            } else {
                evaluation = false;
            }
        }
        return evaluation;
    }

    public boolean evaluate(RowSet rs) {

        CachedRowSet frs = (CachedRowSet)rs;
        boolean evaluation = false;

        try {
            int columnValue = -1;

            if (this.colNumber > 0) {
                columnValue = frs.getInt(this.colNumber);
            } else if (this.colName != null) {
                columnValue = frs.getInt(this.colName);
            } else {
                return false;
            }

            if ((columnValue >= this.lo)
                &&
                (columnValue <= this.hi)) {
                evaluation = true;
            }
        } catch (SQLException e) {
            JDBCTutorialUtilities.printSQLException(e);
            return false;
        } catch (NullPointerException npe) {
            System.err.println("NullPointerException caught");
            return false;
        }
        return evaluation;
    }
}

```

这是一个非常简单的实现，检查由`colName`或`colNumber`指定的列中的值是否在`lo`到`hi`的范围内，包括边界。以下代码行，来自`FilteredRowSetSample.java`，创建了一个过滤器，仅允许`STORE_ID`列值指示介于 10000 和 10999 之间的行，这表示加利福尼亚位置：

```java
StateFilter myStateFilter = new StateFilter(10000, 10999, 1);

```

请注意，刚刚定义的`StateFilter`类仅适用于一列。可以通过将每个参数数组而不是单个值来使其适用于两个或更多列。例如，`Filter`对象的构造函数可能如下所示：

```java
public Filter2(Object [] lo, Object [] hi, Object [] colNumber) {
    this.lo = lo;
    this.hi = hi;
    this.colNumber = colNumber;
}

```

`colNumber`对象中的第一个元素表示将根据其在`lo`和`hi`中的第一个元素进行检查的第一列。由`colNumber`指示的第二列中的值将与`lo`和`hi`中的第二个元素进行检查，依此类推。因此，这三个数组中的元素数量应该相同。下面的代码是`evaluate(RowSet rs)`方法的一个实现示例，用于`Filter2`对象，其中参数是数组：

```java
public boolean evaluate(RowSet rs) {
    CachedRowSet crs = (CachedRowSet)rs;
    boolean bool1;
    boolean bool2;
    for (int i = 0; i < colNumber.length; i++) {

        if ((rs.getObject(colNumber[i] >= lo [i]) &&
            (rs.getObject(colNumber[i] <= hi[i]) {
            bool1 = true;
        } else {
            bool2 = true;
        }

        if (bool2) {
            return false;
        } else {
            return true;
        }
    }
}

```

使用`Filter2`实现的优势在于可以使用任何`Object`类型的参数，并且可以检查一个或多个列，而无需编写另一个实现。但是，您必须传递一个`Object`类型，这意味着您必须将原始类型转换为其`Object`类型。例如，如果您使用`int`值作为`lo`和`hi`，则必须在将其传递给构造函数之前将`int`值转换为`Integer`对象。`String`对象已经是`Object`类型，因此您无需转换它们。

## 创建 FilteredRowSet 对象

使用从`RowSetProvider`类创建的`RowSetFactory`实例来创建`FilteredRowSet`对象。以下是来自`FilteredRowSetSample.java`的示例：

```java
    RowSetFactory factory = RowSetProvider.newFactory();
    try (FilteredRowSet frs = factory.createFilteredRowSet()) {
      // ...

```

与其他断开连接的`RowSet`对象一样，`frs`对象必须从表格数据源（在参考实现中是关系数据库）中填充自身数据。下面来自`FilteredRowSetSample.java`的代码片段设置了连接到数据库以执行其命令所需的属性。请注意，此代码使用`DriverManager`类建立连接，这是为了方便起见。通常情况下，最好使用已在实现了 Java 命名和目录接口（JNDI）的命名服务中注册的`DataSource`对象。

```java
    RowSetFactory factory = RowSetProvider.newFactory();
    try (FilteredRowSet frs = factory.createFilteredRowSet()){
      frs.setCommand("SELECT * FROM COFFEE_HOUSES");
      frs.setUsername(settings.userName);
      frs.setPassword(settings.password);
      frs.setUrl(settings.urlString);
      frs.execute();
      // ...

```

下面的代码行将`frs`对象填充了存储在`COFFEE_HOUSE`表中的数据：

```java
frs.execute();

```

`execute`方法通过调用`frs`的`RowSetReader`对象在后台执行各种操作，该对象创建连接，执行`frs`的命令，将`frs`填充为从生成的`ResultSet`对象中获取的数据，并关闭连接。请注意，如果`COFFEE_HOUSES`表的行数超过了`frs`对象一次性内存中可以容纳的行数，将使用`CachedRowSet`的分页方法。

在这种情况下，Coffee Break 的所有者将在办公室完成前述任务，然后导入或下载存储在`frs`对象中的信息到咖啡馆比较应用程序中。从现在开始，`frs`对象将独立运行，不再依赖于与数据源的连接。

## 创建和设置谓词对象

现在`FilteredRowSet`对象`frs`包含了 Coffee Break 商店的列表，您可以为`frs`对象中可见的行数设置选择条件以缩小范围。

以下代码行使用先前定义的`StateFilter`类来创建对象`myStateFilter`，该对象检查`STORE_ID`列以确定哪些商店位于加利福尼亚（如果其 ID 号在 10000 到 10999 之间，则商店位于加利福尼亚）：

```java
StateFilter myStateFilter = new StateFilter(10000, 10999, 1);

```

以下行将`myStateFilter`设置为`frs`的过滤器。

```java
frs.setFilter(myStateFilter);

```

要进行实际过滤，您调用`next`方法，该方法在参考实现中调用您先前实现的`Predicate.evaluate`方法的适当版本。

如果返回值为`true`，则该行将可见；如果返回值为`false`，则该行将不可见。

## 使用新的`Predicate`对象设置 FilteredRowSet 对象以进一步过滤数据

您可以串行设置多个过滤器。第一次调用`setFilter`方法并传递一个`Predicate`对象时，您已经应用了该过滤器中的过滤条件。在对每一行调用`next`方法后，只有满足过滤条件的行才会可见，然后您可以再次调用`setFilter`，传递不同的`Predicate`对象。尽管一次只设置一个过滤器，但效果是两个过滤器都会累积应用。

例如，所有者通过将`stateFilter`设置为`frs`的`Predicate`对象来检索加利福尼亚的 Coffee Break 商店列表。现在所有者想要比较加利福尼亚的两个城市中的商店，旧金山（表`COFFEE_HOUSES`中的 SF）和洛杉矶（表中的 LA）。首先要做的是编写一个`Predicate`实现，用于过滤旧金山或洛杉矶的商店：

```java
public class CityFilter implements Predicate {

    private String[] cities;
    private String colName = null;
    private int colNumber = -1;

    public CityFilter(String[] citiesArg, String colNameArg) {
        this.cities = citiesArg;
        this.colNumber = -1;
        this.colName = colNameArg;
    }

    public CityFilter(String[] citiesArg, int colNumberArg) {
        this.cities = citiesArg;
        this.colNumber = colNumberArg;
        this.colName = null;
    }

    public boolean evaluate Object valueArg, String colNameArg) {

        if (colNameArg.equalsIgnoreCase(this.colName)) {
            for (int i = 0; i < this.cities.length; i++) {
                if (this.cities[i].equalsIgnoreCase((String)valueArg)) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean evaluate(Object valueArg, int colNumberArg) {

        if (colNumberArg == this.colNumber) {
            for (int i = 0; i < this.cities.length; i++) {
                if (this.cities[i].equalsIgnoreCase((String)valueArg)) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean evaluate(RowSet rs) {

        if (rs == null) return false;

        try {
            for (int i = 0; i < this.cities.length; i++) {

                String cityName = null;

                if (this.colNumber > 0) {
                    cityName = (String)rs.getObject(this.colNumber);
                } else if (this.colName != null) {
                    cityName = (String)rs.getObject(this.colName);
                } else {
                    return false;
                }

                if (cityName.equalsIgnoreCase(cities[i])) {
                    return true;
                }
            }
        } catch (SQLException e) {
            return false;
        }
        return false;
    }
}

```

来自`FilteredRowSetSample.java`的以下代码片段设置了新的过滤器，并遍历`frs`中的行，打印出`CITY`列包含旧金山或洛杉矶的行。请注意，`frs`当前仅包含商店位于加利福尼亚的行，因此当将过滤器更改为另一个`Predicate`对象时，`state`对象的条件仍然有效。接下来的代码将过滤器设置为`CityFilter`对象`city`。`CityFilter`实现使用数组作为构造函数的参数，以说明可以如何完成：

```java
  public void testFilteredRowSet() throws SQLException {

    StateFilter myStateFilter = new StateFilter(10000, 10999, 1);
    String[] cityArray = { "SF", "LA" };

    CityFilter myCityFilter = new CityFilter(cityArray, 2);

	RowSetFactory factory = RowSetProvider.newFactory();

    try (FilteredRowSet frs = factory.createFilteredRowSet()){
      frs.setCommand("SELECT * FROM COFFEE_HOUSES");
      frs.setUsername(settings.userName);
      frs.setPassword(settings.password);
      frs.setUrl(settings.urlString);
      frs.execute();

      System.out.println("\nBefore filter:");
      FilteredRowSetSample.viewTable(this.con);

      System.out.println("\nSetting state filter:");
      frs.beforeFirst();
      frs.setFilter(myStateFilter);
      this.viewFilteredRowSet(frs);

      System.out.println("\nSetting city filter:");
      frs.beforeFirst();
      frs.setFilter(myCityFilter);
      this.viewFilteredRowSet(frs);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

输出应包含每个位于加利福尼亚旧金山或洛杉矶的商店的行。如果有一行中`CITY`列包含 LA 且`STORE_ID`列包含 40003，则不会包含在列表中，因为在将过滤器设置为`state`时已经被过滤掉（40003 不在 10000 到 10999 的范围内）。

## 更新 FilteredRowSet 对象

您可以对`FilteredRowSet`对象进行更改，但前提是该更改不违反当前生效的任何过滤条件。例如，如果新值或值在过滤条件内，则可以插入新行或更改现有行中的一个或多个值。

## 插入或更新行

假设两家新的 Coffee Break 咖啡馆刚刚开业，所有者希望将它们添加到所有咖啡馆的列表中。如果要插入的行不符合当前累积的过滤条件，则将阻止其添加。

`frs`对象的当前状态是设置了`StateFilter`对象，然后设置了`CityFilter`对象。因此，`frs`目前仅显示符合两个过滤器条件的行。同样重要的是，除非符合两个过滤器的条件，否则无法向`frs`对象添加行。以下代码片段尝试向`frs`对象插入两行新行，其中一个行中的`STORE_ID`和`CITY`列的值都符合条件，另一个行中的`STORE_ID`的值不符合过滤条件，但`CITY`列的值符合：

```java
frs.moveToInsertRow();
frs.updateInt("STORE_ID", 10101);
frs.updateString("CITY", "SF");
frs.updateLong("COF_SALES", 0);
frs.updateLong("MERCH_SALES", 0);
frs.updateLong("TOTAL_SALES", 0);
frs.insertRow();

frs.updateInt("STORE_ID", 33101);
frs.updateString("CITY", "SF");
frs.updateLong("COF_SALES", 0);
frs.updateLong("MERCH_SALES", 0);
frs.updateLong("TOTAL_SALES", 0);
frs.insertRow();
frs.moveToCurrentRow();

```

如果使用方法`next`迭代`frs`对象，你会发现旧金山、加利福尼亚州的新咖啡馆的行，但不会看到华盛顿州旧金山的商店的行。

## 删除所有过滤器以使所有行可见

所有者可以通过取消过滤器来添加华盛顿州的商店。没有设置过滤器，`frs`对象中的所有行再次可见，任何位置的商店都可以添加到商店列表中。以下代码行取消当前过滤器，有效地使先前在`frs`对象上设置的两个`Predicate`实现无效。

```java
frs.setFilter(null);

```

## 删除行

如果所有者决定关闭或出售其中一家 Coffee Break 咖啡馆，所有者将希望从`COFFEE_HOUSES`表中删除它。只要行可见，所有者就可以删除表现不佳的咖啡馆的行。

例如，假设刚刚使用参数 null 调用了方法`setFilter`，则`frs`对象上没有设置任何过滤器。这意味着所有行都是可见的，因此可以删除。然而，在设置了过滤掉除加利福尼亚州以外任何州的`StateFilter`对象`myStateFilter`之后，只有位于加利福尼亚州的商店才能被删除。当为`frs`对象设置了`CityFilter`对象`myCityFilter`时，只有旧金山、加利福尼亚州或洛杉矶、加利福尼亚州的咖啡馆可以被删除，因为它们是唯一可见的行。
