# 如何用正则表达式验证 24 小时格式的时间

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/regular-expressions/how-to-validate-time-in-24-hours-format-with-regular-expression/>

24 小时格式正则表达式模式中的时间

```java
 ([01]?[0-9]|2[0-3]):[0-5][0-9] 
```

描述

```java
 (				#start of group #1
 [01]?[0-9]			#  start with 0-9,1-9,00-09,10-19
 |				#  or
 2[0-3]				#  start with 20-23
)				#end of group #1
 :				#    follow by a semi colon (:)
  [0-5][0-9]			#      follw by 0..5 and 0..9, which means 00 to 59 
```

24 小时制格式是从 0-23 或 00-23 开始，然后是分号(:)，接着是 00-59。

## Java 正则表达式示例

```java
 package com.mkyong.regex;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Time24HoursValidator{

	  private Pattern pattern;
	  private Matcher matcher;

	  private static final String TIME24HOURS_PATTERN = 
                 "([01]?[0-9]|2[0-3]):[0-5][0-9]";

	  public Time24HoursValidator(){
		  pattern = Pattern.compile(TIME24HOURS_PATTERN);
	  }

	  /**
	   * Validate time in 24 hours format with regular expression
	   * @param time time address for validation
	   * @return true valid time fromat, false invalid time format
	   */
	  public boolean validate(final String time){

		  matcher = pattern.matcher(time);
		  return matcher.matches();

	  }
} 
```

 ## 匹配的时间格式:

1.“01:00”，“02:00”，“13:00”，
2。“1:00”，“2:00”，“13:01”，
3。" 23:59 " 15:00 "
4 . "00:00″,"0:00"

 ## 时间格式不匹配:

1.“24:00”-小时超出范围[0-23]
2。“12:60”——分钟超出范围[00-59]
3。“0:0”——分钟格式无效，至少两位数
4。“13:1”——无效的分钟格式，至少两位数
5。“101:00”-小时超出范围[0-23]

## 单元测试–时间 24 小时验证测试

```java
 package com.mkyong.regex;

import org.testng.Assert;
import org.testng.annotations.*;

/**
 * Time 24 hours format validator Testing
 * @author mkyong
 *
 */
public class Time24HoursValidatorTest {

	private Time24HoursValidator time24HoursValidator;

	@BeforeClass
        public void initData(){
		time24HoursValidator = new Time24HoursValidator();
        }

	@DataProvider
	public Object[][] ValidTime24HoursProvider() {
		return new Object[][]{
			new Object[] {"01:00"}, new Object[] {"02:00"},
                        new Object[] {"13:00"}, new Object[] {"1:00"}, 
                        new Object[] {"2:00"},new Object[] {"13:01"},
		        new Object[] {"23:59"}, new Object[] {"15:00"},
		        new Object[] {"00:00"}, new Object[] {"0:00"}
		};
	}	

	@DataProvider
	public Object[][] InvalidTime24HoursProvider() {
		return new Object[][]{
			new Object[] {"24:00"},new Object[] {"12:60"},
			new Object[] {"0:0"},new Object[] {"13:1"},
			new Object[] {"101:00"}
		};
	}

	@Test(dataProvider = "ValidTime24HoursProvider")
	public void ValidTime24HoursTest(String time) {
	    boolean valid = time24HoursValidator.validate(time);
	    System.out.println("Time24Hours is valid : " + time + " , " + valid);
	    Assert.assertEquals(true, valid);
	}

	@Test(dataProvider = "InvalidTime24HoursProvider", 
                 dependsOnMethods="ValidTime24HoursTest")
	public void InValidTime24HoursTest(String time) {
	    boolean valid = time24HoursValidator.validate(time);
	    System.out.println("Time24Hours is valid : " + time + " , " + valid);
	    Assert.assertEquals(false, valid); 
	}	
} 
```

## 单元测试–结果

```java
 Time24Hours is valid : 01:00 , true
Time24Hours is valid : 02:00 , true
Time24Hours is valid : 13:00 , true
Time24Hours is valid : 1:00 , true
Time24Hours is valid : 2:00 , true
Time24Hours is valid : 13:01 , true
Time24Hours is valid : 23:59 , true
Time24Hours is valid : 15:00 , true
Time24Hours is valid : 00:00 , true
Time24Hours is valid : 0:00 , true
Time24Hours is valid : 24:00 , false
Time24Hours is valid : 12:60 , false
Time24Hours is valid : 0:0 , false
Time24Hours is valid : 13:1 , false
Time24Hours is valid : 101:00 , false
PASSED: ValidTime24HoursTest("01:00")
PASSED: ValidTime24HoursTest("02:00")
PASSED: ValidTime24HoursTest("13:00")
PASSED: ValidTime24HoursTest("1:00")
PASSED: ValidTime24HoursTest("2:00")
PASSED: ValidTime24HoursTest("13:01")
PASSED: ValidTime24HoursTest("23:59")
PASSED: ValidTime24HoursTest("15:00")
PASSED: ValidTime24HoursTest("00:00")
PASSED: ValidTime24HoursTest("0:00")
PASSED: InValidTime24HoursTest("24:00")
PASSED: InValidTime24HoursTest("12:60")
PASSED: InValidTime24HoursTest("0:0")
PASSED: InValidTime24HoursTest("13:1")
PASSED: InValidTime24HoursTest("101:00")

===============================================
    com.mkyong.regex.Time24HoursValidatorTest
    Tests run: 15, Failures: 0, Skips: 0
===============================================

===============================================
mkyong
Total tests run: 15, Failures: 0, Skips: 0
=============================================== 
```

## 参考

[http://en.wikipedia.org/wiki/24-hour_clock](http://web.archive.org/web/20190311020049/http://en.wikipedia.org/wiki/24-hour_clock)

想了解更多关于正则表达式的知识吗？强烈推荐这本经典的书——《掌握正则表达式》

<center>

[http://web.archive.org/web/20190311020049if_/http://rcm.amazon.com/e/cm?t=progrlife-20&o=1&p=8&l=as1&asins=0596528124&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr](http://web.archive.org/web/20190311020049if_/http://rcm.amazon.com/e/cm?t=progrlife-20&o=1&p=8&l=as1&asins=0596528124&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr)

</center>

[regex](http://web.archive.org/web/20190311020049/http://www.mkyong.com/tag/regex/) [timestamp](http://web.archive.org/web/20190311020049/http://www.mkyong.com/tag/timestamp/)







