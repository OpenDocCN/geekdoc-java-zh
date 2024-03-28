# 教程：介绍 MBeans

> 原文：[`docs.oracle.com/javase/tutorial/jmx/mbeans/index.html`](https://docs.oracle.com/javase/tutorial/jmx/mbeans/index.html)

本课程介绍了 JMX API 的基本概念，即受管理的 bean，或*MBeans*。

一个 MBean 是一个受管理的 Java 对象，类似于 JavaBeans 组件，遵循 JMX 规范中设定的设计模式。一个 MBean 可以代表一个设备、一个应用程序或任何需要被管理的资源。MBeans 公开一个由以下内容组成的管理接口：

+   一组可读或可写的属性，或两者兼有。

+   一组可调用的操作。

+   自我描述。

管理接口在 MBean 实例的整个生命周期中不会改变。当发生某些预定义事件时，MBeans 也可以发出通知。

JMX 规范定义了五种类型的 MBean：

+   标准 MBeans

+   动态 MBeans

+   开放 MBeans

+   模型 MBeans

+   MXBeans

本教程中的示例仅演示了最简单的 MBean 类型，即标准 MBeans 和 MXBeans。
