# 类字面量作为运行时类型标记

> 原文：[`docs.oracle.com/javase/tutorial/extra/generics/literals.html`](https://docs.oracle.com/javase/tutorial/extra/generics/literals.html)

JDK 5.0 中的一个变化是类`java.lang.Class`是泛型的。这是一个有趣的例子，使用泛型性来做除了容器类之外的事情。

现在`Class`有一个类型参数`T`，你可能会问，`T`代表什么？它代表`Class`对象所代表的类型。

例如，`String.class`的类型是`Class<String>`，`Serializable.class`的类型是`Class<Serializable>`。这可以用来提高反射代码的类型安全性。

特别是，由于`Class`中的`newInstance()`方法现在返回一个`T`，在通过反射创建对象时可以获得更精确的类型。

例如，假设你需要编写一个实用方法，执行数据库查询，给定一个 SQL 字符串，并返回与该查询匹配的数据库中的对象集合。

一种方法是显式传递一个工厂对象，在调用点编写代码如下：

```java
interface Factory<T> { T make();} 

public <T> Collection<T> select(Factory<T> factory, String statement) { 
    Collection<T> result = new ArrayList<T>(); 

    /* *Run sql query using jdbc* */  
    for (/* *Iterate over jdbc results.* */) { 
        T item = factory.make();
        /* *Use reflection and set all of item's 
         * fields from sql results.* 
         */ 
        result.add(item); 
    } 
    return result; 
}

```

你可以这样调用

```java
select(new Factory<EmpInfo>(){ 
    public EmpInfo make() {
        return new EmpInfo();
    }}, "selection string");

```

或者你可以声明一个类`EmpInfoFactory`来支持`Factory`接口

```java
class EmpInfoFactory implements Factory<EmpInfo> {
    ...
    public EmpInfo make() { 
        return new EmpInfo();
    }
}

```

并调用它

```java
select(getMyEmpInfoFactory(), "selection string");

```

这种解决方案的缺点是需要：

+   使用冗长的匿名工厂类在调用点，或者

+   为每种使用的类型声明一个工厂类，并在调用点传递一个工厂实例，这有点不自然。

将类字面量作为工厂对象是很自然的，然后可以通过反射来使用。今天（没有泛型的情况下）代码可能会这样写：

```java
Collection emps = sqlUtility.select(EmpInfo.class, "select * from emps");
...
public static Collection select(Class c, String sqlStatement) { 
    Collection result = new ArrayList();
    /* *Run sql query using jdbc.* */
    for (/* *Iterate over jdbc results.* */ ) { 
        Object item = c.newInstance(); 
        /* *Use reflection and set all of item's
         * fields from sql results.* 
         */  
        result.add(item); 
    } 
    return result; 
}

```

然而，这不会给我们提供我们想要的精确类型的集合。现在`Class`是泛型的，我们可以改为写如下代码：

```java
Collection<EmpInfo> 
    emps = sqlUtility.select(EmpInfo.class, "select * from emps");
...
public static <T> Collection<T> select(Class<T> c, String sqlStatement) { 
    Collection<T> result = new ArrayList<T>();
    /* *Run sql query using jdbc.* */
    for (/* *Iterate over jdbc results.* */ ) { 
        T item = c.newInstance(); 
        /* *Use reflection and set all of item's
         * fields from sql results.* 
         */  
        result.add(item);
    } 
    return result; 
} 

```

上面的代码以类型安全的方式给出了我们想要的精确类型的集合。

使用类字面量作为运行时类型标记的技术是一个非常有用的技巧。这是一个在新的用于操作注解的 API 中广泛使用的习语。
