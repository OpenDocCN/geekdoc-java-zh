# SimpleJdbcTemplate 中的 Spring 命名参数示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-named-parameters-examples-in-simplejdbctemplate/>

在 JdbcTemplate 中，SQL 参数由一个特殊的占位符“**”表示？**"符号并按位置装订。问题是每当参数的顺序改变时，你也必须改变参数绑定，这很容易出错，而且维护起来很麻烦。

要解决这个问题，您可以使用“**命名的参数**，而 SQL 参数是由一个冒号加一个名称来定义的，而不是由位置来定义的。另外，命名参数仅在 **SimpleJdbcTemplate** 和**namedparametejdbctemplate**中受支持。

参见下面三个在 Spring 中使用命名参数的例子。

## 示例 1

演示如何在单个 insert 语句中使用命名参数的示例。

```java
 //insert with named parameter
	public void insertNamedParameter(Customer customer){

		String sql = "INSERT INTO CUSTOMER " +
			"(CUST_ID, NAME, AGE) VALUES (:custId, :name, :age)";

		Map<String, Object> parameters = new HashMap<String, Object>();
		parameters.put("custId", customer.getCustId());
		parameters.put("name", customer.getName());
		parameters.put("age", customer.getAge());

		getSimpleJdbcTemplate().update(sql, parameters);

	} 
```

## 示例 2

演示如何在批处理操作语句中使用命名参数的示例。

```java
 public void insertBatchNamedParameter(final List<Customer> customers){

		String sql = "INSERT INTO CUSTOMER " +
		"(CUST_ID, NAME, AGE) VALUES (:custId, :name, :age)";

		List<SqlParameterSource> parameters = new ArrayList<SqlParameterSource>();
		for (Customer cust : customers) {
			parameters.add(new BeanPropertySqlParameterSource(cust)); 
		}

		getSimpleJdbcTemplate().batchUpdate(sql,
			parameters.toArray(new SqlParameterSource[0]));
	} 
```

## 示例 3

在批处理操作语句中使用命名参数的另一个例子。

```java
 public void insertBatchNamedParameter2(final List<Customer> customers){

	   SqlParameterSource[] params = 
		SqlParameterSourceUtils.createBatch(customers.toArray());

	   getSimpleJdbcTemplate().batchUpdate(
		"INSERT INTO CUSTOMER (CUST_ID, NAME, AGE) VALUES (:custId, :name, :age)",
		params);

	} 
```

## 下载源代码

Download it – [Spring-JDBC-Named-Parameter-Example.zip](http://web.archive.org/web/20210820135911/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-JDBC-Example.zip) (15 KB)Tags : [jdbc](http://web.archive.org/web/20210820135911/https://mkyong.com/tag/jdbc/) [spring](http://web.archive.org/web/20210820135911/https://mkyong.com/tag/spring/)<input type="hidden" id="mkyong-current-postId" value="3843">