# Java开发-面试题-0042-sleep()和wait()的区别

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**





PS： 看见这问题突然想起一个笑话，蛮搞笑的，就是在js代码某个调用的功能里面添加sleep(5s)的方法，后续方便拿钱优化系统，当然这也只是一个段子，现在的人这么精明是不可能被这个东西骗到的，哈哈哈哈。





### `sleep()` 和 `wait()` 的区别（详细说明）

在 Java 中，`Thread.sleep()` 和 `Object.wait()` 是两种用于线程控制的方法，但它们的功能和使用场景完全不同。

------

### 1. **基本概念**

| **特性**   | **`sleep()`**                  | **`wait()`**                                                 |
| ---------- | ------------------------------ | ------------------------------------------------------------ |
| 所属类     | `Thread` 类的静态方法          | `Object` 类的实例方法                                        |
| 功能       | 暂停当前线程一段时间，不释放锁 | 暂停当前线程，直到被唤醒（通过 `notify` 或 `notifyAll`），并释放锁 |
| 是否释放锁 | **不释放锁**                   | **释放锁**                                                   |
| 使用范围   | 适用于任何场景，暂停线程执行   | 用于线程间协作，必须在同步块或方法中                         |

------



### 2. **功能对比**

#### **1) `sleep()`**

- **定义**：`Thread.sleep(long millis)` 是一个让线程“休眠”指定时间的方法，单位为毫秒。

- **特点**：

  - **暂停线程指定时间后会自动恢复运行**。
  - **不释放锁**，如果线程在同步代码块或方法中调用 `sleep()`，其他线程无法访问被锁定的资源。
  - 是 `Thread` 类的 **静态方法**，所以调用时总是针对当前线程。

- **代码示例**：

  ```java
  public class SleepExample {
      public static void main(String[] args) {
          Thread thread = new Thread(() -> {
              synchronized (SleepExample.class) {
                  System.out.println("Thread starts sleeping...");
                  try {
                      Thread.sleep(3000); // 当前线程休眠 3 秒
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  System.out.println("Thread wakes up!");
              }
          });
          thread.start();
      }
  }
  ```

  **输出**：

  ```
  Thread starts sleeping...
  // (3 秒后)
  Thread wakes up!
  ```

  **注意**：

  - 在 `Thread.sleep()` 的休眠期间，线程持有的锁 **不会释放**，其他线程无法访问同步资源。

------



#### **2) `wait()`**

- **定义**：`wait()` 是 `Object` 类的实例方法，用于线程间的通信与协作。

- **特点**：

  - **当前线程调用 `wait()` 时会进入 等待状态，直到被其他线程通过 `notify()` 或 `notifyAll()` 唤醒。**
  - **会释放锁**，允许其他线程进入同步代码块。
  - 必须在同步代码块或同步方法中使用，否则会抛出 `IllegalMonitorStateException`。

- **代码示例**：

  ```java
  public class WaitExample {
      public static void main(String[] args) {
          Object lock = new Object();
  
          Thread thread1 = new Thread(() -> {
              synchronized (lock) {
                  try {
                      System.out.println("Thread 1 starts waiting...");
                      lock.wait(); // 释放锁并等待被唤醒
                      System.out.println("Thread 1 is notified!");
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
              }
          });
  
          Thread thread2 = new Thread(() -> {
              synchronized (lock) {
                  System.out.println("Thread 2 is notifying...");
                  lock.notify(); // 唤醒正在等待的线程
              }
          });
  
          thread1.start();
          try {
              Thread.sleep(1000); // 确保 thread1 先执行
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
          thread2.start();
      }
  }
  ```

  **输出**：

  ```
  Thread 1 starts waiting...
  Thread 2 is notifying...
  Thread 1 is notified!
  ```

  **注意**：

  - 调用 `wait()` 时，线程会立即 **释放锁**，进入等待状态。
  - 必须使用同步代码块，否则会抛出异常。

------



### 3. **主要区别**

| **比较点**         | **`sleep()`**                    | **`wait()`**                                     |
| ------------------ | -------------------------------- | ------------------------------------------------ |
| 所属类             | `Thread`                         | `Object`                                         |
| 是否释放锁         | **不释放锁**                     | **释放锁**                                       |
| 是否需要同步代码块 | 不需要，可以在任何地方调用       | 必须在同步代码块或方法中调用                     |
| 是否需要显式唤醒   | **不需要，休眠结束后自动恢复**   | **需要显式唤醒（通过 `notify` 或 `notifyAll`）** |
| 使用场景           | 控制线程的执行速度或模拟定时任务 | 线程间通信或协作                                 |
| 异常处理           | 抛出 `InterruptedException`      | 抛出 `InterruptedException`                      |

------



### 4. **实际应用场景**

- **`sleep()`**
  - 控制线程的执行间隔，如定时任务或模拟延迟。
  - 简单场景下暂停线程，无需考虑线程间通信。
- **`wait()`**
  - 用于线程间的协作和通信，典型应用是生产者-消费者模型。
  - 需要一个线程等待特定条件的达成，其他线程负责唤醒。

------



### 5. **总结**

- **`sleep()`** 是线程控制工具，适用于单线程场景。
- **`wait()`** 是线程间协作工具，适用于多线程通信场景。
- 如果需要高效的线程同步与通信，建议使用 `java.util.concurrent` 包中的工具类（如 `Condition`、`Semaphore` 等）。







<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0042.png?raw=true)

上图是由 Pic 生成的

关键词：Cute dolphins

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



