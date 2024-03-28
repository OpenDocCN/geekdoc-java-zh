# 课程：实现您自己的权限

> 原文：[`docs.oracle.com/javase/tutorial/security/userperm/index.html`](https://docs.oracle.com/javase/tutorial/security/userperm/index.html)

本课程演示了如何编写一个定义自己特殊权限的类。本课程的基本组件包括：

1.  一个名为**ExampleGame**的示例游戏。

1.  一个名为**HighScore**的类，被`ExampleGame`用来存储用户最新的高分。

1.  一个名为**HighScorePermission**的类，用于保护对用户存储的高分值的访问。

1.  用户的安全**策略文件**，授予`ExampleGame`更新他/她的高分的权限。

基本场景如下：

1.  用户玩`ExampleGame`。

1.  如果用户达到新的高分，`ExampleGame`使用`HighScore`类来保存这个新值。

1.  `HighScore`类查看用户的安全策略，以检查`ExampleGame`是否有权限更新用户的高分值。

1.  如果`ExampleGame`有权限更新高分，则 HighScore 类更新该值。

我们描述每个基本组件的关键点，然后展示一个示例运行：

+   ExampleGame

+   高分类

+   高分权限类

+   一个示例策略文件

+   将所有内容整合在一起
