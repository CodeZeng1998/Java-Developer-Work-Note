# Java开发-面试题-0040说明 JVM 的一些常用参数

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**



> 注意：下列常用参数黑色加粗的是使用最频繁的几个参数之一。



JVM（Java Virtual Machine）的常用参数可以分为几类：内存相关参数、垃圾回收相关参数、性能优化参数、调试相关参数等。以下是一些常用的 JVM 参数说明：



## 1. 内存相关参数
这些参数用于配置 JVM 的堆内存、非堆内存、栈大小等。

* **-Xms**：设置 JVM 初始化堆内存大小。

​		示例：-Xms512m，表示 JVM 启动时分配 512 MB 的堆内存。

* **-Xmx**：设置 JVM 最大堆内存大小。

​		示例：-Xmx2g，表示 JVM 最大可以使用 2 GB 的堆内存。
* **-Xmn**：设置新生代内存大小。

​		示例：-Xmn256m，表示新生代的大小为 256 MB。
* **-XX:MetaspaceSize**：设置元空间（Metaspace）的初始大小。

​		示例：-XX:MetaspaceSize=128m，表示初始分配 128 MB 的元空间。
* **-XX:MaxMetaspaceSize**：设置元空间的最大值。

​		示例：-XX:MaxMetaspaceSize=512m，表示元空间最大可扩展到 512 MB。
* **-Xss**：设置每个线程的栈大小。

​		示例：-Xss1m，表示每个线程分配 1 MB 的栈内存。






## 2. 垃圾回收相关参数
这些参数用于调整 JVM 的垃圾回收策略和行为。

* **-XX:+UseG1GC**：启用 G1 垃圾回收器，这是现代 JVM 默认的 GC 垃圾回收器，适合低延迟的应用。

* -XX:+UseParallelGC：启用并行垃圾回收器，适合高吞吐量的场景。

* -XX:+UseConcMarkSweepGC：启用 CMS（并发标记清除）垃圾回收器，适合低延迟的应用场景（已被 G1GC 逐步替代）。

* -XX:MaxGCPauseMillis：设置最大 GC 暂停时间。

​		示例：-XX:MaxGCPauseMillis=200，表示最大 GC 暂停时间为 200 毫秒。
* -XX:GCTimeRatio：设置 GC 时间占应用时间的比率。

​		示例：-XX:GCTimeRatio=19，表示 GC 时间占比为 1/(1+19) = 5%。






## 3. 性能优化参数
用于提高应用的性能，包括即时编译器（JIT）相关的参数。

* -XX:+AggressiveOpts：启用一些 JVM 实验性的优化选项，这些选项可能在未来成为默认设置。

* -XX:+TieredCompilation：启用分层编译，分层编译会结合解释器和编译器的优点，提升应用启动速度和长期性能。

* -XX:CompileThreshold：设置方法编译的阈值，即方法被调用多少次后，JIT 开始编译该方法。

​		示例：-XX:CompileThreshold=1000，表示方法调用 1000 次后进行编译。






## 4. 调试相关参数
这些参数有助于调试和分析 JVM 应用程序。

* **-XX:+PrintGC**：输出垃圾回收的简要信息。

* **-XX:+PrintGCDetails**：输出垃圾回收的详细信息，包括每次垃圾回收的时间、回收了多少内存等。

* -XX:+PrintGCTimeStamps：输出垃圾回收的时间戳。

* **-XX:+HeapDumpOnOutOfMemoryError**：在发生 OOM（OutOfMemoryError）时生成堆转储文件，方便后续分析。

​		示例：-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/path/to/dump，指定 OOM 时生成的堆转储文件路径。
* -Xloggc：指定垃圾回收日志的输出路径。

​		示例：-Xloggc:/path/to/gc.log，将 GC 日志输出到指定文件。






## 5. 类加载相关参数
* -XX:+TraceClassLoading：用于跟踪类加载的过程，输出哪些类被加载以及类加载的时间。
* -XX:+TraceClassUnloading：用于跟踪类卸载的过程。
* -verbose:class：输出每次类加载时的详细信息，包括类的名称和加载器信息。



## 6. 其他常用参数
* **-server**：让 JVM 以服务器模式运行，适用于需要较高性能和长时间运行的服务器应用。相对 -client 模式，-server 会有更好的优化。

* **-D**：设置系统属性。

  

示例：-Dfile.encoding=UTF-8，表示设置文件的编码方式为 UTF-8。
这些 JVM 参数可以根据应用的实际需求进行调优。例如，对于需要大量内存的应用，可以增大堆内存的大小（-Xmx）；对于对延迟敏感的应用，可以调整垃圾回收策略和暂停时间（如 -XX:MaxGCPauseMillis）；对于调试内存泄漏问题，可以启用 -XX:+HeapDumpOnOutOfMemoryError 等参数。







<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0040.png?raw=true)

上图是由 Pic 生成的

关键词：Cute seals

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



