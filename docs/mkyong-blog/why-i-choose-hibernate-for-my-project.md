# 为什么我的项目选择 Hibernate？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/why-i-choose-hibernate-for-my-project/>

目前我的公司使用 IBATIS 和纯 SQL 作为数据库持久化机制。我非常喜欢 SQL 查询，尤其是在调优方面，但我不喜欢在 Java 应用程序中编写所有的 SQL 语句，这很容易出现打字错误，这是一项多么愚蠢和繁琐的工作？

最后，我的公司有了一个新项目，这是提出 Hibernate 作为我们新的 java 数据库持久化机制工具的恰当时机。为了说服我的老板接受 Hibernate 作为新项目的考虑因素，我必须强调 Hibernate 的好处和优势。

## 为什么是 O/R 映射？

## 1.生产力

它帮助开发人员摆脱编写复杂繁琐的 SQL 语句，不再需要 JDBC API 进行结果集或数据处理。它使开发人员更专注于业务逻辑，并提高了项目的生产率。

## 2.可维护性

它有助于减少代码行，使系统更容易理解，更强调业务逻辑而不是持久性工作(SQL)。更重要的是，代码更少的系统更容易重构。

## 3.轻便

它将我们的应用程序从底层 sql 数据库和 SQL 方言中抽象出来。切换到其他 SQL 数据库只需对 Hibernate 配置文件进行少量修改(一次编写/随处运行)。

> 老板:嗯，听起来是个有趣的工具。(事实上，我不知道你在说什么)有没有其他的 O/R 映射工具或者数据库持久化机制可以做和 Hibernate 一样的事情？
> **我:**是的，老板，它确实存在，但是我需要告诉你，为什么我选择冬眠。

## 为什么选择 Hibernate 而不是其他？

Java 中流行的开源持久性框架

