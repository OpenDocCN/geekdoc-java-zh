# Java 8 Lambda:比较器示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-lambda-comparator-example/>

![java-lambda-expression](img/2eb586be36897017fa8cdfd5f9bb7195.png)

在本例中，我们将向您展示如何使用 Java 8 Lambda 表达式编写一个`Comparator`来对列表进行排序。

1.经典`Comparator`例子。

```java
 Comparator<Developer> byName = new Comparator<Developer>() {
		@Override
		public int compare(Developer o1, Developer o2) {
			return o1.getName().compareTo(o2.getName());
		}
	}; 
```

2.Lambda 表达式等价。

```java
 Comparator<Developer> byName = 
		(Developer o1, Developer o2)->o1.getName().compareTo(o2.getName()); 
```

## 1.不使用 Lambda 排序

使用年龄比较`Developer`对象的示例。通常，您使用`Collections.sort`并像这样传递一个匿名的`Comparator`类:

TestSorting.java

```java
 package com.mkyong.java8;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class TestSorting {

	public static void main(String[] args) {

		List<Developer> listDevs = getDevelopers();

		System.out.println("Before Sort");
		for (Developer developer : listDevs) {
			System.out.println(developer);
		}

		//sort by age
		Collections.sort(listDevs, new Comparator<Developer>() {
			@Override
			public int compare(Developer o1, Developer o2) {
				return o1.getAge() - o2.getAge();
			}
		});

		System.out.println("After Sort");
		for (Developer developer : listDevs) {
			System.out.println(developer);
		}

	}

	private static List<Developer> getDevelopers() {

		List<Developer> result = new ArrayList<Developer>();

		result.add(new Developer("mkyong", new BigDecimal("70000"), 33));
		result.add(new Developer("alvin", new BigDecimal("80000"), 20));
		result.add(new Developer("jason", new BigDecimal("100000"), 10));
		result.add(new Developer("iris", new BigDecimal("170000"), 55));

		return result;

	}

} 
```

输出

```java
 Before Sort
Developer [name=mkyong, salary=70000, age=33]
Developer [name=alvin, salary=80000, age=20]
Developer [name=jason, salary=100000, age=10]
Developer [name=iris, salary=170000, age=55]

After Sort
Developer [name=jason, salary=100000, age=10]
Developer [name=alvin, salary=80000, age=20]
Developer [name=mkyong, salary=70000, age=33]
Developer [name=iris, salary=170000, age=55] 
```

当排序需求改变时，您只需传入另一个新的匿名`Comparator`类:

```java
 //sort by age
	Collections.sort(listDevs, new Comparator<Developer>() {
		@Override
		public int compare(Developer o1, Developer o2) {
			return o1.getAge() - o2.getAge();
		}
	});

	//sort by name	
	Collections.sort(listDevs, new Comparator<Developer>() {
		@Override
		public int compare(Developer o1, Developer o2) {
			return o1.getName().compareTo(o2.getName());
		}
	});

	//sort by salary
	Collections.sort(listDevs, new Comparator<Developer>() {
		@Override
		public int compare(Developer o1, Developer o2) {
			return o1.getSalary().compareTo(o2.getSalary());
		}
	}); 
```

它可以工作，但是，你是否认为仅仅因为你想改变一行代码就创建一个类有点奇怪？

## 2.用 Lambda 排序

在 Java 8 中，`List`接口直接支持`sort`方法，不再需要使用`Collections.sort`。

```java
 //List.sort() since Java 8
	listDevs.sort(new Comparator<Developer>() {
		@Override
		public int compare(Developer o1, Developer o2) {
			return o2.getAge() - o1.getAge();
		}
	}); 
```

Lambda 表达式示例:

TestSorting.java

```java
 package com.mkyong.java8;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;

public class TestSorting {

	public static void main(String[] args) {

		List<Developer> listDevs = getDevelopers();

		System.out.println("Before Sort");
		for (Developer developer : listDevs) {
			System.out.println(developer);
		}

		System.out.println("After Sort");

		//lambda here!
		listDevs.sort((Developer o1, Developer o2)->o1.getAge()-o2.getAge());

		//java 8 only, lambda also, to print the List
		listDevs.forEach((developer)->System.out.println(developer));
	}

	private static List<Developer> getDevelopers() {

		List<Developer> result = new ArrayList<Developer>();

		result.add(new Developer("mkyong", new BigDecimal("70000"), 33));
		result.add(new Developer("alvin", new BigDecimal("80000"), 20));
		result.add(new Developer("jason", new BigDecimal("100000"), 10));
		result.add(new Developer("iris", new BigDecimal("170000"), 55));

		return result;

	}

} 
```

