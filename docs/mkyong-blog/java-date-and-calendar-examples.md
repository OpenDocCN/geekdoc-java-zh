# Java 日期和日历示例

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/java-date-and-calendar-examples/>

![Calendar](img/ad2f41bfb418c6236a0906c12b1f2396.png)

本教程向您展示如何使用`java.util.Date`和`java.util.Calendar`。

## 1.Java 日期示例

几个使用`Date`API 的例子。

**示例 1.1**–将日期转换为字符串。

```java
 SimpleDateFormat sdf = new SimpleDateFormat("dd/M/yyyy");
	String date = sdf.format(new Date()); 
	System.out.println(date); //15/10/2013 
```

**例 1.2**–将字符串转换为日期。

```java
 SimpleDateFormat sdf = new SimpleDateFormat("dd-M-yyyy hh:mm:ss");
	String dateInString = "31-08-1982 10:20:56";
	Date date = sdf.parse(dateInString);
	System.out.println(date); //Tue Aug 31 10:20:56 SGT 1982 
```

关于详细的日期和时间模式，请参考 this-[simple date format JavaDoc](http://web.archive.org/web/20220807015729/https://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html)。

**示例 1.3**–获取当前日期时间

```java
 SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
	Date date = new Date();
	System.out.println(dateFormat.format(date)); //2013/10/15 16:16:39 
```

**示例 1.4**–将日历转换为日期

```java
 Calendar calendar = Calendar.getInstance();
        Date date =  calendar.getTime(); 
```

## 2.Java 日历示例

几个使用`Calendar`API 的例子。

**示例 2.1**–获取当前日期时间

```java
 SimpleDateFormat sdf = new SimpleDateFormat("yyyy MMM dd HH:mm:ss");	
	Calendar calendar = new GregorianCalendar(2013,0,31);
	System.out.println(sdf.format(calendar.getTime())); 
```

输出

```java
 2013 Jan 31 00:00:00 
```

**示例 2.2**–简单日历示例

```java
 SimpleDateFormat sdf = new SimpleDateFormat("yyyy MMM dd HH:mm:ss");	
	Calendar calendar = new GregorianCalendar(2013,1,28,13,24,56);

	int year       = calendar.get(Calendar.YEAR);
	int month      = calendar.get(Calendar.MONTH); // Jan = 0, dec = 11
	int dayOfMonth = calendar.get(Calendar.DAY_OF_MONTH); 
	int dayOfWeek  = calendar.get(Calendar.DAY_OF_WEEK);
	int weekOfYear = calendar.get(Calendar.WEEK_OF_YEAR);
	int weekOfMonth= calendar.get(Calendar.WEEK_OF_MONTH);

	int hour       = calendar.get(Calendar.HOUR);        // 12 hour clock
	int hourOfDay  = calendar.get(Calendar.HOUR_OF_DAY); // 24 hour clock
	int minute     = calendar.get(Calendar.MINUTE);
	int second     = calendar.get(Calendar.SECOND);
	int millisecond= calendar.get(Calendar.MILLISECOND);

	System.out.println(sdf.format(calendar.getTime()));

	System.out.println("year \t\t: " + year);
	System.out.println("month \t\t: " + month);
	System.out.println("dayOfMonth \t: " + dayOfMonth);
	System.out.println("dayOfWeek \t: " + dayOfWeek);
	System.out.println("weekOfYear \t: " + weekOfYear);
	System.out.println("weekOfMonth \t: " + weekOfMonth);

	System.out.println("hour \t\t: " + hour);
	System.out.println("hourOfDay \t: " + hourOfDay);
	System.out.println("minute \t\t: " + minute);
	System.out.println("second \t\t: " + second);
	System.out.println("millisecond \t: " + millisecond); 
```

输出

```java
 2013 Feb 28 13:24:56
year 		: 2013
month 		: 1
dayOfMonth 	: 28
dayOfWeek 	: 5
weekOfYear 	: 9
weekOfMonth     : 5
hour 		: 1
hourOfDay 	: 13
minute 		: 24
second 		: 56
millisecond     : 0 
```

**例 2.3**–手动设置日期。

```java
 SimpleDateFormat sdf = new SimpleDateFormat("yyyy MMM dd HH:mm:ss");	

	Calendar calendar = new GregorianCalendar(2013,1,28,13,24,56);	
	System.out.println("#1\. " + sdf.format(calendar.getTime()));

	//update a date
	calendar.set(Calendar.YEAR, 2014);
	calendar.set(Calendar.MONTH, 11);
	calendar.set(Calendar.MINUTE, 33);

	System.out.println("#2\. " + sdf.format(calendar.getTime())); 
```

输出

```java
 #1\. 2013 Feb 28 13:24:56
#2\. 2014 Dec 28 13:33:56 
```

**示例 2.4**–增加或减少日期。

```java
 SimpleDateFormat sdf = new SimpleDateFormat("yyyy MMM dd");	

	Calendar calendar = new GregorianCalendar(2013,10,28);	
	System.out.println("Date : " + sdf.format(calendar.getTime()));

	//add one month
	calendar.add(Calendar.MONTH, 1);
	System.out.println("Date : " + sdf.format(calendar.getTime()));

	//subtract 10 days
	calendar.add(Calendar.DAY_OF_MONTH, -10);
	System.out.println("Date : " + sdf.format(calendar.getTime())); 
```

输出

```java
 Date : 2013 Nov 28
Date : 2013 Dec 28
Date : 2013 Dec 18 
```

**示例 2.5**–将日期转换为日历。

```java
 SimpleDateFormat sdf = new SimpleDateFormat("dd-M-yyyy hh:mm:ss");
	String dateInString = "22-01-2015 10:20:56";
	Date date = sdf.parse(dateInString);

        Calendar calendar = Calendar.getInstance();
	calendar.setTime(date); 
```

## 参考

1.  [日历 JavaDoc](http://web.archive.org/web/20220807015729/https://docs.oracle.com/javase/7/docs/api/java/util/Calendar.html)
2.  [日期 JavaDoc](http://web.archive.org/web/20220807015729/https://docs.oracle.com/javase/6/docs/api/java/util/Date.html)
3.  [Java–将字符串转换为日期](http://web.archive.org/web/20220807015729/http://www.mkyong.com/java/how-to-convert-string-to-date-java/)
4.  [如何在 Java 中比较日期](http://web.archive.org/web/20220807015729/http://www.mkyong.com/java/how-to-compare-dates-in-java/)

<input type="hidden" id="mkyong-current-postId" value="13112">