# 使用属性文件支持 ResourceBundle

> 原文：[`docs.oracle.com/javase/tutorial/i18n/resbundle/propfile.html`](https://docs.oracle.com/javase/tutorial/i18n/resbundle/propfile.html)

本节将逐步介绍一个名为``PropertiesDemo``的示例程序。

## 1\. 创建默认属性文件

属性文件是一个简单的文本文件。您可以使用几乎任何文本编辑器创建和维护属性文件。

您应该始终创建一个默认属性文件。此文件的名称以您的`ResourceBundle`的基本名称开头，并以`.properties`后缀结尾。在`PropertiesDemo`程序中，基本名称为`LabelsBundle`。因此，默认属性文件称为`LabelsBundle.properties`。此文件包含以下行：

```java
# This is the default LabelsBundle.properties file
s1 = computer
s2 = disk
s3 = monitor
s4 = keyboard

```

请注意，在前面的文件中，注释行以井号（#）开头。其他行包含键值对。键位于等号的左侧，值位于右侧。例如，`s2`是对应于值`disk`的键。键是任意的。我们可以将`s2`命名为其他名称，比如`msg5`或`diskID`。但一旦定义，键就不应更改，因为它在源代码中被引用。值可以更改。实际上，当您的本地化人员创建新的属性文件以适应其他语言时，他们将把值翻译成各种语言。

## 2\. 根据需要创建其他属性文件

要支持额外的`Locale`，您的本地化人员将创建一个包含翻译值的新属性文件。不需要更改源代码，因为您的程序引用键，而不是值。

例如，要添加对德语的支持，您的本地化人员将翻译`LabelsBundle.properties`中的值，并将其放入名为`LabelsBundle_de.properties`的文件中。请注意，此文件的名称与默认文件的名称相同，以基本名称`LabelsBundle`开头，并以`.properties`后缀结尾。但是，由于此文件用于特定的`Locale`，因此基本名称后面跟着语言代码（`de`）。`LabelsBundle_de.properties`的内容如下：

```java
# This is the LabelsBundle_de.properties file
s1 = Computer
s2 = Platte
s3 = Monitor
s4 = Tastatur

```

`PropertiesDemo`示例程序附带三个属性文件：

```java
LabelsBundle.properties
LabelsBundle_de.properties
LabelsBundle_fr.properties

```

## 3\. 指定 Locale

`PropertiesDemo`程序创建`Locale`对象如下：

```java
Locale[] supportedLocales = {
    Locale.FRENCH,
    Locale.GERMAN,
    Locale.ENGLISH
};

```

这些`Locale`对象应该与前两个步骤中创建的属性文件相匹配。例如，`Locale.FRENCH`对象对应于`LabelsBundle_fr.properties`文件。`Locale.ENGLISH`没有匹配的`LabelsBundle_en.properties`文件，因此将使用默认文件。

## 4\. 创建 ResourceBundle

此步骤展示了`Locale`、属性文件和`ResourceBundle`之间的关系。要创建`ResourceBundle`，请调用`getBundle`方法，指定基本名称和`Locale`：

```java
ResourceBundle labels = ResourceBundle.getBundle("LabelsBundle", currentLocale);

```

`getBundle`方法首先查找与基本名称和`Locale`匹配的类文件。如果找不到类文件，则会检查属性文件。在`PropertiesDemo`程序中，我们使用属性文件而不是类文件来支持`ResourceBundle`。当`getBundle`方法找到正确的属性文件时，它会返回一个包含属性文件中键值对的`PropertyResourceBundle`对象。

## 5\. 获取本地化文本

要从`ResourceBundle`中检索翻译后的值，请按照以下方式调用`getString`方法：

```java
String value = labels.getString(key);

```

`getString`返回的`String`对应指定的键。只要为指定的`Locale`存在属性文件，该`String`就是正确的语言。

## 6\. 遍历所有键

这一步是可选的。在调试程序时，您可能希望获取`ResourceBundle`中所有键的值。`getKeys`方法返回`ResourceBundle`中所有键的`Enumeration`。您可以遍历`Enumeration`并使用`getString`方法获取每个值。以下代码片段来自`PropertiesDemo`程序，展示了如何实现这一点：

```java
ResourceBundle labels = ResourceBundle.getBundle("LabelsBundle", currentLocale);
Enumeration bundleKeys = labels.getKeys();

while (bundleKeys.hasMoreElements()) {
    String key = (String)bundleKeys.nextElement();
    String value = labels.getString(key);
    System.out.println("key = " + key + ", " + "value = " + value);
}

```

## 7\. 运行演示程序

运行`PropertiesDemo`程序会生成以下输出。前三行显示了对各种`Locale`对象调用`getString`返回的值。当使用`getKeys`方法遍历键时，程序会显示最后四行。

```java
Locale = fr, key = s2, value = Disque dur
Locale = de, key = s2, value = Platte
Locale = en, key = s2, value = disk

key = s4, value = Clavier
key = s3, value = Moniteur
key = s2, value = Disque dur
key = s1, value = Ordinateur

```
