# 通过 AbstractExcelView 弹出 MVC 和 Excel 文件

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-export-data-to-excel-file-via-abstractexcelview/>

Spring MVC 自带 **AbstractExcelView** 类，通过 **Apache POI** 库将数据导出到 Excel 文件。在本教程中，将展示如何在 Spring MVC 应用程序中使用 **AbstractExcelView** 类将数据导出到 Excel 文件以供下载。

## 1.Apache 然后

获取 [Apache POI 库](http://web.archive.org/web/20190217092721/http://poi.apache.org/)来创建 excel 文件。

```java
 <!-- Excel library --> 
   <dependency>
	<groupId>org.apache.poi</groupId>
	<artifactId>poi</artifactId>
	<version>3.6</version>
   </dependency> 
```

 ## 2.控制器

一个控制器类，生成虚拟数据用于演示，并获取请求参数以确定返回哪个视图。如果请求参数等于“Excel”，则返回一个 EXCEL 视图( **AbstractExcelView** )。

*文件:RevenueReportController.java*

```java
 package com.mkyong.common.controller;

import java.util.HashMap;
import java.util.Map;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.web.bind.ServletRequestUtils;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.AbstractController;

public class RevenueReportController extends AbstractController{

	@Override
	protected ModelAndView handleRequestInternal(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		String output =
			ServletRequestUtils.getStringParameter(request, "output");

		//dummy data
		Map<String,String> revenueData = new HashMap<String,String>();
		revenueData.put("Jan-2010", "$100,000,000");
		revenueData.put("Feb-2010", "$110,000,000");
		revenueData.put("Mar-2010", "$130,000,000");
		revenueData.put("Apr-2010", "$140,000,000");
		revenueData.put("May-2010", "$200,000,000");

		if(output ==null || "".equals(output)){
			//return normal view
			return new ModelAndView("RevenueSummary","revenueData",revenueData);

		}else if("EXCEL".equals(output.toUpperCase())){
			//return excel view
			return new ModelAndView("ExcelRevenueSummary","revenueData",revenueData);

		}else{
			//return normal view
			return new ModelAndView("RevenueSummary","revenueData",revenueData);

		}	
	}
} 
```

 ## 3.AbstractExcelView

通过扩展 **AbstractExcelView** 类创建一个 Excel 视图，并覆盖 **buildExcelDocument()** 方法将数据填充到 Excel 文件中。 **AbstractExcelView** 正在使用 Apache POI API 创建 Excel 文件细节。

**Note**
For detail about how to use the Apache POI , please refer to [Apache POI documentation](http://web.archive.org/web/20190217092721/http://poi.apache.org/)

*文件:ExcelRevenueReportView.java*

```java
 package com.mkyong.common.view;

import java.util.Map;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.springframework.web.servlet.view.document.AbstractExcelView;

public class ExcelRevenueReportView extends AbstractExcelView{

	@Override
	protected void buildExcelDocument(Map model, HSSFWorkbook workbook,
		HttpServletRequest request, HttpServletResponse response)
		throws Exception {

		Map<String,String> revenueData = (Map<String,String>) model.get("revenueData");
		//create a wordsheet
		HSSFSheet sheet = workbook.createSheet("Revenue Report");

		HSSFRow header = sheet.createRow(0);
		header.createCell(0).setCellValue("Month");
		header.createCell(1).setCellValue("Revenue");

		int rowNum = 1;
		for (Map.Entry<String, String> entry : revenueData.entrySet()) {
			//create the row data
			HSSFRow row = sheet.createRow(rowNum++);
			row.createCell(0).setCellValue(entry.getKey());
			row.createCell(1).setCellValue(entry.getValue());
                }
	}
} 
```

**Note**
Alternatively, you can use the **AbstractJExcelView**, which is using the **JExcelAPI** to create the same Excel view, see this [AbstractJExcelView example](http://web.archive.org/web/20190217092721/http://www.mkyong.com/spring-mvc/spring-mvc-export-data-to-excel-file-via-abstractjexcelview/).

## 4.弹簧配置

为 Excel 视图创建一个 **XmlViewResolver** 。

```java
 <beans ...>

  <bean
  class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" />

	<bean class="com.mkyong.common.controller.RevenueReportController" />

	<bean class="org.springframework.web.servlet.view.XmlViewResolver">
		<property name="location">
			<value>/WEB-INF/spring-excel-views.xml</value>
		</property>
	</bean>

</beans> 
```

*文件:spring-excel-views.xml*

```java
 <bean id="ExcelRevenueSummary"
   	class="com.mkyong.common.view.ExcelRevenueReportView">
   </bean> 
```

## 5.演示

URL:**http://localhost:8080/springmvc/revenue report . htm？output=excel**

它生成一个 Excel 文件供用户下载。

![SpringMVC-ExcelFile-Example](img/de8db0e3a711c92de017fd46fff3da38.png "SpringMVC-ExcelFile-Example")

## 下载源代码

Download it – [SpringMVC-ExcelFile-AbstractExcelView-Example.zip](http://web.archive.org/web/20190217092721/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVC-ExcelFile-Example.zip) (9KB)

## 参考

1.  [阿帕奇兴趣点](http://web.archive.org/web/20190217092721/http://poi.apache.org/)
2.  [AbstractExcelView Javadoc](http://web.archive.org/web/20190217092721/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/servlet/view/document/AbstractExcelView.html)
3.  [Spring MVC 通过 AbstractJExcelView 将数据导出到 Excel 文件](http://web.archive.org/web/20190217092721/http://www.mkyong.com/spring-mvc/spring-mvc-export-data-to-excel-file-via-abstractjexcelview/)
4.  [Spring MVC XmlViewResolver 示例](http://web.archive.org/web/20190217092721/http://www.mkyong.com/spring-mvc/spring-mvc-xmlviewresolver-example/)

[excel](http://web.archive.org/web/20190217092721/http://www.mkyong.com/tag/excel/) [spring mvc](http://web.archive.org/web/20190217092721/http://www.mkyong.com/tag/spring-mvc/)







