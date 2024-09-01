# Java开发-面试题-0034- Java 的四种引用

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**





Java 中的引用类型可以分为四种：**强引用**、**软引用**、**弱引用**、和**虚引用**。它们在垃圾回收机制中的表现和用途各不相同。以下是对每种引用类型的详细说明：

### 1. 强引用（Strong Reference）

强引用是 Java 中最常见的引用类型。==只要一个对象有强引用存在，垃圾回收器就不会回收该对象，即使在内存紧张的情况下==。

#### 特点：

- 默认情况下，所有创建的对象都是强引用。
- 强引用会阻止垃圾回收，当没有其他引用时才会被回收。

#### 示例：

```java
Object obj = new Object(); // 强引用
```

在上述代码中，`obj` 是一个强引用，指向一个 `Object` 对象。只要 `obj` 存在，垃圾回收器就不会回收这个对象。

### 2. 软引用（Soft Reference）

软引用是一种相对弱的引用类型。它的特点是==在内存不足时，垃圾回收器会尝试回收软引用指向的对象。如果内存足够，则不会回收==。软引用通常用于实现内存敏感的缓存。

#### 特点：

- 当系统内存不足时，软引用对象会被回收。
- 软引用可以通过 `SoftReference` 类来实现。

#### 示例：

```java
SoftReference<Object> softRef = new SoftReference<>(new Object());
Object obj = softRef.get();
```

### 3. 弱引用（Weak Reference）

弱引用比软引用更弱，意味着它==在下一次垃圾回收时就会被回收，无论内存是否充足==。弱引用常用于实现某些数据结构，如 `WeakHashMap`。

#### 特点：

- 弱引用对象在下一次垃圾回收时无条件被回收。
- 弱引用可以通过 `WeakReference` 类来实现。

#### 示例：

```java
WeakReference<Object> weakRef = new WeakReference<>(new Object());
Object obj = weakRef.get();
```

### 4. 虚引用（Phantom Reference）

==虚引用是最弱的引用类型。它与其他引用类型不同，虚引用的存在不会影响对象的生命周期。虚引用的主要作用是跟踪对象的回收状态，可以与引用队列（`ReferenceQueue`）一起使用，用于在对象被垃圾回收后执行一些清理操作。==

#### 特点：

- 虚引用对象不能通过 `get()` 方法访问。
- 需要与 `ReferenceQueue` 一起使用，用于在对象被回收后处理一些额外操作。

#### 示例：

```java
ReferenceQueue<Object> queue = new ReferenceQueue<>();
PhantomReference<Object> phantomRef = new PhantomReference<>(new Object(), queue);
```

### 5. 总结

- **强引用**：最常见的引用类型，不会被垃圾回收。
- **软引用**：用于实现缓存，在内存不足时回收。
- **弱引用**：常用于数据结构和内存敏感的场景，在下次垃圾回收时回收。
- **虚引用**：用于跟踪对象的回收状态，通常与 `ReferenceQueue` 一起使用。

这四种引用类型为 Java 开发者提供了不同的工具来控制对象的生命周期，优化内存使用和管理资源。



<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0034.png?raw=true)

上图是由 Pic 生成的

关键词：Hulk

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



