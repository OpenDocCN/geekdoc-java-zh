# 补充字符作为代理

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/supplementaryChars.html`](https://docs.oracle.com/javase/tutorial/i18n/text/supplementaryChars.html)

为了支持补充字符而不改变`char`原始数据类型并导致与之前的 Java 程序不兼容，补充字符被定义为一对称为*代理*的代码点值。 第一个代码点来自`U+D800`到`U+DBFF`的*高代理*范围，第二个代码点来自`U+DC00`到`U+DFFF`的*低代理*范围。 例如，Deseret 字符 LONG I，`U+10400`，是用这对代理值定义的：`U+D801`和`U+DC00`。
