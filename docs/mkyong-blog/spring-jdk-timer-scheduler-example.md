# Spring + JDK 定时器调度器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-jdk-timer-scheduler-example/>

**Note**
Learn the [JDK Timer scheduler example without Spring](http://web.archive.org/web/20190214222721/http://www.mkyong.com/java/jdk-timer-scheduler-example/) and compare the different with this example.

在这个例子中，您将使用 Spring 的 Scheduler API 来调度一个任务。

## 1.调度程序任务

创建调度程序任务…

```java
 package com.mkyong.common;

public class RunMeTask
{
	public void printMe() {
		System.out.println("Run Me ~");
	}
} 
```

```java
 <bean id="runMeTask" class="com.mkyong.common.RunMeTask" /> 
```

Spring 附带了一个**MethodInvokingTimerTaskFactoryBean**作为 JDK TimerTask 的替代。您可以在这里定义要调用的目标调度程序对象和方法。

```java
 <bean id="schedulerTask" 
  class="org.springframework.scheduling.timer.MethodInvokingTimerTaskFactoryBean">
	<property name="targetObject" ref="runMeTask" />
	<property name="targetMethod" value="printMe" />
</bean> 
```

Spring 附带了一个 **ScheduledTimerTask** 来代替 JDK 计时器。您可以在这里传递您的调度程序名称、延迟和周期。

```java
 <bean id="timerTask"
	class="org.springframework.scheduling.timer.ScheduledTimerTask">
	<property name="timerTask" ref="schedulerTask" />
	<property name="delay" value="1000" />
	<property name="period" value="60000" />
</bean> 
```

 ## 2.定时器工厂 Bean

最后，您可以配置一个 TimerFactoryBean bean 来启动您的调度程序任务。

```java
 <bean class="org.springframework.scheduling.timer.TimerFactoryBean">
	<property name="scheduledTimerTasks">
		<list>
			<ref local="timerTask" />
		</list>
	</property>
</bean> 
```

*文件:Spring-Scheduler.xml*

```java
 <beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

<bean id="schedulerTask" 
  class="org.springframework.scheduling.timer.MethodInvokingTimerTaskFactoryBean">
	<property name="targetObject" ref="runMeTask" />
	<property name="targetMethod" value="printMe" />
</bean>

<bean id="runMeTask" class="com.mkyong.common.RunMeTask" />

<bean id="timerTask"
	class="org.springframework.scheduling.timer.ScheduledTimerTask">
	<property name="timerTask" ref="schedulerTask" />
	<property name="delay" value="1000" />
	<property name="period" value="60000" />
</bean>

<bean class="org.springframework.scheduling.timer.TimerFactoryBean">
	<property name="scheduledTimerTasks">
		<list>
			<ref local="timerTask" />
		</list>
	</property>
</bean>

</beans> 
```

运行它

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext context = 
		  new ClassPathXmlApplicationContext("Spring-Scheduler.xml");
    }
} 
```

没有代码需要调用调度程序任务， **TimerFactoryBean** 将在启动时运行您的调度任务。因此，Spring scheduler 将每 60 秒运行一次 printMe()方法，第一次执行时有 1 秒的延迟。

 ## 下载源代码

Download it – [Spring-Scheduler-JDK-TimerExample.zip](http://web.archive.org/web/20190214222721/http://www.mkyong.com/wp-content/uploads/2010/04/Spring-Scheduler-JDK-TimerExample.zip)[integration](http://web.archive.org/web/20190214222721/http://www.mkyong.com/tag/integration/) [scheduler](http://web.archive.org/web/20190214222721/http://www.mkyong.com/tag/scheduler/) [spring](http://web.archive.org/web/20190214222721/http://www.mkyong.com/tag/spring/) [timer](http://web.archive.org/web/20190214222721/http://www.mkyong.com/tag/timer/)







