# JDBC 在条件中准备了 SQL 语句

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jdbc/jdbc-preparedstatement-sql-in-condition/>

Java JDBC `PreparedStatement`示例创建一个 SQL IN 条件。

## 1.PreparedStatement +数组

在 JDBC，我们可以在查询中使用`createArrayOf`创建一个`PreparedStatement`。

```java
 @Override
    public List<Integer> getPostIdByTagId(List<Integer> tagIds) {

        List<Integer> result = new ArrayList<>();

        String sql = "SELECT tr.object_id as post_id FROM wp_term_relationships tr " +
                " JOIN wp_term_taxonomy tt JOIN wp_terms t " +
                " ON tr.term_taxonomy_id = tt.term_taxonomy_id " +
                " AND tt.term_id = t.term_id " +
                " WHERE t.term_id IN (?) AND tt.taxonomy= 'post_tag'";

        try (Connection connection = dataSource.getConnection();
             PreparedStatement ps = connection.prepareStatement(sql)) {

            Array tagIdsInArray = connection.createArrayOf("integer", tagIds.toArray());
            ps.setArray(1, tagIdsInArray);

            try (ResultSet rs = ps.executeQuery()) {
                while (rs.next()) {
                    result.add(rs.getInt("post_id"));
                }
            }

        } catch (SQLException e) {
            logger.error("Unknown error : {}", e);
        }

        return result;

    } 
```

但是，这种`array`型并不是标准的 JDBC 选项。如果我们用 MYSQL 运行这个，它将提示以下错误消息:

```java
 java.sql.SQLFeatureNotSupportedException 
```

MySQL 不支持`array`类型，测试用

pom.xml

```java
 <dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>5.1.47</version>
	</dependency> 
```

## 2 准备好的报表+连接

这个版本可以在 MySQL 和任何支持 SQL 的数据库中运行，不需要魔法，只需要手工加入和替换`(?)`

```java
 @Override
    public List<Integer> getPostIdByTagId(List<Integer> tagIds) {

        List<Integer> result = new ArrayList<>();

        String sql = "SELECT tr.object_id as post_id FROM wp_term_relationships tr " +
                " JOIN wp_term_taxonomy tt JOIN wp_terms t " +
                " ON tr.term_taxonomy_id = tt.term_taxonomy_id " +
                " AND tt.term_id = t.term_id " +
                " WHERE t.term_id IN (?) AND tt.taxonomy= 'post_tag'";

        String sqlIN = tagIds.stream()
			.map(x -> String.valueOf(x))
			.collect(Collectors.joining(",", "(", ")"));

        sql = sql.replace("(?)", sqlIN);

        try (Connection connection = dataSource.getConnection();
             PreparedStatement ps = connection.prepareStatement(sql)) {

            try (ResultSet rs = ps.executeQuery()) {
                while (rs.next()) {
                    result.add(rs.getInt("post_id"));
                }
            }

        } catch (SQLException e) {
            logger.error("Unknown error : {}", e);
        }

        return result;

    } 
```

## 参考

*   [prepared statement Java docs](http://web.archive.org/web/20230101144831/https://docs.oracle.com/javase/8/docs/api/java/sql/PreparedStatement.html)
*   [Java 8–string joiner 示例–Mkyong.com](/web/20230101144831/https://mkyong.com/java8/java-8-stringjoiner-example/)

<input type="hidden" id="mkyong-current-postId" value="14980">