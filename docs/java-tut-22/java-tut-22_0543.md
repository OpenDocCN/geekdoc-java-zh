# 包装器实现

> 原文：[`docs.oracle.com/javase/tutorial/collections/implementations/wrapper.html`](https://docs.oracle.com/javase/tutorial/collections/implementations/wrapper.html)

包装器实现将所有真正的工作委托给指定的集合，但在该集合提供的功能之上添加额外的功能。对于设计模式爱好者，这是*装饰者*模式的一个例子。虽然这可能看起来有点奇特，但实际上非常简单。

这些实现是匿名的；而不是提供一个公共类，库提供了一个静态工厂方法。所有这些实现都在 [`Collections`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html) 类中，该类仅包含静态方法。

## 同步包装器

同步包装器为任意集合添加了自动同步（线程安全）。六个核心集合接口  [`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html), [`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html), [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html), [`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html), [`SortedSet`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html), 和 [`SortedMap`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html)  都有一个静态工厂方法。

```java
public static <T> Collection<T> synchronizedCollection(Collection<T> c);
public static <T> Set<T> synchronizedSet(Set<T> s);
public static <T> List<T> synchronizedList(List<T> list);
public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m);
public static <T> SortedSet<T> synchronizedSortedSet(SortedSet<T> s);
public static <K,V> SortedMap<K,V> synchronizedSortedMap(SortedMap<K,V> m);

```

这些方法中的每一个都返回由指定集合支持的同步（线程安全）`Collection`。为了保证串行访问，*所有*对支持集合的访问必须通过返回的集合完成。确保这一点的简单方法是不保留对支持集合的引用。使用以下技巧创建同步集合。

```java
List<Type> list = Collections.synchronizedList(new ArrayList<Type>());

```

以这种方式创建的集合与通常同步的集合（如 [`Vector`](https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html)）一样线程安全。

面对并发访问时，在迭代时，用户必须手动对返回的集合进行同步。原因是迭代是通过对集合的多次调用完成的，这些调用必须组合成单个原子操作。以下是迭代包装同步集合的惯用法。

```java
Collection<Type> c = Collections.synchronizedCollection(myCollection);
synchronized(c) {
    for (Type e : c)
        foo(e);
}

```

如果使用显式迭代器，则必须在 `synchronized` 块内调用 `iterator` 方法。不遵循此建议可能导致不确定的行为。在同步 `Map` 的 `Collection` 视图上进行迭代的惯用法类似。在迭代任何 `Collection` 视图时，用户必须同步在同步的 `Map` 上，而不是在 `Collection` 视图本身上进行同步，如下例所示。

```java
Map<KeyType, ValType> m = Collections.synchronizedMap(new HashMap<KeyType, ValType>());
    ...
Set<KeyType> s = m.keySet();
    ...
// Synchronizing on m, not s!
synchronized(m) {
    while (KeyType k : s)
        foo(k);
}

```

使用包装器实现的一个小缺点是，您无法执行包装实现的任何*非接口*操作。因此，在前面的`List`示例中，您无法在包装的`ArrayList`上调用`ArrayList`的[`ensureCapacity`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#ensureCapacity-int-)操作。

## 不可修改的包装器

与添加功能到包装集合的同步包装器不同，不可修改的包装器会剥夺功能。特别是，它们剥夺了通过拦截所有可能修改集合的操作并抛出`UnsupportedOperationException`来修改集合的能力。不可修改的包装器有两个主要用途，如下所示：

+   使集合在构建后变为不可变。在这种情况下，最好不要保留对支持集合的引用。这绝对保证了不可变性。

+   允许某些客户端对您的数据结构进行只读访问。您保留对支持集合的引用，但分发对包装器的引用。这样，客户端可以查看但不能修改，而您保持完全访问权限。

与同步包装器一样，每个六个核心`Collection`接口都有一个静态工厂方法。

```java
public static <T> Collection<T> unmodifiableCollection(Collection<? extends T> c);
public static <T> Set<T> unmodifiableSet(Set<? extends T> s);
public static <T> List<T> unmodifiableList(List<? extends T> list);
public static <K,V> Map<K, V> unmodifiableMap(Map<? extends K, ? extends V> m);
public static <T> SortedSet<T> unmodifiableSortedSet(SortedSet<? extends T> s);
public static <K,V> SortedMap<K, V> unmodifiableSortedMap(SortedMap<K, ? extends V> m);

```

## 检查接口包装器

`Collections.checked` *接口*包装器用于与泛型集合一起使用。这些实现返回指定集合的*动态*类型安全视图，如果客户端尝试添加错误类型的元素，则会抛出`ClassCastException`。语言中的泛型机制提供了编译时（静态）类型检查，但有可能击败此机制。动态类型安全视图完全消除了这种可能性。
