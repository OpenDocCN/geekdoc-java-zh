# 平台环境的问题和练习

> 原文：[`docs.oracle.com/javase/tutorial/essential/environment/QandE/questions.html`](https://docs.oracle.com/javase/tutorial/essential/environment/QandE/questions.html)

## 问题

1\. 一个程序员安装了一个包含在.jar 文件中的新库。为了从他的代码中访问该库，他将 CLASSPATH 环境变量设置为指向新的.jar 文件。现在他发现当他尝试启动简单的应用程序时，会收到错误消息：

```java
java Hello
Exception in thread "main" java.lang.NoClassDefFoundError: Hello

```

在这种情况下，`Hello`类被编译成一个在当前目录中的.class 文件，但`java`命令似乎找不到它。出了什么问题？

## 练习

1\. 编写一个名为`PersistentEcho`的应用程序，具有以下功能：

+   如果使用命令行参数运行`PersistentEcho`，它会打印出这些参数。它还会将打印出的字符串保存到一个属性中，并将该属性保存到名为`PersistentEcho.txt`的文件中。

+   如果在没有命令行参数的情况下运行`PersistentEcho`，它会查找名为 PERSISTENTECHO 的环境变量。如果该变量存在，`PersistentEcho`会打印出其值，并以与命令行参数相同的方式保存该值。

+   如果在没有命令行参数的情况下运行`PersistentEcho`，并且未定义 PERSISTENTECHO 环境变量，则它会从`PersistentEcho.txt`中检索属性值并打印出来。

检查你的答案。
