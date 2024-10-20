# 将 Android 源代码附加到 Eclipse IDE

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/attach-android-source-code-to-eclipse-ide/>

默认情况下， **Android SDK** 或 **Eclipse ADT** 插件不捆绑任何 Android 的源代码进行调试。在 Eclipse IDE 中，进入任何 Android 类都将提示没有附加源代码，请参见下面的屏幕:



![android source code not found in Eclipse](img/d1f7d112e9473629db5aac6149d7335e.png "android-source-code-to-eclipse")

## 解决办法

根据这个官方的 [Android 源代码文章](http://web.archive.org/web/20200920092334/http://source.android.com/source/initializing.html)，震惊的是我们需要使用`repo`下载然后构建完整的源代码来获得`android.jar`的源代码。

如果你认为上面的工作太多，或者，你可以安装名为“ **Android Source** 的 Eclipse 插件来获得“`android.jar`的源代码。阅读这篇文章“【Android 的附加 Eclipse 插件”。

在安装了" **Android source** "插件后，假设现有项目以及新创建的针对 Android 的项目将自动附加源代码 jar。然而，我现有的 Android 项目仍然没有附加到正确的源，我必须手动附加它。

找到" **Android Source** "插件文件夹，它应该在以下目录中:

```java
 ECLIPSE_PATH\plugins\com.android.ide.eclipse.source_MAY_BE_VARY 
```

文件夹:



![android source folders](img/acc74f27305810250dc9b8edd3a4eae2.png "android-source-code-to-eclipse-folder")

*   14–安卓 4.0.1
*   10–安卓 2.3.4
*   9–安卓 2.3
*   8–安卓 2.2
*   7–安卓 2.1
*   6–安卓 2.0.1
*   4–安卓 1.6
*   3–安卓 1.5

每个文件夹包含一个“ **sources.zip** ”，它针对特定的 Android 版本。比如你开发的是 **Android 2.3** ，那么从文件夹“ **10** ”中获取“ **sources.zip** ，手动将其附加到 Eclipse IDE 中。

再次踏入 Android 类，源代码显示。



![attach android source code to eclipse ide](img/4edbdf562f11f94658caa1f248a31037.png "android-source-code-to-eclipse-result")

## 参考

1.  【Android 的附加 Eclipse 插件
2.  [Android 开源项目](http://web.archive.org/web/20200920092334/http://source.android.com/)

Tags : [android](http://web.archive.org/web/20200920092334/https://mkyong.com/tag/android/) [eclipse](http://web.archive.org/web/20200920092334/https://mkyong.com/tag/eclipse/) [source code](http://web.archive.org/web/20200920092334/https://mkyong.com/tag/source-code/)<input type="hidden" id="mkyong-current-postId" value="10402">

### 相关文章

*   [如何将 SWT 源代码附加到 Eclipse IDE 上？](/web/20200920092334/https://mkyong.com/swt/how-do-attach-swt-source-code-to-eclipse-ide/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link0)
*   [Maven -获取 Jar 的源代码](/web/20200920092334/https://mkyong.com/maven/maven-get-source-code-for-jar/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link1)
*   [Eclipse -如何附加 JDK 源代码](/web/20200920092334/https://mkyong.com/eclipse/eclipse-how-to-attach-jdk-source-code/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link2)
*   【Eclipse IDE 的 Java 反编译器插件
*   [内容辅助(Ctrl + Space)不起作用- Ecl](/web/20200920092334/https://mkyong.com/java/content-assist-ctrl-space-is-not-working-eclipse/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link4)

*   [在 Eclipse 中将 Java 项目转换为 Web 项目](/web/20200920092334/https://mkyong.com/java/how-to-convert-java-project-to-web-project-in-eclipse/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link5)
*   [如何在 Eclipse IDE 中查看 Java 类层次结构？](/web/20200920092334/https://mkyong.com/java/how-to-view-java-class-hierarchy-in-eclipse-ide/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link6)
*   [如何将 Java 源代码转换成 HTML 页面](/web/20200920092334/https://mkyong.com/java/how-to-convert-java-source-code-to-html-page/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link7)
*   [如何更改 Eclipse splash 欢迎屏幕图像](/web/20200920092334/https://mkyong.com/java/how-to-change-eclipse-splash-welcome-screen-image/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link8)
*   [Java -如何生成 serialVersionUID](/web/20200920092334/https://mkyong.com/java/how-to-generate-serialversionuid/?utm_source=self&utm_medium=referral&utm_campaign=afterpost-related&utm_content=link9)