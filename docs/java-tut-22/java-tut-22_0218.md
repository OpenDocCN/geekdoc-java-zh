# 锁对象

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/newlocks.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/newlocks.html)

同步代码依赖于一种简单的可重入锁。这种类型的锁易于使用，但有许多限制。更复杂的锁习语由 [`java.util.concurrent.locks`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/package-summary.html) 包支持。我们不会详细讨论此包，而是专注于其最基本的接口 [`Lock`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/Lock.html)。

`Lock` 对象的工作方式与同步代码中使用的隐式锁非常相似。与隐式锁一样，一次只有一个线程可以拥有 `Lock` 对象。`Lock` 对象还支持通过其关联的 [`Condition`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/Condition.html) 对象实现 `wait/notify` 机制。

`Lock` 对象相对于隐式锁的最大优势在于其能够在尝试获取锁时撤销操作。如果指定了超时时间，`tryLock` 方法在锁不可用时或超时之前会撤销操作。`lockInterruptibly` 方法在获取锁之前如果另一个线程发送中断信号，则会撤销操作。

让我们使用 `Lock` 对象来解决我们在 Liveness 中看到的死锁问题。阿方索和加斯顿已经训练自己注意到朋友即将鞠躬的时刻。我们通过要求我们的 `Friend` 对象必须在继续鞠躬之前为*两个*参与者获取锁来模拟这种改进。这是改进模型的源代码，``Safelock``。为了展示这种习语的多功能性，我们假设阿方索和加斯顿如此迷恋他们新发现的安全鞠躬能力，以至于他们无法停止向彼此鞠躬：

```java

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.Random;

public class Safelock {
    static class Friend {
        private final String name;
        private final Lock lock = new ReentrantLock();

        public Friend(String name) {
            this.name = name;
        }

        public String getName() {
            return this.name;
        }

        public boolean impendingBow(Friend bower) {
            Boolean myLock = false;
            Boolean yourLock = false;
            try {
                myLock = lock.tryLock();
                yourLock = bower.lock.tryLock();
            } finally {
                if (! (myLock && yourLock)) {
                    if (myLock) {
                        lock.unlock();
                    }
                    if (yourLock) {
                        bower.lock.unlock();
                    }
                }
            }
            return myLock && yourLock;
        }

        public void bow(Friend bower) {
            if (impendingBow(bower)) {
                try {
                    System.out.format("%s: %s has"
                        + " bowed to me!%n", 
                        this.name, bower.getName());
                    bower.bowBack(this);
                } finally {
                    lock.unlock();
                    bower.lock.unlock();
                }
            } else {
                System.out.format("%s: %s started"
                    + " to bow to me, but saw that"
                    + " I was already bowing to"
                    + " him.%n",
                    this.name, bower.getName());
            }
        }

        public void bowBack(Friend bower) {
            System.out.format("%s: %s has" +
                " bowed back to me!%n",
                this.name, bower.getName());
        }
    }

    static class BowLoop implements Runnable {
        private Friend bower;
        private Friend bowee;

        public BowLoop(Friend bower, Friend bowee) {
            this.bower = bower;
            this.bowee = bowee;
        }

        public void run() {
            Random random = new Random();
            for (;;) {
                try {
                    Thread.sleep(random.nextInt(10));
                } catch (InterruptedException e) {}
                bowee.bow(bower);
            }
        }
    }

    public static void main(String[] args) {
        final Friend alphonse =
            new Friend("Alphonse");
        final Friend gaston =
            new Friend("Gaston");
        new Thread(new BowLoop(alphonse, gaston)).start();
        new Thread(new BowLoop(gaston, alphonse)).start();
    }
}

```
