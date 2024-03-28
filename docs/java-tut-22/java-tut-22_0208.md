# 内在锁和同步

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html)

同步建立在一个称为*内在锁*或*监视器锁*的内部实体周围。内在锁在同步的两个方面发挥作用：强制对对象状态的独占访问和建立对可见性至关重要的 happens-before 关系。

每个对象都有一个与之关联的内在锁。按照惯例，需要独占和一致访问对象字段的线程在访问这些字段之前必须*获取*对象的内在锁，然后在完成后*释放*内在锁。线程在获取锁和释放锁之间被认为*拥有*内在锁。只要一个线程拥有内在锁，其他线程就无法获取相同的锁。当另一个线程尝试获取锁时，它将被阻塞。

当一个线程释放一个内在锁时，该操作与后续获取相同锁的任何操作建立 happens-before 关系。

## 同步方法中的锁

当一个线程调用一个同步方法时，它会自动获取该方法对象的内在锁，并在方法返回时释放它。即使返回是由未捕获的异常引起的，锁也会被释放。

你可能会想知道当调用静态同步方法时会发生什么，因为静态方法与类相关联，而不是对象。在这种情况下，线程会获取与类相关联的`Class`对象的内在锁。因此，对类的静态字段的访问受到一个与类的任何实例的锁不同的锁的控制。

## 同步语句

创建同步代码的另一种方法是使用*synchronized 语句*。与同步方法不同，同步语句必须指定提供内在锁的对象：

```java
public void addName(String name) {
    synchronized(this) {
        lastName = name;
        nameCount++;
    }
    nameList.add(name);
}

```

在这个例子中，`addName`方法需要同步对`lastName`和`nameCount`的更改，但也需要避免同步调用其他对象的方法。（从同步代码调用其他对象的方法可能会导致在 Liveness 部分描述的问题。）如果没有同步语句，就必须有一个单独的、非同步的方法，唯一目的是调用`nameList.add`。

同步语句也有助于通过细粒度同步提高并发性。例如，假设类`MsLunch`有两个实例字段，`c1`和`c2`，它们永远不会同时使用。所有这些字段的更新必须同步，但没有理由阻止对 c1 的更新与对 c2 的更新交错这样做会通过创建两个仅用于提供锁的对象来减少并发性。

```java
public class MsLunch {
    private long c1 = 0;
    private long c2 = 0;
    private Object lock1 = new Object();
    private Object lock2 = new Object();

    public void inc1() {
        synchronized(lock1) {
            c1++;
        }
    }

    public void inc2() {
        synchronized(lock2) {
            c2++;
        }
    }
}

```

使用这种习语要非常小心。你必须绝对确定交错访问受影响字段是安全的。

## 可重入同步

请记住，一个线程不能获取另一个线程拥有的锁。但一个线程*可以*获取它已经拥有的锁。允许一个线程多次获取相同的锁使*可重入同步*成为可能。这描述了一种情况，即同步代码直接或间接地调用一个也包含同步代码的方法，并且两组代码使用相同的锁。如果没有可重入同步，同步代码将不得不采取许多额外的预防措施，以避免一个线程导致自己被阻塞。
