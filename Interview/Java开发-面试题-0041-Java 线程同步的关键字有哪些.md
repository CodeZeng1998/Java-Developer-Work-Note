# Java开发-面试题-0041-Java 线程同步的关键字有哪些

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**







emmm，前段时间刷面试题突然遇见这种面试题说实话确实把我整懵了，问题过于范了，回答不知道挑啥来说好，下文仅提供一部分的思路。



解答内容如下：

* `synchronized` 关键字（可详细说使用在不同地方的区别）
* `volatile` 关键字（保证可见性）
* JUC包（可举例说明自己项目里具体使用到的一些类，如`CountDownLatch` 等）









### 1. `synchronized` 关键字

`synchronized` 是最常用的线程同步机制，用于保证多个线程在访问共享资源时的互斥性。

#### **作用**

- 方法同步：在方法声明前加上 `synchronized`，表示整个方法是同步的，访问该方法需要获取对象的监视器锁。

  ```java
  public synchronized void method() {
      // 同步方法
  }
  ```

- 代码块同步：只同步某一段关键代码，而不是整个方法。

  ```java
  public void method() {
      synchronized (this) {
          // 同步代码块
      }
  }
  ```

#### **锁定对象**

- 静态方法：锁定的是 **类的 Class 对象**。
- 普通方法：锁定的是当前实例对象 (`this`)。

------



### 2. `volatile` 关键字

`volatile` 用于保证变量的 **可见性** 和 **有序性**。与 `synchronized` 不同，它 **不能保证原子性**，主要用于防止线程间的变量缓存一致性问题。

#### **作用**

- **可见性**：保证一个线程对变量的修改，其他线程能够立即感知到。
- **禁止指令重排序**：确保代码按照编写顺序执行。

#### **示例**

```java
private volatile boolean flag = true;

public void method() {
    while (flag) {
        // 其他线程修改 flag 时，当前线程能够立即感知
    }
}
```

------





### 3. 配合 `java.util.concurrent` 包使用

- **锁机制**：`ReentrantLock` 是一种更灵活的替代 `synchronized` 的同步工具。
- **信号量机制**：`CountDownLatch`、`CyclicBarrier` 等用于线程之间的协调和同步。

如果需要更复杂的线程同步操作，推荐结合 `Lock` 接口及其实现类使用，例如：

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Example {
    private final Lock lock = new ReentrantLock();

    public void method() {
        lock.lock();
        try {
            // 同步代码块
        } finally {
            lock.unlock();
        }
    }
}
```

------



### 总结

- `synchronized`：保证互斥访问和线程安全。
- `volatile`：保证变量的可见性和有序性。
- **推荐**：对于复杂同步需求，使用 `Lock` 和其他工具类（如 `ReadWriteLock`、`Semaphore`）实现更高效、更灵活的线程同步。





<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0041.png?raw=true)

上图是由 Pic 生成的

关键词：Cute Ice Dinosaur

<br/>



> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**



