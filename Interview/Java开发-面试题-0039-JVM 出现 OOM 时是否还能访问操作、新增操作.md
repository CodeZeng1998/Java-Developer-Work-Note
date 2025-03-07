# Java开发-面试题-0039-JVM 出现 OOM 时是否还能访问操作、新增操作

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**

<br/>

JVM发生`OutOfMemoryError`（简称 OOM）时，是否还能继续访问或新增操作，取决于OOM发生的**具体场景**、**错误类型**以及**系统的状态**。下面详细说明几种常见的 OOM 情况及其影响。

<br/>

### 1. **常见的 OOM 类型**

在 JVM 中，`OutOfMemoryError` 可以在不同内存区域发生，最常见的几种情况包括：

- **堆内存（Heap Space）不足**
- **方法区/元空间（Metaspace）不足**
- **直接内存（Direct Memory）不足**
- **GC Overhead Limit Exceeded**
- **虚拟机栈溢出（Stack Overflow）**

不同的 OOM 类型对程序的影响不同，具体说明如下：

<br/>

### 2. **堆内存不足 (Java Heap Space)**

堆内存不足的 OOM 错误是最常见的一种。当应用程序试图分配内存给对象，但堆空间已满，且垃圾回收器无法释放足够的空间时，就会抛出 `OutOfMemoryError: Java heap space`。

<br/>

#### 影响：

- **对象分配失败**：一旦发生堆内存 OOM，**无法再为新对象分配内存**。这意味着，任何需要在堆上分配内存的操作（如创建新对象、加载类等）都会失败。
- **继续访问现有对象**：尽管新对象无法分配内存，但**现有的对象依然可以访问**。如果堆上的数据结构和状态没有损坏，可以继续读取现有数据，但由于不能新增对象，程序可能无法执行进一步的操作，进而导致程序不可用。
- **程序可能不崩溃**：如果代码捕获了 OOM 异常，程序可能不会立刻崩溃，而是进入受限的运行状态。此时，某些操作可能仍然可以执行，但大多数依赖对象创建的功能会中断。

<br/>

#### 示例：

```java
try {
    List<String> list = new ArrayList<>();
    while (true) {
        list.add("New Object");  // 不断新增对象，触发堆内存 OOM
    }
} catch (OutOfMemoryError e) {
    // 可以继续操作已经存在的对象，但无法新增对象
    System.out.println("堆内存溢出！");
}
```

<br/>

### 3. **方法区/元空间不足 (Metaspace)**

方法区（JDK 8 之前）或元空间（JDK 8 之后）是 JVM 用来存储类的元数据的区域。如果加载过多类或类的元数据非常大，可能会导致元空间 OOM，抛出 `OutOfMemoryError: Metaspace`。

<br/>

#### 影响：

- **新类无法加载**：元空间 OOM 的直接影响是无法再加载新类，这意味着任何动态生成类（如使用反射、代理等技术）会失败。
- **现有类不受影响**：已经加载的类和它们的实例可以继续使用，因此，如果应用没有动态生成新类的需求，程序仍然可以部分运行。

<br/>

#### 示例：

```
java复制代码try {
    while (true) {
        ClassLoader loader = new URLClassLoader(new URL[]{});
        loader.loadClass("com.example.SomeClass");  // 触发类加载，导致元空间 OOM
    }
} catch (OutOfMemoryError e) {
    System.out.println("元空间溢出！");
}
```

<br/>

### 4. **直接内存不足 (Direct Buffer Memory)**

JVM 的直接内存通常用于 NIO（New I/O）中的 `ByteBuffer`，这是堆外内存。如果直接内存耗尽，JVM 会抛出 `OutOfMemoryError: Direct buffer memory`。

<br/>

#### 影响：

- **无法分配新的直接内存**：无法为 NIO 操作分配新的直接缓冲区，但不影响堆上的对象操作。
- **继续访问现有的对象**：和堆内存 OOM 类似，现有的 Java 对象依然可以正常访问，只是依赖直接内存的操作（如大文件或网络通信等 NIO 操作）会失败。

<br/>

### 5. **GC Overhead Limit Exceeded**

当 JVM 花费了大量时间进行垃圾回收但回收的内存不足时，会抛出 `OutOfMemoryError: GC overhead limit exceeded`。这意味着 JVM 频繁进行垃圾回收却没有效果。

<br/>

#### 影响：

- **程序极度缓慢**：虽然在发生此类 OOM 时，JVM 还没有完全耗尽内存，但垃圾回收占用了过多的 CPU 时间，导致程序几乎无法继续运行。
- **可能仍可访问对象**：现有的对象可以继续访问，但系统几乎无法进行有意义的操作，因为大部分时间都被用来进行无效的 GC。

<br/>

### 6. **虚拟机栈溢出（Stack Overflow）**

虚拟机栈溢出是由于递归过深或过多线程导致的栈空间耗尽，可能会抛出 `OutOfMemoryError: unable to create new native thread` 或 `StackOverflowError`。

<br/>

#### 影响：

- **无法创建新线程**：在栈溢出或线程数量过多时，JVM 无法再创建新线程，这会影响到并发或多线程的应用。
- **继续执行现有线程**：虽然新线程无法创建，但现有线程依然可以继续执行，程序部分功能依然可以正常运行。

<br/>

#### 7. **OOM 的捕获与处理**

虽然 OOM 是一种严重错误，但它是一个**`Error`**，并不意味着程序一定会立刻崩溃。如果 OOM 发生在某个子模块，并且该模块的异常被捕获，程序可以继续执行剩余的部分，但整体性能和功能会受到极大的限制。

```java
try {
    // 可能导致 OOM 的代码
} catch (OutOfMemoryError e) {
    // 处理 OOM 错误，比如日志记录、释放资源等
    System.out.println("内存不足，无法继续执行部分操作！");
}
```

<br/>

#### 总结：

- 能否继续访问或新增操作

  取决于 OOM 的类型和发生的区域。

  - 堆内存 OOM：现有对象可以访问，但不能新增对象。
  - 方法区/元空间 OOM：无法加载新类，但现有类可以继续使用。
  - 直接内存 OOM：影响 NIO 相关操作，但不影响堆内存操作。
  - GC Overhead Limit：程序极度缓慢，但现有对象仍能访问。
  - 栈溢出 OOM：无法创建新线程，但现有线程可以继续执行。

- **在特定条件下**，如果 OOM 被捕获，程序可以部分恢复，但性能和可用性会受到严重影响。





<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0039.png?raw=true)

上图是由 Pic 生成的

关键词：Black Thursday turn off the lights and eat noodles

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



