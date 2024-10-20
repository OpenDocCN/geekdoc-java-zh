# 如何用正则表达式验证 12 小时格式的时间

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/regular-expressions/how-to-validate-time-in-12-hours-format-with-regular-expression/>

12 小时格式正则表达式模式中的时间

```java
 (1[012]|[1-9]):[0-5][0-9](\\s)?(?i)(am|pm) 
```

描述

```java
 (				#start of group #1
 1[012]				#  start with 10, 11, 12
 |				#  or
 [1-9]				#  start with 1,2,...9
)				#end of group #1
 :				#    follow by a semi colon (:)
  [0-5][0-9]			#      follw by 0..5 and 0..9, which means 00 to 59
            (\\s)?		#        follow by a white space (optional)
                  (?i)		#          next checking is case insensitive
                      (am|pm)	#            follow by am or pm 
```

12 小时制格式是从 0-12 开始，然后是分号(:)，接着是 00-59，最后以 am 或 pm 结束。

## Java 正则表达式示例

```java
 package com.mkyong.regex;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Time12HoursValidator{

	  private Pattern pattern;
	  private Matcher matcher;

	  private static final String TIME12HOURS_PATTERN = 
                                "(1[012]|[1-9]):[0-5][0-9](\\s)?(?i)(am|pm)";

	  public Time12HoursValidator(){
		  pattern = Pattern.compile(TIME12HOURS_PATTERN);
	  }

	  /**
	   * Validate time in 12 hours format with regular expression
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

1.“凌晨 1 点”，“凌晨 1 点”，“凌晨 1 点”，
2。“下午 1 点”，“下午 1 点”，“下午 1 点”，
3。"下午 12 点 50 分"

 ## 时间格式不匹配:

1.“0:00am”-小时超出范围[1-12]
2。“上午 10:00”——只允许一个空格
3。“1:00”——必须以 am 或 pm 结束
4。“23:00am”-不允许 24 小时格式
5。“1:61pm”-分钟超出范围[0-59]
6。“13:00pm”-小时超出范围[1-12]
7。“001:50pm”——无效的小时格式
8。“上午 10:99”-分钟超出范围[0-59]
9。“01:00pm”——不允许 24 小时格式
10。“1:00 BM”-必须以 am 或 pm 结束

## 单元测试–时间 12 小时验证测试

```java
 package com.mkyong.regex;

import org.testng.Assert;
import org.testng.annotations.*;

/**
 * Time 12 hours format validator Testing
 * @author mkyong
 *
 */
public class Time12HoursValidatorTest {

	private Time12HoursValidator time12HoursValidator;

	@BeforeClass
        public void initData(){
		time12HoursValidator = new Time12HoursValidator();
        }

	@DataProvider
	public Object[][] ValidTime12HoursProvider() {
		return new Object[][]{
		     new Object[] {"1:00am"}, new Object[] {"1:00 am"}, 
                     new Object[] {"1:00 AM"}, new Object[] {"1:00pm"}, 
                     new Object[] {"1:00 pm"},new Object[] {"1:00 PM"},
		     new Object[] {"12:50 pm"}
		};
	}

	@DataProvider
	public Object[][] InvalidTime12HoursProvider() {
		return new Object[][]{
			 new Object[] {"0:00 am"},new Object[] {"10:00  am"},
			 new Object[] {"1:00"},new Object[] {"23:00 am"},
			 new Object[] {"1:61 pm"},new Object[] {"13:00 pm"},
			 new Object[] {"001:50 pm"},new Object[] {"10:99 am"},
			 new Object[] {"01:00 pm"}, new Object[] {"1:00 bm"}
		};
	}

	@Test(dataProvider = "ValidTime12HoursProvider")
	public void ValidTime12HoursTest(String time) {
	   boolean valid = time12HoursValidator.validate(time);
	   System.out.println("Time12Hours is valid : " + time + " , " + valid);
	   Assert.assertEquals(true, valid);
	}

	@Test(dataProvider = "InvalidTime12HoursProvider", 
                 dependsOnMethods="ValidTime12HoursTest")
	public void InValidTime12HoursTest(String time) {
	   boolean valid = time12HoursValidator.validate(time);
	   System.out.println("Time12Hours is valid : " + time + " , " + valid);
	   Assert.assertEquals(false, valid); 
	}	
} 
```

## 单元测试–结果

```java
 Time12Hours is valid : 1:00am , true
Time12Hours is valid : 1:00 am , true
Time12Hours is valid : 1:00 AM , true
Time12Hours is valid : 1:00pm , true
Time12Hours is valid : 1:00 pm , true
Time12Hours is valid : 1:00 PM , true
Time12Hours is valid : 12:50 pm , true
Time12Hours is valid : 0:00 am , false
Time12Hours is valid : 10:00  am , false
Time12Hours is valid : 1:00 , false
Time12Hours is valid : 23:00 am , false
Time12Hours is valid : 1:61 pm , false
Time12Hours is valid : 13:00 pm , false
Time12Hours is valid : 001:50 pm , false
Time12Hours is valid : 10:99 am , false
Time12Hours is valid : 01:00 pm , false
Time12Hours is valid : 1:00 bm , false
PASSED: ValidTime12HoursTest("1:00am")
PASSED: ValidTime12HoursTest("1:00 am")
PASSED: ValidTime12HoursTest("1:00 AM")
PASSED: ValidTime12HoursTest("1:00pm")
PASSED: ValidTime12HoursTest("1:00 pm")
PASSED: ValidTime12HoursTest("1:00 PM")
PASSED: ValidTime12HoursTest("12:50 pm")
PASSED: InValidTime12HoursTest("0:00 am")
PASSED: InValidTime12HoursTest("10:00  am")
PASSED: InValidTime12HoursTest("1:00")
PASSED: InValidTime12HoursTest("23:00 am")
PASSED: InValidTime12HoursTest("1:61 pm")
PASSED: InValidTime12HoursTest("13:00 pm")
PASSED: InValidTime12HoursTest("001:50 pm")
PASSED: InValidTime12HoursTest("10:99 am")
PASSED: InValidTime12HoursTest("01:00 pm")
PASSED: InValidTime12HoursTest("1:00 bm")

===============================================
    com.mkyong.regex.Time12HoursValidatorTest
    Tests run: 17, Failures: 0, Skips: 0
===============================================

===============================================
mkyong
Total tests run: 17, Failures: 0, Skips: 0
=============================================== 
```

## 参考

[http://en.wikipedia.org/wiki/12-hour_clock](http://web.archive.org/web/20190214230831/http://en.wikipedia.org/wiki/12-hour_clock)

想了解更多关于正则表达式的知识吗？强烈推荐这本最好最经典的书——《掌握正则表达式》

<center>

[http://web.archive.org/web/20190214230831if_/http://rcm.amazon.com/e/cm?t=progrlife-20&o=1&p=8&l=as1&asins=0596528124&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr](http://web.archive.org/web/20190214230831if_/http://rcm.amazon.com/e/cm?t=progrlife-20&o=1&p=8&l=as1&asins=0596528124&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr)

</center>

[regex](http://web.archive.org/web/20190214230831/http://www.mkyong.com/tag/regex/) [timestamp](http://web.archive.org/web/20190214230831/http://www.mkyong.com/tag/timestamp/)







