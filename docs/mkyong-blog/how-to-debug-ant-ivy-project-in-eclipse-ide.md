# 如何在 Eclipse IDE 中调试 Ant Ivy 项目

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/ant/how-to-debug-ant-ivy-project-in-eclipse-ide/>

在本教程中，我们将向您展示如何在 Eclipse IDE 中调试 Ant-Ivy web 项目。

使用的技术:

1.  Eclipse 4.2
2.  Eclipse Tomcat 插件
3.  Ant 1.9.4
4.  阿帕奇 IvyDE
5.  弹簧 4.1.3 .释放

**Note**
Previous [Ant Spring MVC web project](http://web.archive.org/web/20221225035500/http://www.mkyong.com/ant/ant-spring-mvc-and-war-file-example/) will be reused.

## 1.安装 Apache IvyDE

安装 Apache IvyDE，它将 Ivy 集成到 Eclipse IDE 中。重启 Eclipse 以完成安装。

![ivy-site](img/67d87d086bb9a3e142282f1c8b9acd2d.png)

## 2.向项目添加 Ivy 支持

将之前的 [Ant Spring MVC 项目](http://web.archive.org/web/20221225035500/http://www.mkyong.com/ant/ant-spring-mvc-and-war-file-example/)作为 Java 项目导入到 Eclipse IDE 中(我们稍后会转换)。

**Note**
There are some error icons on Java source, ignore it, the error will be gone after integrated with Ivy dependency management.

2.1 右击项目文件夹，选择"配置->添加 Ivy 依赖管理"

![Debug-ant-Ivy-in-eclipse-1](img/b60d60647b43ee1046298a1bba3a8546.png)

2.2 右键单击`ivy.xml`或`build.xml`，选择“添加 Ivy 库…”会出现一个 Ivy 对话框，单击完成按钮接受默认设置。

![Debug-ant-Ivy-in-eclipse-2](img/e029219bfef0ad65c20070721689ce8b.png)

2.3 再次右键点击项目文件夹，选择“Ivy”，现在有很多选项可用。

![Debug-ant-Ivy-in-eclipse-3](img/dc6fbc66de601f5cdc4b76427c33894e.png)

## 3.转换为 Eclipse Web 项目

要在 Eclipse 中将导入的 Java 项目转换为 web 项目(WTP ):

3.1 右击项目，选择"属性->项目刻面" :

勾选`Java`，选择 1.7，勾选`Dynamic Web Module`，选择 2.5，点击“进一步配置可用”，点击“下一步”接受 Java 应用的默认设置。

![Debug-ant-Ivy-in-eclipse-4](img/c8063c8d277a581f9e206700823aff8d.png)

将“内容目录”更新到`war`文件夹(包含 WEB-INF 文件夹的文件夹)，取消选中生成 web.xml 选项。

![Debug-ant-Ivy-in-eclipse-5](img/e7c87af7000dffa3300a4a0894fc44ef.png)

3.2 在项目属性上，选择“部署程序集”，点击“添加…”按钮。选择“Java 构建路径条目-> Ivy”。

![Debug-ant-Ivy-in-eclipse-6](img/dbb5155580f2bd41bd1dddf9108fde04.png)

确保文件夹是正确的，并且添加了 Ivy 库。

![Debug-ant-Ivy-in-eclipse-7](img/794a8ce3b068f518d35472292550945d.png)

## 4.调试它

以上项目与 Eclipse 集成，只需像往常一样调试项目。在服务器视图中，只需创建一个 Tomcat 服务器并附加 web 项目。

![Debug-ant-Ivy-in-eclipse-8](img/2fbf5a047b94c493ed9a61ec4a276138.png)

完成了。

## 参考

1.  [IvyDE 官网](http://web.archive.org/web/20221225035500/https://ant.apache.org/ivy/ivyde/)
2.  [蚂蚁春天 MVC web 项目](http://web.archive.org/web/20221225035500/http://www.mkyong.com/ant/ant-spring-mvc-and-war-file-example/)

<input type="hidden" id="mkyong-current-postId" value="13513">