# 课程：隔离特定于区域设置的数据

> 原文：[`docs.oracle.com/javase/tutorial/i18n/resbundle/index.html`](https://docs.oracle.com/javase/tutorial/i18n/resbundle/index.html)

特定于区域设置的数据必须根据最终用户语言和地区的惯例进行定制。用户界面显示的文本是最明显的区域设置数据示例。例如，在美国的应用程序中，取消按钮将在德国显示为 "Abbrechen" 按钮。在其他国家，此按钮将具有其他标签。显然，您不希望硬编码此按钮标签。如果您可以自动获取给定 `Locale` 的正确标签，那不是很好吗？幸运的是，只要将特定于区域设置的对象隔离在 `ResourceBundle` 中，您就可以做到这一点。

在本课程中，您将学习如何创建和访问 `ResourceBundle` 对象。如果您急于查看一些编码示例，请继续查看本课程中的最后两节。然后您可以回到前两节获取有关 `ResourceBundle` 对象的一些概念信息。

## 关于 ResourceBundle 类

`ResourceBundle` 对象包含特定于区域设置的对象。当您需要特定于区域设置的对象时，您可以从 `ResourceBundle` 中获取，它会返回与最终用户的 `Locale` 匹配的对象。本节解释了 `ResourceBundle` 与 `Locale` 的关系，并描述了 `ResourceBundle` 的子类。

## 准备使用 ResourceBundle

在创建 `ResourceBundle` 对象之前，您应该进行一些规划。首先，识别程序中特定于区域设置的对象。然后将它们组织成类别，并根据不同的类别存储在不同的 `ResourceBundle` 对象中。

## 使用属性文件支持 ResourceBundle

如果您的应用程序包含需要翻译成各种语言的 `String` 对象，您可以将这些 `String` 对象存储在 `PropertyResourceBundle` 中，该对象由一组属性文件支持。由于属性文件是简单的文本文件，可以由翻译人员创建和维护。您无需更改源代码。在本节中，您将学习如何设置支持 `PropertyResourceBundle` 的属性文件。

## 使用 ListResourceBundle

`ListResourceBundle` 类是 `ResourceBundle` 的子类，使用列表管理特定于区域设置的对象。`ListResourceBundle` 由一个类文件支持，这意味着每次需要支持额外的 `Locale` 时，您必须编写和编译一个新的源文件。但是，`ListResourceBundle` 对象很有用，因为与属性文件不同，它们可以存储任何类型的特定于区域设置的对象。通过逐步执行示例程序，本节演示了如何使用 `ListResourceBundle`。

## 自定义 ResourceBundle 加载

本节代表了改进`ResourceBundle.getBundle`工厂灵活性的新功能。`ResourceBundle.Control`类与加载资源包的工厂方法合作。这允许将资源包加载过程的每个重要步骤及其缓存控制视为单独的方法。
