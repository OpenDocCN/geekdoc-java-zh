# 如何在 Eclipse 中设置 Wicket 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/how-do-setup-wicket-examples-in-eclipse/>

[Wicket examples](http://web.archive.org/web/20201109010243/http://wicketstuff.org/wicket14/) 是通过示例学习 Apache Wicket 的好地方，也是新的或有经验的 Wicket 开发人员必须参考的网站。在这个 wicket 示例站点中，它几乎包含了通用 Wicket 组件的所有用法。

在本指南中，我们将向您展示如何在您的本地开发环境(Eclipse IDE)中设置上面的 Apache Wicket 示例站点。

使用的工具:

1.  Apache Wicket 1.4.17
2.  Eclipse 3.6
3.  maven3

## 1.下载源代码

从 http://wicket.apache.org/[下载 Apache Wicket 1.4.17。Wicket 示例代码打包在“src”文件夹中。](http://web.archive.org/web/20201109010243/http://wicket.apache.org/)

## 2.审查目录

提取下载的 Wicket zip 文件，检查目录结构。文件夹“ **wicket-examples** ”，它在“$WICKET_PATH/src”文件夹里面就是你所需要的。



![wicket example folder structure](img/4c092b4886f64f22821f646c3dab8f94.png "wicket-example-folder")

## 3.Maven 构建

导航到“ **wicket-examples** ”文件夹，用 Maven 编译和构建，并使其支持 Eclipse WTP 特性。

```java
$WICKET_EXAMPLE_FILE_PATH> mvn eclipse:eclipse -Dwtpversion=2.0

```

P.S Maven 将自动配置项目并下载项目依赖项。

## 4.Eclipse 项目+ WTP

将项目导入 Eclipse IDE(您应该知道如何进行:)。

然而，在 Wicket 1.4.17 中，Maven " `-Dwtpversion=2.0`"选项似乎在我的 Eclipse 3.6 中不起作用，因为我注意到 Eclipse facets 文件和部署依赖库没有正确配置。如果您有同样的问题，请执行以下步骤:

1.右键单击项目–>属性–>“**项目方面**”。选择“**动态 Web 模块**”和“ **Java** ”。



![wicket example eclipse facets](img/9ff663d12ff9133814b9d9ba801faa2d.png "wicket-example-eclipse-facets")

2.相同的窗口–>选择"**部署程序集**，确保库和根部署路径"/"配置正确。缺了就补上。



![wicket example eclipse depoyment dependency](img/07ed24439f299dfbb18ba04d04649d68.png "wicket-example-eclipse-deployment")

## 5.部署+测试

在 Eclipse IDE 中，创建一个 Tomcat 实例，将您配置的“wicket-example”项目分配给新的 Tomcat 实例并启动它。

访问此 URL:**http://localhost:8080/wicket-examples/**



![wicket example cloned site](img/50c5b90ac3ef41870f82a23659f99483.png "wicket-example-cloned")

完成了。整个 wicket examples 站点被克隆到您的本地开发环境中。

Tags : [eclipse](http://web.archive.org/web/20201109010243/https://mkyong.com/tag/eclipse/) [wicket](http://web.archive.org/web/20201109010243/https://mkyong.com/tag/wicket/)<input type="hidden" id="mkyong-current-postId" value="846">

### 相关文章

*   【Eclipse IDE 的 Java 反编译器插件
*   [内容辅助(Ctrl + Space)不起作用- Ecl](/web/20201109010243/https://www.mkyong.com/java/content-assist-ctrl-space-is-not-working-eclipse/)
*   [在 Eclipse 中将 Java 项目转换为 Web 项目](/web/20201109010243/https://www.mkyong.com/java/how-to-convert-java-project-to-web-project-in-eclipse/)
*   [如何在 Eclipse IDE 中查看 Java 类层次结构？](/web/20201109010243/https://www.mkyong.com/java/how-to-view-java-class-hierarchy-in-eclipse-ide/)
*   [如何更改 Eclipse splash 欢迎屏幕图像](/web/20201109010243/https://www.mkyong.com/java/how-to-change-eclipse-splash-welcome-screen-image/)

*   [Java -如何生成 serialVersionUID](/web/20201109010243/https://www.mkyong.com/java/how-to-generate-serialversionuid/)
*   [如何在 Eclipse 控制台显示汉字](/web/20201109010243/https://www.mkyong.com/java/how-to-display-chinese-character-in-eclipse-console/)
*   [访问限制:BASE64Encoder 类型不是](/web/20201109010243/https://www.mkyong.com/java/access-restriction-the-type-base64encoder-is-not-accessible-due-to-restriction/)
*   [如何在 Eclipse IDE 中调试 Ant Ivy 项目](/web/20201109010243/https://www.mkyong.com/ant/how-to-debug-ant-ivy-project-in-eclipse-ide/)
*   [如何将 SWT 库导入 Eclipse Workspace？](/web/20201109010243/https://www.mkyong.com/swt/how-to-import-swt-library-into-eclipse-workspace/)