1.**冬眠**–【http://www.hibernate.org/】
2[。**EJB 3**–](http://web.archive.org/web/20201109025833/http://www.hibernate.org/)[http://java.sun.com/products/ejb/index.jsp](http://web.archive.org/web/20201109025833/http://java.sun.com/products/ejb/index.jsp)
3。**甲骨文热门链接**–[http://www . Oracle . com/technology/products/IAS/toplink/index . html](http://web.archive.org/web/20201109025833/http://www.oracle.com/technology/products/ias/toplink/index.html)–
4 .**卡宴**–[http://cayenne.apache.org/](http://web.archive.org/web/20201109025833/http://cayenne.apache.org/)
5。**开 JPA【http://openjpa.apache.org/】–
6。伊巴蒂斯**–[http://ibatis.apache.org/javadownloads.cgi](http://web.archive.org/web/20201109025833/http://ibatis.apache.org/javadownloads.cgi)
7。**JPOX**–[http://www.jpox.org/](http://web.archive.org/web/20201109025833/http://www.jpox.org/)

我不想单独比较每个 O/R 或非 O/R 持久性机制，这可能需要花费一年多的时间进行研究和实践比较。基于我个人的拙见，我将选择 Hibernate 作为最好的 O/R 映射持久化机制。

## 选择 Hibernate 的原因

## 1.生产力、可维护性、可移植性

Hibernate 提供了以上所有的 O/R 优势。(生产力、可维护性、可移植性)。

## 2.免费–经济高效

Hibernate 是免费和开源的——性价比高

## 3.学习曲线很短

因为我们都有使用 Hibernate 的工作经验，而且 Hibernate 是完全面向对象的概念，这将缩短我们的学习曲线。

## 4.代码生成工具

社区提供的 Hibernate 工具可以帮助开发者快速简单地生成或开发 hibernate 应用程序。(Eclipse 的插件和代码生成工具)

## 5.流行的

Hibernate 很受欢迎，当我们用 Hibernate 出错时，我们可以很容易地从 Google 找到答案。此外，还有很多关于 Hibernate 的书籍、社区和论坛。

> **我:**嗨老板，嗨，嗨，你在听吗？
> **老板:**……对，对，我明白你的意思。
> **我:** …。(真的吗？)
> 
> **我:** ……(免费…发货日期…这是老板)谢谢老板…你太棒了！

这里还有一个使用 Hibernate 的好处是我没有通知老板的。可能是我加的 6 号。

## 6)市场需求

Java 市场需要 Hibernate 开发人员，与其他工具相比，Hibernate 开发人员的需求正在稳步增长。Hibernate 的工作经验无疑为我的下一次跳跃增加了优势。你认为我应该通知我的老板吗？🙂

## 参考

Hibernate tools (Eclipse 插件&代码生成)
[http://www.hibernate.org/255.html](http://web.archive.org/web/20201109025833/http://www.hibernate.org/255.html)
[http://www . hibernate . org/Hib _ docs/tools/reference/en/html _ single/](http://web.archive.org/web/20201109025833/http://www.hibernate.org/hib_docs/tools/reference/en/html_single/)

1.Hibernate 是最好的选择吗？
[http://java.dzone.com/news/hibernate-best-choice](http://web.archive.org/web/20201109025833/http://java.dzone.com/news/hibernate-best-choice)

2.休眠 VS TopLink VS CMP
http://www.theserverside.com/discussions/thread.tss?thread_id=27037

3.http://www.theserverside.com/discussions/thread.tss? hibernate vs EJB 3.0 持久性
[thread_id=38800](http://web.archive.org/web/20201109025833/http://www.theserverside.com/discussions/thread.tss?thread_id=38800)

4.基于 Hibernate vs . JDO vs . EJB 3 的持久性的优缺点新的 J2EE 应用程序来构建
[http://saloon.javaranch.com/cgi-bin/ubb/ultimatebb.cgi?ubb = get _ topic&f = 78&t = 000927](http://web.archive.org/web/20201109025833/http://saloon.javaranch.com/cgi-bin/ubb/ultimatebb.cgi?ubb=get_topic&f=78&t=000927)

5.iBATIS 与 Hibernate——是什么原因导致了二者的选择？
[http://www.javalobby.org/java/forums/t16496.html](http://web.archive.org/web/20201109025833/http://www.javalobby.org/java/forums/t16496.html)

6.Java 中的开源持久性框架
[http://java-source.net/open-source/persistence](http://web.archive.org/web/20201109025833/http://java-source.net/open-source/persistence)

Tags : [hibernate](http://web.archive.org/web/20201109025833/https://mkyong.com/tag/hibernate/) [java](http://web.archive.org/web/20201109025833/https://mkyong.com/tag/java/)<input type="hidden" id="mkyong-current-postId" value="508">

### 相关文章

*   [检查数组](/web/20201109025833/https://www.mkyong.com/java/check-duplicated-value-in-array/)中的重复值
*   [如何在 fedora core (linux)上安装 Java JDK](/web/20201109025833/https://www.mkyong.com/java/how-to-install-java-jdk-on-fedora-core-linux/)
*   [在 Java windows 或 Linux 中打开浏览器](/web/20201109025833/https://www.mkyong.com/java/open-browser-in-java-windows-or-linux/)
*   【Eclipse IDE 的 Java 反编译器插件
*   [如何去除字符串- Java 之间的空格](/web/20201109025833/https://www.mkyong.com/java/how-to-remove-whitespace-between-string-java/)

*   If 语句小心！！！
*   [Java . lang . unsupportedclassversionerror:错误的版本](/web/20201109025833/https://www.mkyong.com/java/javalangunsupportedclassversionerror-bad-version-number-in-class-file/)
*   [如何在 Java 中删除临时文件](/web/20201109025833/https://www.mkyong.com/java/how-to-delete-temporary-file-in-java/)
*   [Java Web Start (Jnlp)教程](/web/20201109025833/https://www.mkyong.com/java/java-web-start-jnlp-tutorial-unofficial-guide/)
*   [如何将数据导出到 CSV 文件- Java](/web/20201109025833/https://www.mkyong.com/java/how-to-export-data-to-csv-file-java/)