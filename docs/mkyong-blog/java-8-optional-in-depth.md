# Java 8 可选深度

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-optional-in-depth/>

Java 8 在 java.util 包中引入了一个新的可选类。它用于表示值存在或不存在。这个新构造的主要优点是不再有太多的空检查和`NullPointerException`。它避免了任何运行时`NullPointerExceptions`，支持我们开发干净整洁的 Java APIs 或应用程序。像集合和数组一样，它也是一个最多容纳一个值的容器。让我们用一些有用的例子来探索这个新的结构。

Java 8 可选的优点:

1.  不需要空检查。
2.  运行时不再出现 NullPointerException。
3.  我们可以开发干净整洁的 API。
4.  不再有锅炉板代码

## 1.可选基本示例

如果给定对象中存在一个值，方法返回一个非空的可选值。否则返回空可选。

`Optionaal.empty()`方法对于创建一个空的可选对象很有用。

OptionalBasicExample.java

```java
 package com.mkyong;

import java.util.Optional;

public class OptionalBasicExample {

    public static void main(String[] args) {

        Optional<String> gender = Optional.of("MALE");
        String answer1 = "Yes";
        String answer2 = null;

        System.out.println("Non-Empty Optional:" + gender);
        System.out.println("Non-Empty Optional: Gender value : " + gender.get());
        System.out.println("Empty Optional: " + Optional.empty());

        System.out.println("ofNullable on Non-Empty Optional: " + Optional.ofNullable(answer1));
        System.out.println("ofNullable on Empty Optional: " + Optional.ofNullable(answer2));

        // java.lang.NullPointerException
        System.out.println("ofNullable on Non-Empty Optional: " + Optional.of(answer2));

    }

} 
```

输出

```java
 Non-Empty Optional:Optional[MALE]
Non-Empty Optional: Gender value : MALE
Empty Optional: Optional.empty

ofNullable on Non-Empty Optional: Optional[Yes]
ofNullable on Empty Optional: Optional.empty

Exception in thread "main" java.lang.NullPointerException
	at java.util.Objects.requireNonNull(Objects.java:203)
	at java.util.Optional.<init>(Optional.java:96)
	at java.util.Optional.of(Optional.java:108)
	//... 
```

freestar.config.enabled_slots.push({ placementName: "mkyong_incontent_1", slotId: "mkyong_incontent_1" });

## 2.可选。地图和平面图

OptionalMapFlapMapExample.java

```java
 package com.mkyong;

import java.util.Optional;

public class OptionalMapFlapMapExample {

    public static void main(String[] args) {

        Optional<String> nonEmptyGender = Optional.of("male");
        Optional<String> emptyGender = Optional.empty();

        System.out.println("Non-Empty Optional:: " + nonEmptyGender.map(String::toUpperCase));
        System.out.println("Empty Optional    :: " + emptyGender.map(String::toUpperCase));

        Optional<Optional<String>> nonEmptyOtionalGender = Optional.of(Optional.of("male"));
        System.out.println("Optional value   :: " + nonEmptyOtionalGender);
        System.out.println("Optional.map     :: " + nonEmptyOtionalGender.map(gender -> gender.map(String::toUpperCase)));
        System.out.println("Optional.flatMap :: " + nonEmptyOtionalGender.flatMap(gender -> gender.map(String::toUpperCase)));

    }

} 
```

输出

```java
 Non-Empty Optional:: Optional[MALE]
Empty Optional    :: Optional.empty
Optional value   :: Optional[Optional[male]]
Optional.map     :: Optional[Optional[MALE]]
Optional.flatMap :: Optional[MALE] 
```

## 3.可选.过滤器

OptionalFilterExample.java

```java
 package com.mkyong;

import java.util.Optional;

public class OptionalFilterExample {

    public static void main(String[] args) {

        Optional<String> gender = Optional.of("MALE");
        Optional<String> emptyGender = Optional.empty();

        //Filter on Optional
        System.out.println(gender.filter(g -> g.equals("male"))); //Optional.empty
        System.out.println(gender.filter(g -> g.equalsIgnoreCase("MALE"))); //Optional[MALE]
        System.out.println(emptyGender.filter(g -> g.equalsIgnoreCase("MALE"))); //Optional.empty

    }

} 
```

输出

```java
 Optional.empty
Optional[MALE]
Optional.empty 
```

## 4.可选的 isPresent 和 ifPresent

如果给定的可选对象非空，则返回 true。否则返回 false。

如果给定的可选对象非空，则执行给定的动作。否则返回 false。

OptionalIfPresentExample.java

