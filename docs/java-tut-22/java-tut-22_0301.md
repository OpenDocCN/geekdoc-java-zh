# 问题和练习：更多关于富互联网应用程序的内容

> 原文：[`docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/QandE/questions.html`](https://docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/QandE/questions.html)

## 问题

1.  真或假：富互联网应用程序（RIAs）可以通过在属性名称前加上`"jnlp."`来设置安全属性。

1.  真或假：只有签名的富互联网应用程序（RIAs）才能使用 JNLP API 访问客户端上的文件。

## 练习

1.  在以下的 JNLP 文件中，添加一个名为`jnlp.foo`的安全属性，并将其值设置为`true`。

    ```java
    <?xml version="1.0" encoding="UTF-8"?>
    <jnlp spec="1.0+" codebase="" href="">
        <information>
            <title>Dynamic Tree Demo</title>
            <vendor>Dynamic Team</vendor>

        </information>
        <resources>
            <!-- Application Resources -->
            <j2se version="1.6+" href=
                "http://java.sun.com/products/autodl/j2se" />
            <jar href="DynamicTreeDemo.jar" main="true" />
        </resources>
        <applet-desc 
           name="Dynamic Tree Demo Applet"
           main-class="components.DynamicTreeApplet"
           width="300"
           height="300">
         </applet-desc>
         <update check="background"/>
    </jnlp>                           

    ```

检查你的答案。
