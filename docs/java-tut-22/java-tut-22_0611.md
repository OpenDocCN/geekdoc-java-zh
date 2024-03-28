# 提高排序性能

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/perform.html`](https://docs.oracle.com/javase/tutorial/i18n/text/perform.html)

对长字符串列表进行排序通常是耗时的。如果您的排序算法重复比较字符串，可以通过使用`CollationKey`类加快该过程。

一个[`CollationKey`](https://docs.oracle.com/javase/8/docs/api/java/text/CollationKey.html)对象代表给定`String`和`Collator`的排序键。比较两个`CollationKey`对象涉及对排序键进行按位比较，比使用`Collator.compare`方法比较`String`对象更快。然而，生成`CollationKey`对象需要时间。因此，如果一个`String`只需比较一次，`Collator.compare`提供更好的性能。

接下来的示例使用`CollationKey`对象对单词数组进行排序。此示例的源代码在`KeysDemo.java`中。

`KeysDemo`程序在`main`方法中创建一个`CollationKey`对象数组。要创建`CollationKey`，需要在`Collator`对象上调用`getCollationKey`方法。除非两个`CollationKey`对象来自同一个`Collator`，否则无法比较它们。`main`方法如下：

```java
static public void main(String[] args) {
    Collator enUSCollator = Collator.getInstance(new Locale("en","US"));
    String [] words = {
        "peach",
        "apricot",
        "grape",
        "lemon"
    };

    CollationKey[] keys = new CollationKey[words.length];

    for (int k = 0; k < keys.length; k ++) {
        keys[k] = enUSCollator. getCollationKey(words[k]);
    }

    sortArray(keys);
    printArray(keys);
}

```

`sortArray`方法调用`CollationKey.compareTo`方法。`compareTo`方法返回一个整数，如果`keys[i]`对象小于、等于或大于`keys[j]`对象，则返回小于零、等于零或大于零。请注意，程序比较的是`CollationKey`对象，而不是原始单词数组中的`String`对象。以下是`sortArray`方法的代码：

```java
public static void sortArray(CollationKey[] keys) {
    CollationKey tmp;

    for (int i = 0; i < keys.length; i++) {
        for (int j = i + 1; j < keys.length; j++) {
            if (keys[i].compareTo(keys[j]) > 0) {
                tmp = keys[i];
                keys[i] = keys[j];
                keys[j] = tmp; 
            }
        }
    }
}

```

`KeysDemo`程序对`CollationKey`对象数组进行排序，但最初的目标是对`String`对象数组进行排序。为了检索每个`CollationKey`的`String`表示形式，程序在`displayWords`方法中调用`getSourceString`，如下所示：

```java
static void displayWords(CollationKey[] keys) {
    for (int i = 0; i < keys.length; i++) {
        System.out.println(keys[i].getSourceString());
    }
}

```

`displayWords`方法打印以下行：

```java
apricot
grape
lemon
peach

```