```java
 package com.mkyong;

import java.util.Optional;

public class OptionalIfPresentExample {

    public static void main(String[] args) {

        Optional<String> gender = Optional.of("MALE");
        Optional<String> emptyGender = Optional.empty();

        if (gender.isPresent()) {
            System.out.println("Value available.");
        } else {
            System.out.println("Value not available.");
        }

        gender.ifPresent(g -> System.out.println("In gender Option, value available."));

        //condition failed, no output print
        emptyGender.ifPresent(g -> System.out.println("In emptyGender Option, value available."));

    }

} 
```

输出

```java
 Value available.
In gender Option, value available. 
```

## 5.可选或其他方法

如果存在于可选容器中，它将返回值。否则返回给定的默认值。

OptionalOrElseExample.java

```java
 package com.mkyong;

import java.util.Optional;

public class OptionalOrElseExample {

    public static void main(String[] args) {

        Optional<String> gender = Optional.of("MALE");
        Optional<String> emptyGender = Optional.empty();

        System.out.println(gender.orElse("<N/A>")); //MALE
        System.out.println(emptyGender.orElse("<N/A>")); //<N/A>

        System.out.println(gender.orElseGet(() -> "<N/A>")); //MALE
        System.out.println(emptyGender.orElseGet(() -> "<N/A>")); //<N/A>

    }

} 
```

输出

```java
 MALE
<N/A>
MALE
<N/A> 
```

## 6.不带 Java 8 可选

正如大家对网上购物所熟悉的那样。让我们假设我们想要为一个著名的电子商务网站实现一个移动产品模块。

让我们在没有 Java 8 可选的情况下实现移动域模块。

ScreenResolution.java

```java
 package com.mkyong.without.optional;

public class ScreenResolution {

	private int width;
	private int height;

	public ScreenResolution(int width, int height){
		this.width = width;
		this.height = height;
	}

	public int getWidth() {
		return width;
	}

	public int getHeight() {
		return height;
	}

} 
```

DisplayFeatures.java

```java
 package com.mkyong.without.optional;

public class DisplayFeatures {

	private String size; // In inches
	private ScreenResolution resolution;

	public DisplayFeatures(String size, ScreenResolution resolution){
		this.size = size;
		this.resolution = resolution;
	}

	public String getSize() {
		return size;
	}
	public ScreenResolution getResolution() {
		return resolution;
	}

} 
```

Mobile.java

```java
 package com.mkyong.without.optional;

public class Mobile {

	private long id;
	private String brand;
	private String name;
	private DisplayFeatures displayFeatures;
	// Likewise we can see Memory Features, Camera Features etc.

	public Mobile(long id, String brand, String name, 
                            DisplayFeatures displayFeatures){
		this.id = id;
		this.brand = brand;
		this.name = name;
		this.displayFeatures = displayFeatures;
	}

	public long getId() {
		return id;
	}

	public String getBrand() {
		return brand;
	}

	public String getName() {
		return name;
	}

	public DisplayFeatures getDisplayFeatures() {
		return displayFeatures;
	}

} 
```

在这里，如果我们观察`getMobileScreenWidth()`方法，它有许多锅炉板代码和许多空检查。在 Java 8 之前，我们应该做所有这些无意义的事情来避免运行时 NullPointerExceptions。

MobileService.java

```java
 package com.mkyong.without.optional;

public class MobileService {

	public int getMobileScreenWidth(Mobile mobile){

		if(mobile != null){
			DisplayFeatures dfeatures = mobile.getDisplayFeatures();
			if(dfeatures != null){
				ScreenResolution resolution = dfeatures.getResolution();
				if(resolution != null){
					return resolution.getWidth();
				}
			}
		}
		return 0;

	}

} 
```

开发一个测试应用程序来测试这些领域对象。

MobileTesterWithoutOptional.java

```java
 package com.mkyong.without.optional;

public class MobileTesterWithoutOptional {

	public static void main(String[] args) {

		ScreenResolution resolution = new ScreenResolution(750,1334);
		DisplayFeatures dfeatures = new DisplayFeatures("4.7", resolution);
		Mobile mobile = new Mobile(2015001, "Apple", "iPhone 6s", dfeatures);

		MobileService mService = new MobileService();

		int mobileWidth = mService.getMobileScreenWidth(mobile);
		System.out.println("Apple iPhone 6s Screen Width = " + mobileWidth);

		ScreenResolution resolution2 = new ScreenResolution(0,0);
		DisplayFeatures dfeatures2 = new DisplayFeatures("0", resolution2);
		Mobile mobile2 = new Mobile(2015001, "Apple", "iPhone 6s", dfeatures2);		
		int mobileWidth2 = mService.getMobileScreenWidth(mobile2);
		System.out.println("Apple iPhone 16s Screen Width = " + mobileWidth2);

	}

} 
```