输出

```java
 Before Sort
Developer [name=mkyong, salary=70000, age=33]
Developer [name=alvin, salary=80000, age=20]
Developer [name=jason, salary=100000, age=10]
Developer [name=iris, salary=170000, age=55]

After Sort
Developer [name=jason, salary=100000, age=10]
Developer [name=alvin, salary=80000, age=20]
Developer [name=mkyong, salary=70000, age=33]
Developer [name=iris, salary=170000, age=55] 
```

## 3.更多 Lambda 示例

3.1 按年龄排序

```java
 //sort by age
	Collections.sort(listDevs, new Comparator<Developer>() {
		@Override
		public int compare(Developer o1, Developer o2) {
			return o1.getAge() - o2.getAge();
		}
	});

	//lambda
	listDevs.sort((Developer o1, Developer o2)->o1.getAge()-o2.getAge());

	//lambda, valid, parameter type is optional
	listDevs.sort((o1, o2)->o1.getAge()-o2.getAge()); 
```

3.2 按名称排序

```java
 //sort by name
	Collections.sort(listDevs, new Comparator<Developer>() {
		@Override
		public int compare(Developer o1, Developer o2) {
			return o1.getName().compareTo(o2.getName());
		}
	});

	//lambda
	listDevs.sort((Developer o1, Developer o2)->o1.getName().compareTo(o2.getName()));		

	//lambda
	listDevs.sort((o1, o2)->o1.getName().compareTo(o2.getName())); 
```

3.3 按薪资排序

```java
 //sort by salary
	Collections.sort(listDevs, new Comparator<Developer>() {
		@Override
		public int compare(Developer o1, Developer o2) {
			return o1.getSalary().compareTo(o2.getSalary());
		}
	});				

	//lambda
	listDevs.sort((Developer o1, Developer o2)->o1.getSalary().compareTo(o2.getSalary()));

	//lambda
	listDevs.sort((o1, o2)->o1.getSalary().compareTo(o2.getSalary())); 
```

3.4 反向排序。

3.4.1 Lambda 表达式使用工资对列表进行排序。

```java
 Comparator<Developer> salaryComparator = (o1, o2)->o1.getSalary().compareTo(o2.getSalary());
	listDevs.sort(salaryComparator); 
```

输出

```java
 Developer [name=mkyong, salary=70000, age=33]
Developer [name=alvin, salary=80000, age=20]
Developer [name=jason, salary=100000, age=10]
Developer [name=iris, salary=170000, age=55] 
```

3.4.2 Lambda 表达式使用工资对列表进行排序，顺序相反。

```java
 Comparator<Developer> salaryComparator = (o1, o2)->o1.getSalary().compareTo(o2.getSalary());
	listDevs.sort(salaryComparator.reversed()); 
```

输出

```java
 Developer [name=iris, salary=170000, age=55]
Developer [name=jason, salary=100000, age=10]
Developer [name=alvin, salary=80000, age=20]
Developer [name=mkyong, salary=70000, age=33] 
```

## 参考

1.  [开始使用 Java Lambda 表达式](http://web.archive.org/web/20210818031914/http://www.developer.com/java/start-using-java-lambda-expressions.html)
2.  [甲骨文:λ表达式](http://web.archive.org/web/20210818031914/https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)
3.  [甲骨文:比较器](http://web.archive.org/web/20210818031914/https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)

Tags : [comparator](http://web.archive.org/web/20210818031914/https://mkyong.com/tag/comparator/) [java8](http://web.archive.org/web/20210818031914/https://mkyong.com/tag/java8/) [lambda](http://web.archive.org/web/20210818031914/https://mkyong.com/tag/lambda/) [sorting](http://web.archive.org/web/20210818031914/https://mkyong.com/tag/sorting/)<input type="hidden" id="mkyong-current-postId" value="13840">