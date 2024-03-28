# 自定义资源包加载

> 原文：[`docs.oracle.com/javase/tutorial/i18n/resbundle/control.html`](https://docs.oracle.com/javase/tutorial/i18n/resbundle/control.html)

在本课程的前面，您已经学会了如何创建和访问`ResourceBundle`类的对象。本节扩展了您的知识，并解释了如何利用[`ResourceBundle.Control`](https://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.Control.html)类的功能。

`ResourceBundle.Control`被创建用于指定如何定位和实例化资源包。它定义了一组回调方法，这些方法在`ResourceBundle.getBundle`工厂方法在加载资源包过程中调用。

与之前描述的`ResourceBundle.getBundle`方法不同，此`ResourceBundle.getBundle`方法使用指定的基本名称、默认区域设置和指定的控制定义资源包。

```java
public static final ResourceBundle getBundle(
    String baseName,
    ResourceBundle.Control cont
    // ...

```

指定的控制提供了资源包加载过程的信息。

下面的示例程序`RBControl.java`说明了如何为中文区域定义自己的搜索路径。

## 1\. 创建`properties`文件。

正如之前所述，您可以从类或`properties`文件中加载资源。这些文件包含以下区域的描述：

+   ``RBControl.properties``  全局

+   ``RBControl_zh.properties``  仅语言：简体中文

+   ``RBControl_zh_cn.properties``  仅区域：中国

+   ``RBControl_zh_hk.properties``  仅区域：香港

+   ``RBControl_zh_tw.properties``  台湾

在此示例中，应用程序为香港地区创建了一个新的区域设置。

## 2\. 创建一个`ResourceBundle`实例。

与上一节中的示例一样，此应用程序通过调用`getBundle`方法创建了一个`ResourceBundle`实例：

```java
private static void test(Locale locale) {
    ResourceBundle rb = ResourceBundle.getBundle(
                            "RBControl",
                            locale,
                            new ResourceBundle.Control() {
                                    // ...
                            }
                        );

```

`getBundle`方法搜索具有 RBControl 前缀的`properties`文件。然而，此方法包含一个`Control`参数，用于驱动搜索中文区域的过程。

## 3\. 调用`getCandidateLocales`方法

`getCandidateLocales`方法返回一个候选区域的`Locales`对象列表，用于基本名称和区域设置。

```java
new ResourceBundle.Control() {
    @Override
    public List<Locale> getCandidateLocales(
                            String baseName,
                            Locale locale) {
                // ...                                        
    }
}

```

默认实现返回以下`Locale`对象的列表：Locale(语言, 国家)。

然而，此方法被覆盖以实现以下特定行为：

```java
if (baseName == null)
    throw new NullPointerException();

if (locale.equals(new Locale("zh", "HK"))) {
    return Arrays.asList(
               locale,
               Locale.TAIWAN,
               // no Locale.CHINESE here
               Locale.ROOT);
} else if (locale.equals(Locale.TAIWAN)) {
    return Arrays.asList(
               locale,
               // no Locale.CHINESE here
               Locale.ROOT);
}

```

注意，候选区域序列的最后一个元素必须是根区域。

## 4\. 调用`test`类

为以下四种不同的区域设置调用`test`类：

```java
public static void main(String[] args) {
    test(Locale.CHINA);
    test(new Locale("zh", "HK"));
    test(Locale.TAIWAN);
    test(Locale.CANADA);
}

```

## 5\. 运行示例程序

你将看到程序的输出如下：

```java
locale: zh_CN
        region: China
        language: Simplified Chinese
locale: zh_HK
        region: Hong Kong
        language: Traditional Chinese
locale: zh_TW
        region: Taiwan
        language: Traditional Chinese
locale: en_CA
        region: global
        language: English

```

请注意，新创建的区域被分配为香港地区，因为在适当的`properties`文件中指定了。繁体中文被分配为台湾区域的语言。

`ResourceBundle.Control`类的另外两个有趣的方法没有在`RBControl`示例中使用，但值得一提。`getTimeToLive`方法用于确定资源包在缓存中可以存在多长时间。如果缓存中资源包的时间限制已过期，则调用`needsReload`方法来确定是否需要重新加载资源包。