输出

```java
 Apple iPhone 6s Screen Width = 750
Apple iPhone 16s Screen Width = 0 
```

## 7.Java 8 可选

现在使用 Java 8 可选结构以干净整洁的方式开发相同的领域模型。

没有变化。请参考以上部分。

DisplayFeatures.java

```java
 package com.mkyong.with.optional;

import java.util.Optional;

public class DisplayFeatures {

	private String size; // In inches
	private Optional&lt;ScreenResolution&gt; resolution;

	public DisplayFeatures(String size, Optional<ScreenResolution> resolution){
		this.size = size;
		this.resolution = resolution;
	}

	public String getSize() {
		return size;
	}
	public Optional<ScreenResolution> getResolution() {
		return resolution;
	}

} 
```

Mobile.java

```java
 package com.mkyong.with.optional;

import java.util.Optional;

public class Mobile {

	private long id;
	private String brand;
	private String name;
	private Optional&lt;DisplayFeatures&gt; displayFeatures;
	// Like wise we can see MemoryFeatures, CameraFeatures etc.
	// For simplicity, using only one Features

	public Mobile(long id, String brand, String name, Optional&lt;DisplayFeatures&gt; displayFeatures){
		this.id = id;
		this.brand = brand;
		this.name = name;
		this.displayFeatures = displayFeatures;
	}

	public long getId() {
		return id;
	}

	public String getBrand() {
		return brand;
	}

	public String getName() {
		return name;
	}

	public Optional&lt;DisplayFeatures&gt; getDisplayFeatures() {
		return displayFeatures;
	}

} 
```

在这里，我们可以观察到如何清理我们的`getMobileScreenWidth()` API，而没有空检查和锅炉板代码。我们不担心运行时的 NullPointerExceptions。

MobileService.java

```java
 package com.mkyong.with.optional;

import java.util.Optional;

public class MobileService {

  public Integer getMobileScreenWidth(Optional<Mobile> mobile){
	return mobile.flatMap(Mobile::getDisplayFeatures)
		 .flatMap(DisplayFeatures::getResolution)
		 .map(ScreenResolution::getWidth)
		 .orElse(0);

  }

} 
```

现在开发一个测试组件

MobileTesterWithOptional.java

```java
 package com.mkyong.with.optional;

import java.util.Optional;

public class MobileTesterWithOptional {

  public static void main(String[] args) {
	ScreenResolution resolution = new ScreenResolution(750,1334);
	DisplayFeatures dfeatures = new DisplayFeatures("4.7", Optional.of(resolution));
	Mobile mobile = new Mobile(2015001, "Apple", "iPhone 6s", Optional.of(dfeatures));

	MobileService mService = new MobileService();

	int width = mService.getMobileScreenWidth(Optional.of(mobile));
	System.out.println("Apple iPhone 6s Screen Width = " + width);

	Mobile mobile2 = new Mobile(2015001, "Apple", "iPhone 6s", Optional.empty());		
	int width2 = mService.getMobileScreenWidth(Optional.of(mobile2));
	System.out.println("Apple iPhone 16s Screen Width = " + width2);
  }
} 
```

输出

```java
 Apple iPhone 6s Screen Width = 750
Apple iPhone 16s Screen Width = 0 
```

## 8.Java Optional 适合哪里？

如果我们观察上面的实时零售领域用例，我们应该知道 Java 可选结构在以下地方是有用的。

8.1 方法参数

```java
 public void setResolution(Optional<ScreenResolution> resolution) {
	this.resolution = resolution;
} 
```

8.2 方法返回类型

```java
 public Optional<ScreenResolution> getResolution() {
	return resolution;
} 
```

8.3 构造函数参数

```java
 public DisplayFeatures(String size, Optional<ScreenResolution> resolution){
	this.size = size;
	this.resolution = resolution;
} 
```

8.4 变量声明

```java
 private Optional<ScreenResolution> resolution; 
```

8.5 班级水平

```java
 public class B

public class A<T extends Optional<B>> { } 
```

## 下载源代码

Download – [Java8Optional-example.zip](http://web.archive.org/web/20210309063700/http://www.mkyong.com/wp-content/uploads/2017/02/Java8Optional.zip) (4 KB)

## 参考

1.  [OptionalJavaDoc](http://web.archive.org/web/20210309063700/https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)

Tags : [java8](http://web.archive.org/web/20210309063700/https://mkyong.com/tag/java8/) [optional](http://web.archive.org/web/20210309063700/https://mkyong.com/tag/optional/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="14460">