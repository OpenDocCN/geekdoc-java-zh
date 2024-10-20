# spring+JDBC template+JdbcDaoSupport 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-jdbctemplate-jdbcdaosupport-examples/>

在 Spring JDBC 开发中，您可以使用`JdbcTemplate`和`JdbcDaoSupport`类来简化整个数据库操作过程。

在本教程中，我们将重用最后一个 [Spring + JDBC 示例](http://web.archive.org/web/20221225035543/http://www.mkyong.com/spring/maven-spring-jdbc-example/)，以查看之前(不支持 JdbcTemplate)和之后(支持 JdbcTemplate)示例之间的不同。

## 1.没有 JdbcTemplate 的示例

如果没有 JdbcTemplate，您必须在所有 DAO 数据库操作方法(插入、更新和删除)中创建许多冗余代码(创建连接、关闭连接、处理异常)。它效率不高，难看，容易出错，而且单调乏味。

```java
 private DataSource dataSource;

	public void setDataSource(DataSource dataSource) {
		this.dataSource = dataSource;
	}

	public void insert(Customer customer){

		String sql = "INSERT INTO CUSTOMER " +
				"(CUST_ID, NAME, AGE) VALUES (?, ?, ?)";
		Connection conn = null;

		try {
			conn = dataSource.getConnection();
			PreparedStatement ps = conn.prepareStatement(sql);
			ps.setInt(1, customer.getCustId());
			ps.setString(2, customer.getName());
			ps.setInt(3, customer.getAge());
			ps.executeUpdate();
			ps.close();

		} catch (SQLException e) {
			throw new RuntimeException(e);

		} finally {
			if (conn != null) {
				try {
					conn.close();
				} catch (SQLException e) {}
			}
		}
	} 
```

## 2.JdbcTemplate 示例

使用 JdbcTemplate，您可以节省大量的冗余代码，因为 JdbcTemplate 会自动处理它。

```java
 private DataSource dataSource;
	private JdbcTemplate jdbcTemplate;

	public void setDataSource(DataSource dataSource) {
		this.dataSource = dataSource;
	}

	public void insert(Customer customer){

		String sql = "INSERT INTO CUSTOMER " +
			"(CUST_ID, NAME, AGE) VALUES (?, ?, ?)";

		jdbcTemplate = new JdbcTemplate(dataSource);

		jdbcTemplate.update(sql, new Object[] { customer.getCustId(),
			customer.getName(),customer.getAge()  
		});

	} 
```

看到不同了吗？

## 3.JdbcDaoSupport 示例

通过扩展 JdbcDaoSupport，不再需要在您的类中设置 datasource 和 JdbcTemplate，您只需要将正确的 datasource 注入到 JdbcCustomerDAO 中。您可以通过使用 getJdbcTemplate()方法获得 JdbcTemplate。

```java
 public class JdbcCustomerDAO extends JdbcDaoSupport implements CustomerDAO
	{
	   //no need to set datasource here
	   public void insert(Customer customer){

		String sql = "INSERT INTO CUSTOMER " +
			"(CUST_ID, NAME, AGE) VALUES (?, ?, ?)";

		getJdbcTemplate().update(sql, new Object[] { customer.getCustId(),
				customer.getName(),customer.getAge()  
		});

	} 
```

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="dataSource" 
         class="org.springframework.jdbc.datasource.DriverManagerDataSource">

		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://localhost:3306/mkyongjava" />
		<property name="username" value="root" />
		<property name="password" value="password" />
	</bean>

</beans> 
```

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customerDAO" class="com.mkyong.customer.dao.impl.JdbcCustomerDAO">
		<property name="dataSource" ref="dataSource" />
	</bean>

</beans> 
```

**Note**
In Spring JDBC development, it’s always recommended to use `JdbcTemplate` and `JdbcDaoSupport`, instead of coding JDBC code yourself.

## 下载源代码

Download it – [Spring-JDBC-Example.zip](http://web.archive.org/web/20221225035543/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-JDBC-Example.zip) (15 KB)<input type="hidden" id="mkyong-current-postId" value="3773">