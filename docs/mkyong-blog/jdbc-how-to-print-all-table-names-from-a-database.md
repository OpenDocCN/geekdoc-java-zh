# JDBC——如何打印数据库中的所有表名？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-how-to-print-all-table-names-from-a-database/>

一个 JDBC 的例子，连接到一个 PostgreSQL，并打印出默认数据库中的所有表格`postgres`

pom.xml

```java
 <dependencies>
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>42.2.5</version>
        </dependency>
    </dependencies> 
```

*P.S 用 Java 8 和 postgresql jdbc 驱动程序 42.2.5 测试*

PrintAllTables.java

```java
 package com.mkyong.jdbc;

import java.sql.*;

public class PrintAllTables {

    public static void main(String[] argv) {

        System.out.println("PostgreSQL JDBC Connection Testing ~");

        try {

            Class.forName("org.postgresql.Driver");

        } catch (ClassNotFoundException e) {
            System.err.println("Unable to find the PostgreSQL JDBC Driver!");
            e.printStackTrace();
            return;
        }

        // default database: postgres
        // JDK 7, auto close connection with try-with-resources
        try (Connection connection =
                     DriverManager.getConnection("jdbc:postgresql://127.0.0.1:5432/postgres",
                             "postgres", "password")) {

            DatabaseMetaData metaData = connection.getMetaData();

            try (ResultSet rs = metaData.getTables(null, null, "%", null)) {

                ResultSetMetaData rsMeta = rs.getMetaData();
                int columnCount = rsMeta.getColumnCount();

                while (rs.next()) {

                    System.out.println("\n----------");
                    System.out.println(rs.getString("TABLE_NAME"));
                    System.out.println("----------");

                    for (int i = 1; i <= columnCount; i++) {
                        String columnName = rsMeta.getColumnName(i);
                        System.out.format("%s:%s\n", columnName, rs.getString(i));
                    }

                }
            }

        } catch (SQLException e) {
            System.err.println("Something went wrong!");
            e.printStackTrace();
            return;
        }

    }

} 
```

输出

```java
 ----------
pg_aggregate_fnoid_index
----------
table_cat:null
table_schem:pg_catalog
table_name:pg_aggregate_fnoid_index
table_type:SYSTEM INDEX
remarks:null

----------
pg_am_name_index
----------
table_cat:null
table_schem:pg_catalog
table_name:pg_am_name_index
table_type:SYSTEM INDEX
remarks:null

----------
pg_am_oid_index
----------
table_cat:null
table_schem:pg_catalog
table_name:pg_am_oid_index
table_type:SYSTEM INDEX
remarks:null

----------
pg_amop_fam_strat_index
----------
table_cat:null
table_schem:pg_catalog
table_name:pg_amop_fam_strat_index
table_type:SYSTEM INDEX
remarks:null

----------
pg_amop_oid_index
----------
table_cat:null
table_schem:pg_catalog
table_name:pg_amop_oid_index
table_type:SYSTEM INDEX
remarks:null

----------
pg_amop_opr_fam_index
----------
table_cat:null
table_schem:pg_catalog
table_name:pg_amop_opr_fam_index
table_type:SYSTEM INDEX
remarks:null

//...

----------
pg_stat_progress_vacuum
----------
table_cat:null
table_schem:pg_catalog
table_name:pg_stat_progress_vacuum
table_type:SYSTEM VIEW
remarks:null

----------
pg_stat_replication
----------
table_cat:null
table_schem:pg_catalog
table_name:pg_stat_replication
table_type:SYSTEM VIEW
remarks:null

----------
pg_stat_ssl
----------
table_cat:null
table_schem:pg_catalog
table_name:pg_stat_ssl
table_type:SYSTEM VIEW
remarks:null

//... 
```

## 参考

*   [database metadata::getTables JavaDocs](http://web.archive.org/web/20220216043654/https://docs.oracle.com/javase/8/docs/api/java/sql/DatabaseMetaData.html#getTables-java.lang.String-java.lang.String-java.lang.String-java.lang.String:A-)

<input type="hidden" id="mkyong-current-postId" value="15105">