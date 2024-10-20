# 通过自动检测进行弹簧自动布线

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-autowiring-by-autodetect/>

在 Spring 中，“ **Autowiring by AutoDetect** ”表示如果默认构造函数(带任何数据类型的参数)，则选择“ [autowire by constructor](http://web.archive.org/web/20190308062331/http://www.mkyong.com/spring/spring-autowiring-by-constructor/) ”，否则使用“ [autowire by type](http://web.archive.org/web/20190308062331/http://www.mkyong.com/spring/spring-autowiring-by-type/) ”。

参见 Spring“通过自动检测进行自动布线”的示例。通过构造函数或类型将“功夫”bean 自动连接到“熊猫”中(基于熊猫 bean 的实现)。

```java
 <bean id="panda" class="com.mkyong.common.Panda" autowire="autodetect" />

	<bean id="kungfu" class="com.mkyong.common.KungFu" >
		<property name="name" value="Shao lin" />
	</bean> 
```

## 1.自动检测–由构造函数

如果提供了默认构造器，自动检测将按构造器选择焊线。

```java
 package com.mkyong.common;

public class Panda {
	private KungFu kungfu;

	public Panda(KungFu kungfu) {
		System.out.println("autowiring by constructor");
		this.kungfu = kungfu;
	}

	public KungFu getKungfu() {
		return kungfu;
	}

	public void setKungfu(KungFu kungfu) {
		System.out.println("autowiring by type");
		this.kungfu = kungfu;
	}

	//...
} 
```

*输出*

```java
 autowiring by constructor
Person [kungfu=Language [name=Shao lin]] 
```

 ## 2.自动检测–按类型

如果没有找到默认构造函数，自动检测将按类型选择焊线。

```java
 package com.mkyong.common;

public class Panda {
	private KungFu kungfu;

	public KungFu getKungfu() {
		return kungfu;
	}

	public void setKungfu(KungFu kungfu) {
		System.out.println("autowiring by type");
		this.kungfu = kungfu;
	}

	//...
} 
```

*输出*

```java
 autowiring by type
Person [kungfu=Language [name=Shao lin]] 
```

 ## 下载源代码

Download it – [Spring-AutoWiring-by-Auto-Detect-Example.zip](http://web.archive.org/web/20190308062331/http://www.mkyong.com/wp-content/uploads/2011/06/Spring-AutoWiring-by-Auto-Detect-Example.zip) (6 KB)

## 参考

1.  [弹簧按类型自动布线](http://web.archive.org/web/20190308062331/http://www.mkyong.com/spring/spring-autowiring-by-type/)
2.  [弹簧由构造者自动接线](http://web.archive.org/web/20190308062331/http://www.mkyong.com/spring/spring-autowiring-by-constructor/)

[spring](http://web.archive.org/web/20190308062331/http://www.mkyong.com/tag/spring/) [wiring](http://web.archive.org/web/20190308062331/http://www.mkyong.com/tag/wiring/)







