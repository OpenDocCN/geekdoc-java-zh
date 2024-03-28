# 并发集合

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/collections.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/collections.html)

`java.util.concurrent` 包包含了许多对 Java 集合框架的补充。这些最容易通过提供的集合接口进行分类：

+   [`BlockingQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html) 定义了一个先进先出的数据结构，当尝试向满队列添加或从空队列检索时会阻塞或超时。

+   [`ConcurrentMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html) 是 [`java.util.Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html) 的子接口，定义了有用的原子操作。这些操作仅在键存在时移除或替换键值对，或仅在键不存在时添加键值对。使这些操作原子化有助于避免同步。`ConcurrentMap` 的标准通用实现是 [`ConcurrentHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html)，它是 [`HashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html) 的并发模拟。

+   [`ConcurrentNavigableMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentNavigableMap.html) 是 `ConcurrentMap` 的子接口，支持近似匹配。`ConcurrentNavigableMap` 的标准通用实现是 [`ConcurrentSkipListMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentSkipListMap.html)，它是 [`TreeMap`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html) 的并发模拟。

所有这些集合都有助于避免内存一致性错误，通过定义将一个对象添加到集合的操作与随后访问或移除该对象的操作之间的 happens-before 关系。
