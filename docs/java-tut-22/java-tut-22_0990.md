# 添加、替换具有属性的绑定

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/bindattr.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/bindattr.html)

讨论了命名示例如何使用`bind()`，`rebind()`。[`DirContext`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html)接口包含这些方法的重载版本，接受属性。您可以使用这些`DirContext`方法在将绑定或子上下文添加到命名空间时将属性与对象关联起来。例如，您可以创建一个`Person`对象并将其绑定到命名空间，并同时关联有关该`Person`对象的属性。

## 添加具有属性的绑定

[`DirContext.bind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#bind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-)用于向上下文添加具有属性的绑定。它接受对象的名称、要绑定的对象和一组属性作为参数。

```java
// Create the object to be bound
Fruit fruit = new Fruit("orange");

// Create attributes to be associated with the object
Attributes attrs = new BasicAttributes(true); // case-ignore
Attribute objclass = new BasicAttribute("objectclass");
objclass.add("top");
objclass.add("organizationalUnit");
attrs.put(objclass);

// Perform bind
ctx.bind("ou=favorite, ou=Fruits", fruit, attrs);

```

`此示例`创建一个``Fruit``类的对象，并将其绑定到名称`"ou=favorite"`中，相对于`ctx`命名为`"ou=Fruits"`的上下文。此绑定具有`"objectclass"`属性。如果随后在`ctx`中查找名称`"ou=favorite, ou=Fruits"`，则会获取`fruit`对象。然后获取`"ou=favorite, ou=Fruits"`的属性，您将获得创建对象时使用的属性。以下是此示例的输出。

```java
# java Bind
orange
attribute: objectclass
value: top
value: organizationalUnit
value: javaObject
value: javaNamingReference
attribute: javaclassname
value: Fruit
attribute: javafactory
value: FruitFactory
attribute: javareferenceaddress
value: #0#fruit#orange
attribute: ou
value: favorite

```

显示的额外属性和属性值用于存储有关对象（`fruit`）的信息。这些额外属性在本教程中将更详细地讨论。

如果您运行此示例两次，则第二次尝试将失败，并显示[`NameAlreadyBoundException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameAlreadyBoundException.html)。这是因为名称`"ou=favorite"`已经绑定在`"ou=Fruits"`上下文中。为了使第二次尝试成功，您需要使用`rebind()`。

## 替换具有属性的绑定

[`DirContext.rebind()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#rebind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-)用于添加或替换绑定及其属性。它接受与`bind()`相同的参数。然而，`rebind()`的语义要求，如果名称已经绑定，则将解除绑定，并绑定新给定的对象和属性。

```java
// Create the object to be bound
Fruit fruit = new Fruit("lemon");

// Create attributes to be associated with the object
Attributes attrs = new BasicAttributes(true); // case-ignore
Attribute objclass = new BasicAttribute("objectclass");
objclass.add("top");
objclass.add("organizationalUnit");
attrs.put(objclass);

// Perform bind
ctx.rebind("ou=favorite, ou=Fruits", fruit, attrs);

```

当您运行`此示例`时，它将替换``bind()``示例创建的绑定。

```java
# java Rebind
lemon
attribute: objectclass
value: top
value: organizationalUnit
value: javaObject
value: javaNamingReference
attribute: javaclassname
value: Fruit
attribute: javafactory
value: FruitFactory
attribute: javareferenceaddress
value: #0#fruit#lemon
attribute: ou
value: favorite

```
