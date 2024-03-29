# 饥饿和活锁

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/starvelive.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/starvelive.html)

饥饿和活锁问题比死锁问题要少见得多，但仍然是每个并发软件设计者可能会遇到的问题。

## 饥饿

*饥饿*描述了一个线程无法定期访问共享资源并且无法取得进展的情况。这种情况发生在"贪婪"线程长时间地使共享资源不可用时。例如，假设一个对象提供了一个经常需要很长时间才能返回的同步方法。如果一个线程频繁调用这个方法，其他也需要频繁同步访问同一对象的线程将经常被阻塞。

## 活锁

一个线程经常是作为对另一个线程动作的响应。如果另一个线程的动作也是对另一个线程动作的响应，那么可能会发生*livelock*。与死锁类似，活锁的线程无法取得进一步的进展。然而，这些线程并没有被阻塞  它们只是忙于相互响应而无法恢复工作。这就好比两个人试图在走廊里互相让对方通过：阿方斯向左移动让加斯通通过，而加斯通向右移动让阿方斯通过。看到他们仍然互相阻挡，阿方斯向右移动，而加斯通向左移动。他们仍然互相阻挡，所以...
