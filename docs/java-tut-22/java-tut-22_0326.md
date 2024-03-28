# 问题和练习：自包含应用程序

> [`docs.oracle.com/javase/tutorial/deployment/selfContainedApps/QandE/questions.html`](https://docs.oracle.com/javase/tutorial/deployment/selfContainedApps/QandE/questions.html)

## 问题

1.  以下哪一项不是自包含应用程序的优势？

    1.  用户使用他们熟悉的安装程序安装应用程序。

    1.  该应用程序作为本机应用程序运行。

    1.  该应用程序在用户机器上需要更少的空间。

    1.  您可以控制应用程序使用的 JRE 版本。

    1.  该应用程序不需要浏览器来运行。

1.  真或假：MIME 类型必须始终用于设置文件关联。

1.  用于识别`<fx:deploy>` Ant 任务中自包含应用程序的入口点的元素是什么？

## 练习

1.  编写`<fx:deploy>` Ant 任务，为名为“我的示例应用程序”的简单应用程序生成 Windows MSI 捆绑包。应用程序的 JAR 文件位于`dist`目录中，主类为`samples.MyApp`，输出文件将写入当前目录。

1.  加强上一个练习中的代码，为所有 Windows 安装程序创建捆绑包，并为文本文件定义文件关联。

检查你的答案。
